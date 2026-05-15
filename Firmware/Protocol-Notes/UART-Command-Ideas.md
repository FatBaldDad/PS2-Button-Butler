# UART Command Ideas

## Overview

This document collects early UART command ideas for the **FBD PS2 Button Butler** project.

UART will likely be one of the first communication methods used between the Button Butler interposer, touchscreen controller, debug adapter, I/O expansion board, and future internal PS2 mod-control hardware.

These commands are early planning ideas.

They are not final firmware requirements yet.

---

## Purpose

The purpose of the UART command system is to give Button Butler a simple way to:

- Report button events
- Report lid switch state
- Report input and output states
- Receive touchscreen requests
- Receive service/debug commands
- Control simple outputs
- Confirm commands
- Reject unsafe commands
- Report firmware version
- Support future expansion

The first UART protocol should be simple enough to test with a USB-to-serial adapter and a serial terminal.

---

## Design Goals

The UART command system should be:

1. Simple
2. Human-readable
3. Easy to type manually
4. Easy to parse on a small MCU
5. Easy to debug
6. Safe by default
7. Consistent in responses
8. Expandable for touchscreen use
9. Useful for service mode
10. Small enough for early PIC firmware

The first goal is not speed.

The first goal is predictable communication.

---

## UART Role In Button Butler

UART may be used for several roles.

Possible UART roles:

| UART Role | Purpose |
| --- | --- |
| Debug UART | Send firmware messages to a serial terminal |
| Touchscreen UART | Communicate between touchscreen and interposer |
| Service UART | Allow installer commands and diagnostics |
| I/O UART | Communicate with an I/O expansion board |
| Device UART | Talk to optional devices if supported |
| Muxed UART | Switch between low-priority service devices |

The first prototype may only need a debug UART.

---

## Recommended First UART Use

The first UART use should be simple debug output.

Recommended first messages:

- Firmware boot message
- Firmware ready message
- Button pressed
- Button released
- Short press detected
- Long press detected
- Lid open
- Lid closed
- Output changed
- Error message

This allows the button behavior to be tested before any touchscreen is involved.

---

## Human-Readable Protocol

The early protocol should use readable text.

Example messages:

```text
EVT BOOT
EVT READY
EVT BTN_PRESS
EVT BTN_RELEASE
EVT SHORT_PRESS
EVT LID_OPEN
STATE OUT1=OFF OUT2=ON
OK OUT1 ON
ERR UNKNOWN_COMMAND
```

Readable messages make it easier to test, log, and debug.

---

## Why Not Binary First?

A binary protocol may be useful later, but it is not ideal for early development.

Human-readable text is better early because it is:

- Easier to test manually
- Easier to read in logs
- Easier to explain
- Easier to document
- Easier to debug with simple tools
- Easier to change while experimenting

A binary protocol can be considered later only if needed.

---

## Possible UART Settings

Initial UART settings may be:

| Setting | Suggested Value |
| --- | --- |
| Baud rate | 115200 or 9600 |
| Data bits | 8 |
| Parity | None |
| Stop bits | 1 |
| Flow control | None |
| Logic level | Match hardware voltage |
| Line ending | LF or CRLF |

The final baud rate should be selected after the MCU and clock source are tested.

---

## Baud Rate Notes

A higher baud rate such as `115200` is convenient for development.

A lower baud rate such as `9600` may be more forgiving if:

- The MCU clock is not very accurate
- The internal oscillator has too much error
- Long wires are used
- Noise is a problem
- Early firmware is very simple

The best baud rate should be selected by testing.

---

## Logic Level Notes

UART logic voltage must match the hardware.

Possible logic levels:

- 3.3V UART
- 5V UART
- USB-to-serial adapter logic level
- Touchscreen controller logic level
- PIC logic level

Important rules:

- Do not feed 5V UART into a 3.3V-only input.
- Share ground between UART devices.
- Cross TX and RX.
- Avoid backfeeding unpowered boards through TX/RX lines.
- Use level shifting if needed.

---

## UART Wiring

Typical UART wiring:

| Device A | Device B |
| --- | --- |
| TX | RX |
| RX | TX |
| GND | GND |

