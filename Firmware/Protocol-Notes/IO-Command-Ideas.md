# I/O Command Ideas

## Overview

This document collects early command ideas for the **FBD PS2 Button Butler** I/O control system.

The goal is to define simple, readable command concepts that can be used between the Button Butler interposer, touchscreen controller, I/O expansion board, and future mod-control boards.

These commands are early planning ideas.

They are not final firmware requirements yet.

---

## Purpose

The purpose of I/O commands is to give Button Butler a clean way to control and monitor simple inputs and outputs.

Possible uses:

- Turn outputs on or off
- Toggle outputs
- Pulse outputs
- Read input states
- Read output states
- Trigger Bluetooth audio pairing
- Enable or disable ChipSlayer
- Enable or disable a modchip control circuit
- Enable or disable RGB lighting
- Control accessory power
- Report I/O status to the touchscreen
- Support service mode testing

The command system should be simple enough to debug over a serial terminal.

---

## Design Goals

The I/O command system should be:

1. Simple
2. Human-readable during early testing
3. Easy to parse
4. Easy to debug
5. Safe by default
6. Compatible with touchscreen control
7. Compatible with service mode
8. Expandable for future boards
9. Clear about command success or failure
10. Resistant to accidental unsafe actions

The first version should favor clarity over compactness.

---

## Human-Readable Command Style

For early firmware, human-readable commands are recommended.

Example style:

```text
OUT1 ON
OUT1 OFF
OUT1 TOGGLE
IN1?
STATUS
```

Human-readable commands make early testing easier because they can be sent from a serial terminal.

A binary protocol can be considered later if needed.

---

## Basic Command Format

A simple command format could look like this:

```text
<TARGET> <ACTION> <VALUE>
```

Examples:

```text
OUT1 SET ON
OUT2 SET OFF
OUT1 TOGGLE
IN1 READ
IO STATUS
```

A shorter format may also be used:

```text
OUT1 ON
OUT1 OFF
OUT1 TOGGLE
IN1?
STATUS
```

The exact format can be decided later after firmware testing.

---

## Suggested Response Format

Every command should return a response.

Possible responses:

| Response | Meaning |
| --- | --- |
| `OK` | Command accepted and completed |
| `ERR` | Command failed |
| `UNKNOWN` | Command was not recognized |
| `LOCKED` | Command is protected or unavailable |
| `UNSAFE` | Command was blocked for safety |
| `BUSY` | Device is busy |
| `STATE` | State information follows |
| `VALUE` | Requested value follows |
| `VERSION` | Firmware or hardware version follows |

The touchscreen should not assume that a command worked unless it receives a valid response.

---

## Command Categories

I/O commands can be grouped into categories.

| Category | Purpose |
| --- | --- |
| Output Control | Turn outputs on, off, toggle, or pulse |
| Input Reading | Read input states |
| Status Reporting | Return all known I/O states |
| Service Testing | Manually test outputs and inputs |
| Profile Control | Apply build-specific I/O behavior |
| Safety Control | Lock, unlock, or force safe states |
| Version Info | Report firmware or hardware information |

---

## Output Naming

Outputs should use clear names.

Simple generic names:

```text
OUT1
OUT2
OUT3
OUT4
```

Feature-specific aliases may also be useful:

```text
CHIPSLAYER
MODCHIP
BT_AUDIO
BT_PAIR
BLUERETRO
RGB
VIDEO_PWR
ROUTER_PWR
BUZZER
```

Generic names are easier for firmware.

Aliases are easier for humans and touchscreen menus.

Both may be supported later.

---

## Input Naming

Inputs should also use clear names.

Simple generic names:

```text
IN1
IN2
IN3
IN4
```

Feature-specific aliases may include:

```text
BTN
LID
SERVICE
POWER_GOOD
BT_STATUS
ROUTER_READY
VIDEO_PG
CHIPSLAYER_STATE
MODCHIP_STATE
```