Optional signals:

| Signal | Purpose |
| --- | --- |
| VCC reference | Lets the adapter know target voltage, if needed |
| EN | Enable or wake signal |
| RTS / CTS | Hardware flow control, not planned for early firmware |
| RESET | Optional reset control |
| BOOT | Optional bootloader control |

For early Button Butler firmware, TX, RX, and GND should be enough.

---

## Important UART Bus Warning

Standard UART is normally point-to-point.

Do not directly tie multiple TX pins together.

Bad UART wiring can cause:

- Signal contention
- Garbled data
- Unreliable communication
- Possible damage
- Devices talking over each other

If multiple UART devices are needed, use a proper design such as:

- Separate hardware UARTs
- UART mux
- Tri-state buffers
- Open-drain style communication if designed correctly
- A different bus such as I2C or RS-485 where appropriate

---

## Basic Message Types

The UART protocol may include several message types.

| Message Type | Prefix | Purpose |
| --- | --- | --- |
| Event | `EVT` | Reports something that happened |
| Command | No required prefix | Requests an action or state |
| Response | `OK`, `ERR`, etc. | Confirms or rejects a command |
| State Report | `STATE` | Reports current system state |
| Value Report | `VALUE` | Reports one requested value |
| Version Report | `VERSION` | Reports firmware/hardware version |
| Error Report | `ERR` | Reports command or system error |

Using consistent prefixes makes logs easier to read.

---

## Event Messages

Event messages are sent automatically when something changes.

Possible event messages:

| Event | Meaning |
| --- | --- |
| `EVT BOOT` | Firmware started |
| `EVT READY` | Firmware initialized and ready |
| `EVT BTN_PRESS` | Button was pressed |
| `EVT BTN_RELEASE` | Button was released |
| `EVT SHORT_PRESS` | Short press detected |
| `EVT LONG_PRESS` | Long press detected |
| `EVT DOUBLE_PRESS` | Double press detected |
| `EVT LID_OPEN` | Lid switch changed to open |
| `EVT LID_CLOSED` | Lid switch changed to closed |
| `EVT OUT1 ON` | Output 1 turned on |
| `EVT OUT1 OFF` | Output 1 turned off |
| `EVT SERVICE_ON` | Service mode entered |
| `EVT SERVICE_OFF` | Service mode exited |
| `EVT SAFE_MODE` | Safe mode entered |
| `EVT ERROR` | Error occurred |

Events are useful for debugging and touchscreen status updates.

---

## Command Messages

Command messages request an action or information.

Examples:

```text
PING
STATUS
BTN?
LID?
OUT1 ON
OUT1 OFF
OUT1 TOGGLE
VERSION?
```

Commands should be short and predictable.

---

## Response Messages

Every command should receive a response.

Possible response prefixes:

| Response | Meaning |
| --- | --- |
| `OK` | Command accepted |
| `ERR` | Command failed or was rejected |
| `UNKNOWN` | Command was not recognized |
| `LOCKED` | Command is protected |
| `UNSAFE` | Command was blocked for safety |
| `BUSY` | Device is busy |
| `VALUE` | Requested value follows |
| `STATE` | Requested state follows |
| `VERSION` | Version information follows |

The touchscreen should not assume a command worked unless it receives a valid response.

---

## Basic Command List

Minimum useful UART commands:

| Command | Purpose |
| --- | --- |
| `PING` | Check communication |
| `STATUS` | Request current state |
| `BTN?` | Read button state |
| `LID?` | Read lid switch state |
| `OUT1 ON` | Turn output 1 on |
| `OUT1 OFF` | Turn output 1 off |
| `OUT1 TOGGLE` | Toggle output 1 |
| `OUT1?` | Read output 1 state |
| `VERSION?` | Read firmware version |

This command set is enough for early bench testing.

---

## Communication Check Commands

Communication check commands confirm the device is alive.

| Command | Response | Meaning |
| --- | --- | --- |
| `PING` | `OK PONG` | Device is responding |
| `HELLO` | `OK HELLO` | Optional startup handshake |
| `READY?` | `VALUE READY YES` | Device reports ready state |

Example:

```text
> PING
OK PONG
```

---

## Status Commands

Status commands return system state.

| Command | Purpose |
| --- | --- |
| `STATUS` | Return general status |
| `STATE?` | Return general state |
| `INPUTS?` | Return input states |
| `OUTPUTS?` | Return output states |
| `MODS?` | Return mod-related states if available |
| `POWER?` | Return power-related states if available |
| `SAFE?` | Return safe mode state |
| `SERVICE?` | Return service mode state |

Example:

```text
> STATUS
STATE SYSTEM READY=YES SERVICE=OFF SAFE=OFF PROFILE=BASIC
```

---

## Button Commands

Button commands report button-related information.

| Command | Purpose |
| --- | --- |
| `BTN?` | Read physical button state |
| `BTN_TIME?` | Read last press time if supported |
| `BTN_MODE?` | Read current button behavior mode |
| `BTN_PROFILE?` | Read active button profile |

Possible responses:

```text
VALUE BTN RELEASED
VALUE BTN PRESSED
VALUE BTN SHORT_PRESS
VALUE BTN LONG_PRESS
```

---

## Lid Switch Commands

Lid switch commands report lid state.

| Command | Purpose |
| --- | --- |
| `LID?` | Read lid switch state |
| `LID_MODE?` | Read lid behavior mode |
| `LID_EVENT?` | Read last lid event if supported |

Possible responses:

```text
VALUE LID OPEN
VALUE LID CLOSED
VALUE LID UNKNOWN
```

---

## Output Commands

Output commands control or read outputs.

| Command | Purpose |
| --- | --- |
| `OUT1 ON` | Turn output 1 on |
| `OUT1 OFF` | Turn output 1 off |
| `OUT1 TOGGLE` | Toggle output 1 |
| `OUT1 PULSE` | Pulse output 1 using default pulse time |
| `OUT1 PULSE 100` | Pulse output 1 for 100 ms |
| `OUT1?` | Read output 1 state |
| `OUT2 ON` | Turn output 2 on |
| `OUT2 OFF` | Turn output 2 off |
| `OUT2 TOGGLE` | Toggle output 2 |
| `OUT2?` | Read output 2 state |

Example:

```text
> OUT1 ON
OK OUT1 ON

> OUT1?
VALUE OUT1 ON
```

---

## Pulse Commands

Pulse commands briefly activate an output.

Possible uses:

- Reset pulse
- Pairing trigger
- Chime trigger
- Wake signal
- Button simulation

Example commands:

```text
OUT1 PULSE
OUT1 PULSE 100
BT_PAIR PULSE
BLUERETRO_RESET PULSE
BUZZER PULSE
```

Pulse commands should have safety limits.

---

## Pulse Safety Rules

Pulse commands should follow strict limits.

Recommended rules:

- Use a safe default pulse length.
- Enforce a maximum pulse length.
- Reject pulse values that are too long.
- Reject pulse commands during startup.
- Reject pulse commands if output is locked.
- Do not block button handling while pulsing.
- Report when a pulse is accepted or rejected.

Possible responses:

```text
OK OUT1 PULSE 100
UNSAFE OUT1 PULSE
LOCKED OUT1 PULSE
```

---

## Service Mode Commands

Service mode commands are for testing and installation.

| Command | Purpose |
| --- | --- |
| `SERVICE ON` | Enter service mode |
| `SERVICE OFF` | Exit service mode |
| `SERVICE?` | Read service mode state |
| `READ ALL` | Read all known inputs and outputs |
| `TEST OUT1` | Test output 1 |
| `TEST OUT2` | Test output 2 |
| `SAFE` | Enter safe mode |
| `PASS` | Force pass-through mode if supported |
| `DEFAULTS` | Restore safe defaults if supported |

Service mode should be protected before customer use.

---

## Safe Mode Commands

Safe mode is used for conservative behavior.