Input names should match the schematic and board silkscreen where possible.

---

## Basic Output Commands

Basic output commands may include:

| Command | Meaning |
| --- | --- |
| `OUT1 ON` | Turn output 1 on |
| `OUT1 OFF` | Turn output 1 off |
| `OUT1 TOGGLE` | Toggle output 1 |
| `OUT1?` | Read output 1 state |
| `OUT2 ON` | Turn output 2 on |
| `OUT2 OFF` | Turn output 2 off |
| `OUT2 TOGGLE` | Toggle output 2 |
| `OUT2?` | Read output 2 state |

These commands are useful for early testing.

---

## Output Pulse Commands

Some outputs should only activate briefly.

Pulse commands are useful for:

- Reset lines
- Pairing triggers
- Button simulation
- Chime triggers
- Wake signals

Possible pulse commands:

| Command | Meaning |
| --- | --- |
| `OUT1 PULSE` | Pulse output 1 using default pulse time |
| `OUT1 PULSE 100` | Pulse output 1 for 100 ms |
| `BT_PAIR PULSE` | Trigger Bluetooth pairing |
| `BLUERETRO_RESET PULSE` | Reset BlueRetro controller |
| `BUZZER PULSE` | Trigger buzzer output |

Pulse time limits should be enforced by firmware.

---

## Pulse Safety Rules

Pulse commands should follow safety rules:

- Use a default pulse time.
- Enforce a maximum pulse time.
- Reject unsafe pulse requests.
- Do not allow reset outputs to pulse during startup.
- Do not allow repeated pulse spam unless intentionally supported.
- Report whether the pulse was accepted or rejected.

Example response:

```text
OK OUT1 PULSE 100
```

or:

```text
UNSAFE OUT1 PULSE
```

---

## Input Read Commands

Input read commands may include:

| Command | Meaning |
| --- | --- |
| `IN1?` | Read input 1 |
| `IN2?` | Read input 2 |
| `BTN?` | Read physical button state |
| `LID?` | Read lid switch state |
| `SERVICE?` | Read service jumper state |
| `POWER_GOOD?` | Read power-good input |
| `BT_STATUS?` | Read Bluetooth audio status input |
| `ROUTER_READY?` | Read router ready input |

Inputs should return a clear state.

Example:

```text
VALUE LID OPEN
```

or:

```text
VALUE IN1 HIGH
```

---

## Status Commands

Status commands return several states at once.

Possible status commands:

| Command | Meaning |
| --- | --- |
| `STATUS` | Return general system status |
| `IO STATUS` | Return all known I/O states |
| `OUT STATUS` | Return output states only |
| `IN STATUS` | Return input states only |
| `SAFE STATUS` | Return safety/lockout state |
| `PROFILE?` | Return active profile |

Example response:

```text
STATE IO OUT1=OFF OUT2=ON IN1=LOW LID=CLOSED
```

The exact response format can change later.

---

## Service Mode Commands

Service mode commands are used for bench testing and installation.

Possible service commands:

| Command | Meaning |
| --- | --- |
| `SERVICE ON` | Enter service mode |
| `SERVICE OFF` | Exit service mode |
| `TEST OUT1` | Test output 1 |
| `TEST OUT2` | Test output 2 |
| `TEST ALL OUT` | Test all outputs one at a time |
| `READ ALL` | Read all inputs and outputs |
| `SAFE` | Force safe mode |
| `PASS` | Force pass-through mode if supported |
| `DEFAULTS` | Restore safe default states if supported |

Service mode should be protected if connected to customer-facing controls.

---

## Lock And Unlock Commands

Some actions should be locked during normal operation.

Possible lock commands:

| Command | Meaning |
| --- | --- |
| `LOCK` | Lock protected actions |
| `UNLOCK` | Unlock protected actions if allowed |
| `LOCK?` | Read lock state |
| `SERVICE LOCK` | Lock service functions |
| `SERVICE UNLOCK` | Unlock service functions if allowed |

Unlock behavior should be protected.

Possible unlock methods:

- Service jumper
- Lid-open condition
- Touchscreen PIN
- UART command during bench testing
- Special startup hold

---

## Safe Mode Commands

Safe mode should place outputs into conservative states.

Possible safe mode commands:

| Command | Meaning |
| --- | --- |
| `SAFE` | Enter safe mode |
| `SAFE ON` | Enter safe mode |
| `SAFE OFF` | Exit safe mode if allowed |
| `SAFE?` | Read safe mode state |
| `ALL OFF` | Turn all noncritical outputs off |
| `OUTPUTS SAFE` | Restore all outputs to safe defaults |

Safe mode should not disable the basic ability to power or reset the console unless intentionally designed that way.

---

## Profile Commands

Build profiles may change what outputs do.

Possible profile commands:

| Command | Meaning |
| --- | --- |
| `PROFILE?` | Read active profile |
| `PROFILE BASIC` | Select basic profile |
| `PROFILE CHIPSLAYER` | Select ChipSlayer profile |
| `PROFILE BLUERETRO` | Select BlueRetro profile |
| `PROFILE BT_AUDIO` | Select Bluetooth audio profile |
| `PROFILE PREMIUM` | Select premium touchscreen profile |
| `PROFILE SERVICE` | Select service/test profile |
| `PROFILE SAVE` | Save active profile if supported |

Early firmware may not support profile changing.

Profiles can be compiled into firmware at first.

---

## Feature Alias Commands

Feature aliases make commands easier to understand.

Examples:

| Command | Meaning |
| --- | --- |
| `CHIPSLAYER ON` | Enable ChipSlayer output |
| `CHIPSLAYER OFF` | Disable ChipSlayer output |
| `CHIPSLAYER TOGGLE` | Toggle ChipSlayer output |
| `MODCHIP ON` | Enable modchip control output |
| `MODCHIP OFF` | Disable modchip control output |
| `BT_AUDIO ON` | Enable Bluetooth audio |
| `BT_AUDIO OFF` | Disable Bluetooth audio |
| `BT_PAIR PULSE` | Trigger Bluetooth pairing |
| `RGB ON` | Turn RGB lighting on |
| `RGB OFF` | Turn RGB lighting off |
| `VIDEO_PWR ON` | Enable video board power |
| `VIDEO_PWR OFF` | Disable video board power |

Aliases should map to real outputs in the active build profile.

---

## ChipSlayer Command Ideas

Possible ChipSlayer commands:

| Command | Meaning |
| --- | --- |
| `CHIPSLAYER ON` | Enable ChipSlayer |
| `CHIPSLAYER OFF` | Disable ChipSlayer |
| `CHIPSLAYER TOGGLE` | Toggle ChipSlayer state |
| `CHIPSLAYER?` | Read ChipSlayer state |
| `CHIPSLAYER DEFAULT` | Restore profile default |
| `CHIPSLAYER LOCK` | Prevent accidental changes |
| `CHIPSLAYER UNLOCK` | Allow changes if service mode permits |

ChipSlayer state should have a defined default.

---

## Modchip Command Ideas

Possible modchip commands:

| Command | Meaning |
| --- | --- |
| `MODCHIP ON` | Enable modchip |
| `MODCHIP OFF` | Disable modchip |
| `MODCHIP TOGGLE` | Toggle modchip state |
| `MODCHIP?` | Read modchip state if known |
| `MODCHIP NEXTBOOT OFF` | Disable modchip for next boot if supported |
| `MODCHIP DEFAULT` | Restore profile default |
| `MODCHIP LOCK` | Lock modchip changes |
| `MODCHIP UNLOCK` | Unlock modchip changes if allowed |

Modchip commands may need confirmation or service mode.

---