| Command | Purpose |
| --- | --- |
| `SAFE` | Enter safe mode |
| `SAFE ON` | Enter safe mode |
| `SAFE OFF` | Exit safe mode if allowed |
| `SAFE?` | Read safe mode state |
| `OUTPUTS SAFE` | Restore output safe states |
| `ALL OFF` | Turn all noncritical outputs off |

Example:

```text
> SAFE
OK SAFE ON

> SAFE?
VALUE SAFE ON
```

Safe mode should not make the console impossible to operate.

---

## Pass-Through Commands

Pass-through commands may force normal button behavior.

| Command | Purpose |
| --- | --- |
| `PASS` | Force pass-through mode if supported |
| `PASS ON` | Enable pass-through behavior |
| `PASS OFF` | Disable forced pass-through if allowed |
| `PASS?` | Read pass-through state |

Example:

```text
> PASS ON
OK PASS ON
```

Pass-through mode is useful for testing and recovery.

---

## Version Commands

Version commands identify the firmware and hardware.

| Command | Purpose |
| --- | --- |
| `VERSION?` | Read firmware version |
| `HW?` | Read hardware revision if known |
| `FW?` | Read firmware version |
| `PROTOCOL?` | Read protocol version |
| `ID?` | Read device ID |
| `FEATURES?` | List supported features if implemented |

Example response:

```text
VERSION BB_INTERPOSER FW=v0.1.0 HW=PROTO_A PROTO=v0.1 PROFILE=BASIC
```

---

## Profile Commands

Profiles may change behavior by build type.

| Command | Purpose |
| --- | --- |
| `PROFILE?` | Read active profile |
| `PROFILE BASIC` | Select basic profile if supported |
| `PROFILE CHIPSLAYER` | Select ChipSlayer profile if supported |
| `PROFILE MODCHIP` | Select modchip profile if supported |
| `PROFILE BT_AUDIO` | Select Bluetooth audio profile if supported |
| `PROFILE PREMIUM` | Select premium touchscreen profile if supported |
| `PROFILE SERVICE` | Select service profile if supported |
| `PROFILE SAVE` | Save profile if persistent settings are supported |

Early firmware may only have one hardcoded profile.

---

## Feature Alias Commands

Feature aliases make commands easier to read.

Possible aliases:

| Alias | Possible Target |
| --- | --- |
| `CHIPSLAYER` | ChipSlayer control output |
| `MODCHIP` | Modchip control output |
| `BT_AUDIO` | Bluetooth audio enable output |
| `BT_PAIR` | Bluetooth pairing trigger output |
| `BLUERETRO` | BlueRetro enable output |
| `BLUERETRO_RESET` | BlueRetro reset output |
| `RGB` | RGB lighting output |
| `VIDEO_PWR` | Video board power enable |
| `ROUTER` | Internal router power or reset |
| `BUZZER` | Chime or buzzer output |

Aliases should map to real hardware in the active profile.

---

## ChipSlayer Commands

Possible ChipSlayer commands:

| Command | Purpose |
| --- | --- |
| `CHIPSLAYER ON` | Enable ChipSlayer |
| `CHIPSLAYER OFF` | Disable ChipSlayer |
| `CHIPSLAYER TOGGLE` | Toggle ChipSlayer state |
| `CHIPSLAYER?` | Read ChipSlayer state |
| `CHIPSLAYER DEFAULT` | Restore profile default |
| `CHIPSLAYER LOCK` | Lock changes |
| `CHIPSLAYER UNLOCK` | Unlock changes if allowed |

Example:

```text
> CHIPSLAYER TOGGLE
OK CHIPSLAYER ON
```

---

## Modchip Commands

Possible modchip commands:

| Command | Purpose |
| --- | --- |
| `MODCHIP ON` | Enable modchip |
| `MODCHIP OFF` | Disable modchip |
| `MODCHIP TOGGLE` | Toggle modchip state |
| `MODCHIP?` | Read modchip state if known |
| `MODCHIP NEXTBOOT OFF` | Disable modchip for next boot if supported |
| `MODCHIP DEFAULT` | Restore profile default |
| `MODCHIP LOCK` | Lock modchip changes |
| `MODCHIP UNLOCK` | Unlock modchip changes if allowed |