## Bluetooth Audio Command Ideas

Possible Bluetooth audio commands:

| Command | Meaning |
| --- | --- |
| `BT_AUDIO ON` | Enable Bluetooth audio |
| `BT_AUDIO OFF` | Disable Bluetooth audio |
| `BT_AUDIO TOGGLE` | Toggle Bluetooth audio |
| `BT_AUDIO?` | Read Bluetooth audio state |
| `BT_PAIR PULSE` | Trigger pairing mode |
| `BT_RESET PULSE` | Reset Bluetooth audio board |
| `BT_STATUS?` | Read status input if available |

Bluetooth pairing should usually be a pulse, not a latched output.

---

## BlueRetro Command Ideas

Possible BlueRetro commands:

| Command | Meaning |
| --- | --- |
| `BLUERETRO ON` | Enable BlueRetro |
| `BLUERETRO OFF` | Disable BlueRetro |
| `BLUERETRO TOGGLE` | Toggle BlueRetro enable |
| `BLUERETRO RESET` | Reset BlueRetro |
| `BR_CH1 ON` | Enable BlueRetro channel 1 if supported |
| `BR_CH1 OFF` | Disable BlueRetro channel 1 if supported |
| `BR_CH2 ON` | Enable BlueRetro channel 2 if supported |
| `BR_CH2 OFF` | Disable BlueRetro channel 2 if supported |
| `BLUERETRO?` | Read BlueRetro state if available |

Early support may only include enable or reset.

---

## RGB Command Ideas

Possible RGB commands:

| Command | Meaning |
| --- | --- |
| `RGB ON` | Turn RGB lighting on |
| `RGB OFF` | Turn RGB lighting off |
| `RGB TOGGLE` | Toggle RGB lighting |
| `RGB?` | Read RGB state |
| `RGB MODE 1` | Select mode 1 if supported |
| `RGB MODE 2` | Select mode 2 if supported |
| `RGB BRIGHT 50` | Set brightness to 50 percent if supported |
| `RGB DEFAULT` | Restore default lighting behavior |

Early support may only include on/off control.

---

## Video Power Command Ideas

Possible video power commands:

| Command | Meaning |
| --- | --- |
| `VIDEO_PWR ON` | Enable video board power |
| `VIDEO_PWR OFF` | Disable video board power |
| `VIDEO_PWR TOGGLE` | Toggle video power |
| `VIDEO_PWR?` | Read video power state |
| `VIDEO_AUTO ON` | Enable automatic video power management |
| `VIDEO_AUTO OFF` | Disable automatic video power management |
| `VIDEO_PG?` | Read video power-good input if available |

Video power commands should be protected if they can blank the display.

---

## Router Power Command Ideas

Possible internal router commands:

| Command | Meaning |
| --- | --- |
| `ROUTER ON` | Enable router power |
| `ROUTER OFF` | Disable router power |
| `ROUTER RESET` | Reset or restart router |
| `ROUTER?` | Read router power state |
| `ROUTER_READY?` | Read router ready input if available |
| `ROUTER WAIT` | Wait for ready state if supported |

Router commands are useful for internal network-loading builds.

---

## Buzzer Or Chime Command Ideas

Possible buzzer commands:

| Command | Meaning |
| --- | --- |
| `BUZZER PULSE` | Trigger default beep |
| `BUZZER PULSE 100` | Trigger 100 ms beep |
| `CHIME STARTUP` | Play startup chime if supported |
| `CHIME ERROR` | Play error chime if supported |
| `CHIME OFF` | Disable chime output |
| `CHIME ON` | Enable chime output |
| `CHIME?` | Read chime setting |

Chime commands should be optional.

---

## Command Acknowledgement Examples

Accepted command:

```text
OK OUT1 ON
```

Rejected command:

```text
ERR OUT1 ON
```

Unknown command:

```text
UNKNOWN RGB SPARKLE
```