Modchip commands should likely require service mode or confirmation.

---

## Bluetooth Audio Commands

Possible Bluetooth audio commands:

| Command | Purpose |
| --- | --- |
| `BT_AUDIO ON` | Enable Bluetooth audio |
| `BT_AUDIO OFF` | Disable Bluetooth audio |
| `BT_AUDIO TOGGLE` | Toggle Bluetooth audio |
| `BT_AUDIO?` | Read Bluetooth audio state |
| `BT_PAIR PULSE` | Trigger pairing mode |
| `BT_RESET PULSE` | Reset Bluetooth audio board |
| `BT_STATUS?` | Read Bluetooth status input if available |

Example:

```text
> BT_PAIR PULSE
OK BT_PAIR PULSE 500
```

---

## BlueRetro Commands

Possible BlueRetro commands:

| Command | Purpose |
| --- | --- |
| `BLUERETRO ON` | Enable BlueRetro |
| `BLUERETRO OFF` | Disable BlueRetro |
| `BLUERETRO TOGGLE` | Toggle BlueRetro enable |
| `BLUERETRO RESET` | Reset BlueRetro |
| `BR_CH1 ON` | Enable channel 1 if supported |
| `BR_CH1 OFF` | Disable channel 1 if supported |
| `BR_CH2 ON` | Enable channel 2 if supported |
| `BR_CH2 OFF` | Disable channel 2 if supported |
| `BLUERETRO?` | Read BlueRetro state if available |

Early support may only use simple enable and reset controls.

---

## RGB Commands

Possible RGB commands:

| Command | Purpose |
| --- | --- |
| `RGB ON` | Turn RGB lighting on |
| `RGB OFF` | Turn RGB lighting off |
| `RGB TOGGLE` | Toggle RGB lighting |
| `RGB?` | Read RGB state |
| `RGB MODE 1` | Select mode 1 if supported |
| `RGB MODE 2` | Select mode 2 if supported |
| `RGB BRIGHT 50` | Set brightness if supported |
| `RGB DEFAULT` | Restore default lighting behavior |

Early support should start with simple on/off control.

---

## Video Power Commands

Possible video power commands:

| Command | Purpose |
| --- | --- |
| `VIDEO_PWR ON` | Enable video board power |
| `VIDEO_PWR OFF` | Disable video board power |
| `VIDEO_PWR TOGGLE` | Toggle video power |
| `VIDEO_PWR?` | Read video power state |
| `VIDEO_AUTO ON` | Enable automatic video power management |
| `VIDEO_AUTO OFF` | Disable automatic video power management |
| `VIDEO_PG?` | Read video power-good input if available |

Video power commands should be protected because they can blank the display.

---

## Router Commands

Possible internal router commands:

| Command | Purpose |
| --- | --- |
| `ROUTER ON` | Enable router power |
| `ROUTER OFF` | Disable router power |
| `ROUTER RESET` | Restart router |
| `ROUTER?` | Read router state |
| `ROUTER_READY?` | Read router ready input if available |
| `ROUTER WAIT` | Wait for ready state if supported |

Router commands are useful for internal network-loading builds.

---

## Buzzer Or Chime Commands

Possible buzzer or chime commands:

| Command | Purpose |
| --- | --- |
| `BUZZER PULSE` | Trigger a default beep |
| `BUZZER PULSE 100` | Trigger a 100 ms beep |
| `CHIME STARTUP` | Play startup chime if supported |
| `CHIME ERROR` | Play error chime if supported |
| `CHIME ON` | Enable chime behavior |
| `CHIME OFF` | Disable chime behavior |
| `CHIME?` | Read chime setting |

Chime commands should be optional.

---

## State Report Examples

General state:

```text
STATE SYSTEM READY=YES SAFE=OFF SERVICE=OFF PROFILE=BASIC
```

Input state:

```text
STATE INPUT BTN=RELEASED LID=CLOSED SERVICE=OFF
```

Output state:

```text
STATE OUTPUT OUT1=OFF OUT2=ON
```

Mod state:

```text
STATE MODS CHIPSLAYER=ON MODCHIP=OFF BT_AUDIO=OFF RGB=ON
```