Locked command:

```text
LOCKED MODCHIP OFF
```

Unsafe command:

```text
UNSAFE VIDEO_PWR OFF
```

Busy response:

```text
BUSY OUT1 PULSE
```

The response should include enough information to understand what happened.

---

## Event Reporting

The I/O system may send events when states change.

Possible events:

| Event | Meaning |
| --- | --- |
| `EVT OUT1 ON` | Output 1 turned on |
| `EVT OUT1 OFF` | Output 1 turned off |
| `EVT IN1 HIGH` | Input 1 changed high |
| `EVT IN1 LOW` | Input 1 changed low |
| `EVT LID OPEN` | Lid switch changed open |
| `EVT LID CLOSED` | Lid switch changed closed |
| `EVT CHIPSLAYER ON` | ChipSlayer state changed |
| `EVT MODCHIP OFF` | Modchip state changed |
| `EVT BT_PAIR` | Bluetooth pairing triggered |
| `EVT ERROR` | Error state occurred |

Events are useful for touchscreen updates.

---

## State Reporting Examples

Generic I/O state response:

```text
STATE IO OUT1=OFF OUT2=ON IN1=LOW IN2=HIGH
```

Feature-based state response:

```text
STATE MODS CHIPSLAYER=ON MODCHIP=OFF BT_AUDIO=ON RGB=OFF
```

Power state response:

```text
STATE POWER VIDEO_PWR=ON ROUTER=OFF TOUCHSCREEN=ON
```

Button state response:

```text
STATE INPUT BTN=RELEASED LID=CLOSED SERVICE=OFF
```

These formats are only examples.

---

## Safety Restrictions

Some commands should be restricted.

Commands that may need protection:

- `MODCHIP OFF`
- `MODCHIP TOGGLE`
- `VIDEO_PWR OFF`
- `SAFE OFF`
- `SERVICE ON`
- `PROFILE SAVE`
- `DEFAULTS`
- `ROUTER OFF`
- Any command affecting console power/reset behavior
- Any command that can interrupt gameplay or saves

Protection methods may include:

- Service mode required
- Lid open required
- Touchscreen confirmation required
- UART unlock required
- Internal jumper required
- Startup-only command

---

## Command Safety Table

Example safety table:

| Command Type | Normal Mode | Service Mode | Notes |
| --- | --- | --- | --- |
| Read input | Allowed | Allowed | Safe |
| Read status | Allowed | Allowed | Safe |
| Toggle RGB | Allowed | Allowed | Low risk |
| Trigger BT pairing | Allowed or limited | Allowed | May need confirmation |
| Toggle ChipSlayer | Profile dependent | Allowed | Needs known default |
| Toggle modchip | Locked | Allowed | Affects boot behavior |
| Turn video power off | Confirm required | Allowed | Can blank display |
| Restore defaults | Locked | Allowed | Should be protected |
| Force safe mode | Allowed | Allowed | Should always be available |

This table should be adjusted as hardware is tested.

---

## Output Default State Commands

Commands may be needed to restore defaults.

Possible commands:

| Command | Meaning |
| --- | --- |
| `DEFAULTS` | Restore all defaults |
| `OUT DEFAULTS` | Restore output defaults |
| `PROFILE DEFAULTS` | Restore current profile defaults |
| `CHIPSLAYER DEFAULT` | Restore ChipSlayer default |
| `MODCHIP DEFAULT` | Restore modchip default |
| `RGB DEFAULT` | Restore RGB default |
| `POWER DEFAULT` | Restore power output defaults |

Default restoration should be tested carefully.

---

## Persistent Setting Commands

Future firmware may support saved settings.

Possible commands:

| Command | Meaning |
| --- | --- |
| `SAVE` | Save current settings |
| `SAVE PROFILE` | Save active profile |
| `SAVE OUTPUTS` | Save output defaults |
| `LOAD` | Load saved settings |
| `LOAD DEFAULTS` | Load factory defaults |
| `ERASE SETTINGS` | Clear stored settings |