Power state:

```text
STATE POWER VIDEO_PWR=ON ROUTER=OFF TOUCHSCREEN=ON
```

---

## Value Report Examples

Single value examples:

```text
VALUE BTN PRESSED
VALUE BTN RELEASED
VALUE LID OPEN
VALUE LID CLOSED
VALUE OUT1 ON
VALUE OUT1 OFF
VALUE SAFE OFF
VALUE SERVICE ON
```

Value reports should be simple and easy to parse.

---

## Error Response Examples

Unknown command:

```text
UNKNOWN RGB SPARKLE
```

Invalid value:

```text
ERR INVALID_VALUE OUT1 MAYBE
```

Locked command:

```text
LOCKED MODCHIP OFF
```

Unsafe command:

```text
UNSAFE VIDEO_PWR OFF
```

Busy command:

```text
BUSY OUT1 PULSE
```

Unavailable feature:

```text
ERR UNAVAILABLE BLUERETRO
```

---

## Error Codes

Short error codes may be added later.

Possible error codes:

| Code | Meaning |
| --- | --- |
| `E01` | Unknown command |
| `E02` | Invalid target |
| `E03` | Invalid value |
| `E04` | Command locked |
| `E05` | Unsafe command |
| `E06` | Output unavailable |
| `E07` | Input unavailable |
| `E08` | Device busy |
| `E09` | Profile mismatch |
| `E10` | Hardware fault |
| `E11` | Buffer overflow or command too long |
| `E12` | Service mode required |

Example:

```text
ERR E04 MODCHIP OFF LOCKED
```

Readable errors should be preferred during early development.

---

## Device Targeting

Early firmware may not need target prefixes.

If multiple devices are connected later, target prefixes may help.

Possible targets:

| Target | Meaning |
| --- | --- |
| `BB` | Button Butler interposer |
| `IO` | I/O expansion board |
| `TS` | Touchscreen controller |
| `PM` | Power management board |
| `BT` | Bluetooth audio controller |
| `BR` | BlueRetro-related controller |
| `SD` | SD2PSX-related controller |
| `VID` | Video power controller |
| `RGB` | Lighting controller |

Example targeted commands:

```text
BB STATUS
IO OUT1 ON
BT PAIR
VID POWER OFF
RGB OFF
```

Targeting can be added after simple commands work.

---

## Touchscreen Command Flow

The touchscreen should request actions and wait for confirmation.

Example flow:

```text
TS -> BB: STATUS
BB -> TS: STATE MODS CHIPSLAYER=OFF BT_AUDIO=OFF RGB=ON

TS -> BB: CHIPSLAYER ON
BB -> TS: OK CHIPSLAYER ON
BB -> TS: EVT CHIPSLAYER ON
```

The touchscreen should not update its display as if the command worked until it receives confirmation.

---

## Service Command Flow

Example service session:

```text
> SERVICE ON
OK SERVICE ON

> READ ALL
STATE INPUT BTN=RELEASED LID=OPEN SERVICE=ON
STATE OUTPUT OUT1=OFF OUT2=OFF

> TEST OUT1
OK OUT1 PULSE 100

> SERVICE OFF
OK SERVICE OFF
```

Service mode should clearly show when it is active.

---

## Early Bench Test Session

Example early UART test:

```text
> PING
OK PONG

> VERSION?
VERSION BB_INTERPOSER FW=v0.1.0 HW=PROTO_A

> STATUS
STATE SYSTEM READY=YES SAFE=OFF SERVICE=OFF PROFILE=BASIC

> BTN?
VALUE BTN RELEASED

> LID?
VALUE LID CLOSED

> OUT1 ON
OK OUT1 ON

> OUT1?
VALUE OUT1 ON

> OUT1 OFF
OK OUT1 OFF
```

This kind of session is useful during firmware bring-up.

---

## Startup Messages

Startup messages should be predictable.

Possible startup sequence:

```text
EVT BOOT
VERSION BB_INTERPOSER FW=v0.1.0 HW=PROTO_A
EVT READY
STATE SYSTEM READY=YES SAFE=OFF SERVICE=OFF PROFILE=BASIC
```

The touchscreen should wait for `EVT READY` before sending risky commands.

---

## Heartbeat Messages

Heartbeat messages may be useful later.

Possible heartbeat commands:

| Message | Purpose |
| --- | --- |
| `PING` | Ask if device is responding |
| `OK PONG` | Response to ping |
| `EVT HEARTBEAT` | Optional periodic alive message |

Heartbeat should not be required for basic button behavior.

The interposer should work even if the touchscreen disconnects.

---

## Feature Discovery Commands

Feature discovery may help the touchscreen hide unused pages.

Possible commands:

| Command | Purpose |
| --- | --- |
| `FEATURES?` | List available features |
| `OUTPUTS?` | List available outputs |
| `INPUTS?` | List available inputs |
| `ALIASES?` | List supported aliases |
| `CAPS?` | List capabilities |

Example response:

```text
STATE FEATURES BTN LID OUT1 OUT2 CHIPSLAYER RGB SERVICE
```

Early firmware may not need feature discovery.

---

## Command Parsing Rules

Early parsing rules should be simple.

Suggested rules:

- Commands are line-based.
- Commands end with LF or CRLF.
- Commands are preferably uppercase.
- Parser may accept lowercase if space allows.
- Extra spaces should be ignored if practical.
- Empty lines should be ignored.
- Unknown commands should return `UNKNOWN`.
- Overly long commands should return an error or be ignored safely.

Small PICs may need a very simple parser.

---

## Command Length

Commands should be short.

Recommended early limits:

| Item | Suggested Limit |
| --- | --- |
| Command line length | 32 characters |
| Response length | 64 characters |
| Token count | 3 to 5 tokens |
| Output number range | Only implemented outputs |
| Pulse value | Limited by firmware |

The exact limits depend on MCU RAM.

---

## Line Ending Rules

Recommended line ending support:

| Line Ending | Support |
| --- | --- |
| LF | Preferred |
| CRLF | Should be accepted |
| CR | Optional |

Firmware should be tolerant during development.

---

## Case Sensitivity

For development, case-insensitive commands are convenient.

Examples that could be accepted:

```text
STATUS
status
Status
```

If firmware space is limited, uppercase-only commands are acceptable.

If uppercase-only is used, document it clearly.

---

## Safety Levels

Commands can be grouped by safety level.

| Safety Level | Description |
| --- | --- |
| Safe | Allowed in normal mode |
| Limited | Allowed only in certain profiles |
| Protected | Requires confirmation or unlock |
| Service Only | Requires service mode or jumper |
| Disabled | Not available in current firmware |

---

## Example Safety Table

| Command | Safety Level | Notes |
| --- | --- | --- |
| `PING` | Safe | Always allowed |
| `STATUS` | Safe | Always allowed |
| `BTN?` | Safe | Always allowed |
| `LID?` | Safe | Always allowed |
| `OUT1?` | Safe | Always allowed |
| `OUT1 ON` | Limited | Depends on profile |
| `RGB TOGGLE` | Limited | Low risk |
| `BT_PAIR PULSE` | Protected | Avoid accidental pairing |
| `CHIPSLAYER TOGGLE` | Protected | Depends on build |
| `MODCHIP OFF` | Service Only | Affects boot behavior |
| `VIDEO_PWR OFF` | Protected | Can blank display |
| `DEFAULTS` | Service Only | Changes behavior |
| `PROFILE SAVE` | Service Only | Persistent setting |

This table should be adjusted as the project develops.

---

## Lockout Rules

Risky commands should be locked unless conditions are met.

Possible unlock conditions:

- Service mode active
- Lid switch open
- Physical service jumper installed
- Touchscreen confirmation given
- Touchscreen PIN entered
- Startup-only window active
- UART unlock command accepted during bench testing

The lockout method should match the risk.

---

## Rate Limiting

Some commands should not be repeated too quickly.

Rate-limited commands may include:

- Reset pulse
- Power request
- Bluetooth pairing
- BlueRetro reset
- Router reset
- Video power toggle
- Modchip toggle
- Save settings