Early firmware may not support persistent settings.

Stored settings should never make the console impossible to use.

---

## Device Addressing

If multiple boards share a higher-level command system, commands may need device targets.

Possible target prefixes:

| Target | Meaning |
| --- | --- |
| `BB` | Button Butler interposer |
| `IO` | I/O expansion board |
| `TS` | Touchscreen |
| `BR` | BlueRetro-related controller |
| `SD` | SD2PSX-related controller |
| `BT` | Bluetooth audio controller |
| `VID` | Video power controller |
| `RGB` | Lighting controller |

Example targeted commands:

```text
IO OUT1 ON
BB STATUS
BT PAIR
VID POWER OFF
RGB MODE 1
```

Targeted commands can be added later if needed.

---

## Error Codes

Short error codes may be useful later.

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

Example:

```text
ERR E04 MODCHIP OFF LOCKED
```

Human-readable errors are better during early development.

---

## Version Commands

Version commands help match firmware to hardware.

Possible commands:

| Command | Meaning |
| --- | --- |
| `VERSION?` | Read firmware version |
| `HW?` | Read hardware revision if known |
| `ID?` | Read device ID |
| `PROFILE?` | Read active profile |
| `FEATURES?` | List supported features if implemented |

Example response:

```text
VERSION BB_IO v0.1.0 HW=PROTO_A PROFILE=BASIC
```

---

## Feature Discovery Commands

A touchscreen may need to know what features exist.

Possible feature discovery commands:

| Command | Meaning |
| --- | --- |
| `FEATURES?` | Request available features |
| `OUTPUTS?` | Request available outputs |
| `INPUTS?` | Request available inputs |
| `ALIASES?` | Request supported aliases |
| `CAPS?` | Request capabilities |

Example response:

```text
STATE FEATURES OUT1 OUT2 BTN LID CHIPSLAYER RGB
```

This may help the touchscreen hide unused pages.

---

## Command Timing

Command timing should be predictable.

Possible timing rules:

- Commands should be processed quickly.
- Long actions should return `BUSY` or `OK` with an event later.
- Pulse commands should not block the main firmware loop for too long.
- Touchscreen should wait for a response before assuming success.
- Repeated commands should be rate-limited if needed.

Avoid long blocking delays.

---

## Rate Limiting

Some commands should not be repeated too quickly.

Possible rate-limited commands:

- Reset pulses
- Power toggles
- Bluetooth pairing triggers
- Router reset
- Video power toggle
- Modchip toggle
- Save settings

Rate limiting helps prevent accidental repeated actions.

Example response:

```text
BUSY BT_PAIR
```

or:

```text
LOCKED RATE_LIMIT
```

---

## Startup Behavior

During startup, command handling should be conservative.

Startup rules:

- Outputs should initialize to safe states before UART commands are accepted.
- Commands should be ignored until firmware is ready.
- The device should report `READY` when command processing is safe.
- The touchscreen should wait for `READY`.
- Unknown startup state should be reported clearly.

Example startup messages:

```text
EVT BOOT
EVT READY
```

---

## Shutdown Behavior

During shutdown or power loss:

- Outputs should return to safe states where hardware allows.
- Noncritical outputs should turn off.
- No new commands should be processed if power is unstable.
- UART lines should not backfeed unpowered boards.
- Saved settings should not be written during unstable power.

Hardware design is required for truly safe shutdown behavior.

---

## Example Early Test Session

Example serial test session:

```text
> PING
OK PONG

> STATUS
STATE IO OUT1=OFF OUT2=OFF BTN=RELEASED LID=CLOSED

> OUT1 ON
OK OUT1 ON

> OUT1?
VALUE OUT1 ON

> OUT1 OFF
OK OUT1 OFF

> LID?
VALUE LID CLOSED

> VERSION?
VERSION BB_IO v0.1.0
```