Possible responses:

```text
BUSY BT_PAIR
LOCKED RATE_LIMIT
```

---

## Persistent Settings Commands

Persistent settings should be delayed until the system is stable.

Possible future commands:

| Command | Purpose |
| --- | --- |
| `SAVE` | Save current settings |
| `SAVE PROFILE` | Save current profile |
| `LOAD` | Load saved settings |
| `LOAD DEFAULTS` | Load factory defaults |
| `ERASE SETTINGS` | Clear stored settings |

Stored settings should never make the console unusable.

A recovery method should exist before persistent settings are added.

---

## Commands To Implement First

The first firmware should implement only a small set.

Recommended first commands:

| Command | Purpose |
| --- | --- |
| `PING` | Communication test |
| `VERSION?` | Identify firmware |
| `STATUS` | Report basic state |
| `BTN?` | Read button |
| `LID?` | Read lid switch |
| `OUT1 ON` | Test output 1 |
| `OUT1 OFF` | Test output 1 |
| `OUT1?` | Read output 1 |
| `SERVICE ON` | Enter service mode during bench testing |
| `SERVICE OFF` | Exit service mode |

This is enough for early development.

---

## Commands To Delay

The first firmware should delay high-risk commands.

Delay these commands:

- `MODCHIP ON`
- `MODCHIP OFF`
- `MODCHIP NEXTBOOT OFF`
- `VIDEO_PWR OFF`
- `ROUTER WAIT`
- `PROFILE SAVE`
- `ERASE SETTINGS`
- Full SD2PSX commands
- Full BlueRetro commands
- Automatic power management commands
- Firmware update commands

Add these only after the base interposer is proven.

---

## Testing Requirements

UART testing should include:

- Boot message test
- Ready message test
- Valid command test
- Invalid command test
- Unknown command test
- Long command test
- Repeated command test
- Output command test
- Locked command test
- Service mode test
- Power-cycle test
- Touchscreen command test
- Noise and wiring test

The parser should fail safely.

---

## Test Logs

UART logs should be saved for development.

Suggested folder:

```text
Test-Data/UART-Logs/
```

Useful logs:

- First boot log
- Button press log
- Lid switch log
- Output test log
- Touchscreen command log
- Error test log
- Service mode log
- Console test log

Logs should include firmware version and hardware revision when possible.

---

## Documentation Requirements

Every implemented UART command should document:

- Command name
- Purpose
- Required mode
- Required profile
- Accepted values
- Response format
- Possible error responses
- Safety level
- Hardware requirement
- Firmware version added
- Test status

This will prevent confusion later.

---

## Open Questions

Open UART protocol questions:

- Should the first baud rate be 9600 or 115200?
- Should commands be uppercase-only?
- Should the parser accept lowercase?
- Should responses always echo the command?
- Should event messages be enabled by default?
- Should the debug UART and touchscreen UART use the same protocol?
- Should feature aliases be included early?
- Should device targeting be included from the start?
- Should service mode require a physical jumper?
- Should the protocol eventually include checksums?
- Should a binary protocol ever be needed?
- How much RAM will the selected PIC have for command buffers?

These questions should be answered during firmware testing.

---

## Risks

Possible UART protocol risks:

- Parser accepts unsafe commands
- Buffer overflow
- Bad line ending handling
- Touchscreen assumes command success
- UART noise causes false commands
- Wrong baud rate causes unreliable communication
- Multiple TX lines tied together
- Backfeeding through UART lines
- Service mode left unlocked
- Commands become too large for the selected PIC
- Protocol changes break touchscreen firmware

Start small and test carefully.

---

## Summary

The UART command system should begin as a simple, human-readable protocol for debugging and early control.

The first useful UART features should be:

- Boot and ready messages
- Button event reports
- Lid switch reports
- Basic status command
- Version command
- One or two output commands
- Clear command responses
- Safe rejection of unknown or unsafe commands

Advanced commands for touchscreen control, ChipSlayer, modchips, Bluetooth audio, BlueRetro, RGB, video power, router control, settings, and diagnostics can be added after the base firmware is stable.