This kind of test is useful during firmware development.

---

## Example Service Test Session

Example service session:

```text
> SERVICE ON
OK SERVICE ON

> READ ALL
STATE IO OUT1=OFF OUT2=OFF IN1=LOW IN2=HIGH BTN=RELEASED LID=OPEN

> TEST OUT1
OK OUT1 PULSE 100

> OUT2 ON
OK OUT2 ON

> OUT2 OFF
OK OUT2 OFF

> SERVICE OFF
OK SERVICE OFF
```

Service mode should be clearly separated from normal mode.

---

## Example Touchscreen Session

Example touchscreen command flow:

```text
TS -> BB: STATUS
BB -> TS: STATE MODS CHIPSLAYER=OFF BT_AUDIO=OFF RGB=ON

TS -> BB: CHIPSLAYER ON
BB -> TS: OK CHIPSLAYER ON
BB -> TS: EVT CHIPSLAYER ON

TS -> BB: BT_PAIR PULSE
BB -> TS: OK BT_PAIR PULSE 500
BB -> TS: EVT BT_PAIR
```

The touchscreen should update the UI based on confirmed responses and events.

---

## Minimum Viable I/O Commands

The first firmware does not need every command.

Minimum useful commands:

| Command | Purpose |
| --- | --- |
| `PING` | Confirm communication |
| `STATUS` | Read current state |
| `OUT1 ON` | Turn output 1 on |
| `OUT1 OFF` | Turn output 1 off |
| `OUT1 TOGGLE` | Toggle output 1 |
| `OUT1?` | Read output 1 |
| `BTN?` | Read button state |
| `LID?` | Read lid state |
| `VERSION?` | Read firmware version |

This is enough for early testing.

---

## Commands To Avoid In First Firmware

The first firmware should avoid high-risk commands.

Avoid early commands such as:

- Saved profile changes
- Automatic video power toggling
- Modchip next-boot logic
- SD2PSX control
- Router startup automation
- Persistent settings write
- Complex BlueRetro commands
- Full RGB effects
- Advanced diagnostic logging

These can be added after basic I/O control is reliable.

---

## Documentation Requirements

Every supported command should eventually document:

- Command name
- Required mode
- Required profile
- Target output or input
- Accepted values
- Default behavior
- Response format
- Possible errors
- Safety notes
- Firmware version added
- Hardware revision supported

This will keep the protocol understandable as it grows.

---

## Open Questions

Open questions for I/O commands:

- Should commands use generic names, aliases, or both?
- Should command parsing be case-sensitive?
- Should commands end with newline only?
- Should responses include checksums later?
- Should the protocol stay human-readable?
- Should binary commands be used for the touchscreen later?
- Should service commands require a jumper?
- Should modchip commands require confirmation?
- Should output states be saved after power loss?
- Should all outputs have readback?
- Should the touchscreen discover available features automatically?

These questions should be answered during firmware testing.

---

## Risks

Possible risks:

- Command parser accepting unsafe input
- Output toggled accidentally
- Modchip state changed unexpectedly
- Video power disabled unexpectedly
- Touchscreen assumes command worked when it failed
- UART noise creates bad commands
- Human-readable parser uses too much program memory
- Saved settings create bad startup states
- Service mode left unlocked
- Feature aliases mapped to wrong outputs

The command system should start simple and be tested carefully.

---

## Summary

The I/O command system is meant to give Button Butler a simple way to control and monitor internal PS2 mod signals.

Early commands should be human-readable and easy to test.

The first command set should focus on:

- Communication check
- Status reporting
- Input reading
- One or two output controls
- Safe defaults
- Clear responses

Advanced commands for ChipSlayer, modchip control, Bluetooth audio, BlueRetro, RGB, video power, router control, and service features can be added after the base I/O behavior is reliable.
