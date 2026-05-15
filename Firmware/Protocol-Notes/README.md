# Protocol Notes

## Overview

This folder contains communication protocol notes for the **FBD PS2 Button Butler** project.

The goal of these notes is to define simple, reliable, and easy-to-debug communication between the Button Butler boards and optional internal PS2 mod systems.

Button Butler may eventually include several communicating devices, including:

- PIC Button Interposer Board
- Touchscreen Controller
- I/O Expansion Board
- Power Management Board
- BlueRetro-related control hardware
- SD2PSX-related status or control hardware
- Bluetooth audio control hardware
- Internal router or network device
- Future LazyrSavre-style diagnostic hardware

The protocol should start simple and grow only as needed.

---

## Purpose Of This Folder

The `Protocol-Notes/` folder is used to document command ideas, message formats, communication rules, and safety behavior for Button Butler firmware.

This folder may include:

- UART command ideas
- I/O command ideas
- Event message ideas
- Error response ideas
- Touchscreen command flow
- Device addressing ideas
- Service mode command notes
- Safety and lockout behavior
- Future protocol expansion ideas

These notes are early planning documents.

They are not final firmware specifications yet.

---

## Current Documents

| File | Purpose |
| --- | --- |
| `README.md` | Overview of the protocol planning folder |
| `UART-Command-Ideas.md` | General UART command structure, command examples, responses, and event messages |
| `IO-Command-Ideas.md` | I/O control commands for outputs, inputs, aliases, service mode, and mod control |

Additional protocol documents may be added later as the project develops.

---

## Main Protocol Goal

The main protocol goal is:

**Make Button Butler boards talk to each other in a simple, safe, and debuggable way.**

The protocol should allow the system to:

- Send status messages
- Read button state
- Read lid switch state
- Control safe outputs
- Trigger mod-control actions
- Report errors
- Confirm commands
- Reject unsafe commands
- Communicate with the touchscreen
- Support service and test modes

The protocol should not become overly complicated before the hardware is proven.

---

## Early Design Direction

The early protocol should be human-readable.

Example command style:

| Command | Meaning |
| --- | --- |
| `PING` | Check if the device is responding |
| `STATUS` | Request current status |
| `BTN?` | Read button state |
| `LID?` | Read lid switch state |
| `OUT1 ON` | Turn output 1 on |
| `OUT1 OFF` | Turn output 1 off |
| `OUT1 TOGGLE` | Toggle output 1 |
| `VERSION?` | Read firmware version |

Human-readable commands are easier to test with a serial terminal, USB-to-serial adapter, or logic analyzer.

A compact binary protocol can be considered later if the project needs it.

---

## Why Human-Readable First

Human-readable commands are preferred during early development because they are:

- Easy to type manually
- Easy to read in serial logs
- Easy to document
- Easy to debug
- Easy to test before the touchscreen is ready
- Easier to understand when something goes wrong

The first goal is not maximum speed.

The first goal is safe and understandable behavior.

---

## Communication Method

The first planned communication method is UART.

UART is useful because it is:

- Simple
- Common
- Easy to test
- Supported by many microcontrollers
- Compatible with USB-to-serial adapters
- Good enough for low-speed command and status traffic

Possible UART uses:

- Interposer to touchscreen communication
- Interposer debug output
- I/O expansion communication
- Service mode access
- Optional device communication

---

## UART Wiring Basics

A basic UART connection normally uses:

| Signal | Purpose |
| --- | --- |
| TX | Transmit data |
| RX | Receive data |
| GND | Shared ground reference |
| VCC reference | Optional voltage reference, not always used |
| EN or WAKE | Optional enable or wake signal |

Typical point-to-point wiring:

| Device A | Device B |
| --- | --- |
| TX | RX |
| RX | TX |
| GND | GND |

TX and RX should be crossed between devices.

---

## Important UART Rule

Standard UART is normally point-to-point.

Multiple UART TX pins should not be tied together directly.

If multiple devices need to share one UART connection, the design should use:

- A UART mux
- Tri-state control
- Open-drain style signaling if designed for it
- A proper shared bus design
- A controller with multiple hardware UARTs

For early Button Butler development, point-to-point UART is preferred.

---

## Possible UART Settings

Initial UART settings may be:

| Setting | Starting Value |
| --- | --- |
| Baud rate | 115200 or 9600 |
| Data bits | 8 |
| Parity | None |
| Stop bits | 1 |
| Flow control | None |
| Logic level | Match the hardware voltage |

The final baud rate should be selected after testing the selected microcontroller and oscillator accuracy.

---

## Device Roles

Button Butler may use several device roles.

| Device | Role |
| --- | --- |
| Button Interposer | Handles critical power/reset behavior and safety logic |
| Touchscreen Controller | Provides user interface and sends command requests |
| I/O Expansion Board | Provides extra digital inputs and outputs |
| Power Management Board | Controls accessory power outputs if used |
| Service Adapter | Allows debugging and manual command testing |
| Optional Mod Device | BlueRetro, SD2PSX, Bluetooth audio, router, or future hardware |

Each device should have a clear responsibility.

---

## Interposer Role

The Button Interposer Board should remain the authority for critical console behavior.

The interposer should handle:

- Button input
- Button debounce
- Lid switch input
- Console-side button output
- Safe output defaults
- Command validation
- Safety lockouts
- Service mode control
- Status reporting

The touchscreen should request actions.

The interposer should decide whether those actions are safe.

---

## Touchscreen Role

The touchscreen controller should act as the user interface.

The touchscreen may:

- Request current state
- Display status
- Send command requests
- Show command results
- Display errors
- Show service pages
- Hide unavailable features
- Provide user confirmations

The touchscreen should not assume that a command worked until the interposer confirms it.

---

## I/O Expansion Role

The I/O Expansion Board may provide extra control points.

Possible I/O expansion functions:

- Control LEDs
- Control RGB lighting
- Trigger Bluetooth pairing
- Enable or disable ChipSlayer
- Enable or disable modchip control circuits
- Control accessory power enable lines
- Read status inputs
- Report input/output state
- Support service testing

The I/O Expansion Board should have safe default output states.

---

## Basic Message Types

The protocol may include several message types.

| Message Type | Purpose |
| --- | --- |
| Command | A request from one device to another |
| Response | A reply showing whether a command succeeded |
| Event | A message sent when something changes |
| State Report | A message describing current system state |
| Error Report | A message describing a problem |
| Version Report | A message identifying firmware or hardware |

This structure keeps communication organized.

---

## Command Messages

Command messages request an action or information.

Examples:

| Command | Purpose |
| --- | --- |
| `PING` | Check communication |
| `STATUS` | Request system state |
| `OUT1 ON` | Turn output 1 on |
| `OUT1 OFF` | Turn output 1 off |
| `LID?` | Read lid switch |
| `BTN?` | Read button state |
| `SERVICE ON` | Request service mode |
| `VERSION?` | Request firmware version |

Commands should be short and predictable.

---

## Response Messages

Every command should receive a response.

Possible responses:

| Response | Meaning |
| --- | --- |
| `OK` | Command accepted |
| `ERR` | Command failed |
| `UNKNOWN` | Command not recognized |
| `LOCKED` | Command is protected |
| `UNSAFE` | Command blocked for safety |
| `BUSY` | Device is busy |
| `STATE` | State information follows |
| `VERSION` | Version information follows |

The touchscreen should update its UI based on responses, not assumptions.

---

## Event Messages

Event messages are sent when something changes.

Possible events:

| Event | Meaning |
| --- | --- |
| `EVT BOOT` | Firmware restarted |
| `EVT READY` | Firmware is ready |
| `EVT BTN_PRESS` | Button was pressed |
| `EVT BTN_RELEASE` | Button was released |
| `EVT SHORT_PRESS` | Short press detected |
| `EVT LONG_PRESS` | Long press detected |
| `EVT LID_OPEN` | Lid opened |
| `EVT LID_CLOSED` | Lid closed |
| `EVT OUT1 ON` | Output 1 turned on |
| `EVT OUT1 OFF` | Output 1 turned off |
| `EVT ERROR` | Error occurred |

Events are useful for touchscreen status updates and serial debugging.

---

## State Reports

State reports describe current conditions.

Possible state reports:

| Report | Example Meaning |
| --- | --- |
| `STATE INPUT` | Button, lid, and service input states |
| `STATE OUTPUT` | Output states |
| `STATE MODS` | Mod-related states |
| `STATE POWER` | Accessory power states |
| `STATE SYSTEM` | Firmware mode, profile, or lock state |

Example state idea:

`STATE INPUT BTN=RELEASED LID=CLOSED SERVICE=OFF`

Example output state idea:

`STATE OUTPUT OUT1=OFF OUT2=ON`

These examples may change as firmware develops.

---

## Error Reports

Error reports should be clear.

Possible errors:

| Error | Meaning |
| --- | --- |
| `ERR UNKNOWN_COMMAND` | Command was not recognized |
| `ERR INVALID_VALUE` | Command value was invalid |
| `ERR LOCKED` | Command requires unlock or service mode |
| `ERR UNSAFE` | Command was blocked for safety |
| `ERR BUSY` | Device is busy |
| `ERR PROFILE` | Command not available in active profile |
| `ERR HARDWARE` | Hardware fault or unavailable signal |

Readable errors are useful during development.

Short error codes can be added later if needed.

---

## Version Reports

Version reports help match firmware and hardware.

Possible version response:

`VERSION BB_INTERPOSER v0.1.0 HW=PROTO_A PROFILE=BASIC`

Useful version fields:

- Device name
- Firmware version
- Hardware revision
- Active profile
- Protocol version
- Build type
- Feature list if supported

Version reporting is important once multiple board revisions exist.

---

## Device Addressing

Early prototypes may not need device addressing.

If multiple devices are connected through a hub, target names may be useful.

Possible target names:

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

| Command | Meaning |
| --- | --- |
| `BB STATUS` | Ask interposer for status |
| `IO OUT1 ON` | Turn I/O output 1 on |
| `BT PAIR` | Trigger Bluetooth pairing |
| `VID POWER OFF` | Request video power off |
| `RGB OFF` | Turn RGB lighting off |

Targeting can be added later if needed.

---

## Command Safety

Some commands are safe to allow in normal mode.

Other commands should be restricted.

Low-risk commands:

- `PING`
- `STATUS`
- `BTN?`
- `LID?`
- `VERSION?`
- `OUT1?`
- `IN1?`

Higher-risk commands:

- Modchip enable or disable
- Video power off
- Accessory power off
- Reset request
- Power request
- Restore defaults
- Save settings
- Enter service mode
- Change profile

Risky commands may require confirmation, service mode, a lid-open condition, or an unlock step.

---

## Suggested Safety Levels

Commands may use safety levels.

| Safety Level | Description |
| --- | --- |
| Safe | Allowed in normal mode |
| Limited | Allowed only in certain profiles |
| Protected | Requires confirmation or service mode |
| Service Only | Requires service mode or jumper |
| Disabled | Not available in current firmware or profile |

This can help the touchscreen hide or lock risky actions.

---

## Service Mode

Service mode is intended for testing, development, and installer functions.

Service mode may allow:

- Output testing
- Input reading
- Profile checking
- Version checking
- Safe mode entry
- Manual mod-control testing
- UART command testing
- Touchscreen testing

Service mode should not be easy to enter accidentally in customer builds.

---

## Service Mode Commands

Possible service commands:

| Command | Purpose |
| --- | --- |
| `SERVICE ON` | Enter service mode |
| `SERVICE OFF` | Exit service mode |
| `READ ALL` | Read all known inputs and outputs |
| `TEST OUT1` | Test output 1 |
| `TEST OUT2` | Test output 2 |
| `SAFE` | Enter safe mode |
| `PASS` | Force pass-through mode if supported |
| `VERSION?` | Read firmware version |
| `PROFILE?` | Read active profile |

Service commands should be documented carefully before release.

---

## Safe Mode

Safe mode is a conservative state used for testing or recovery.

Possible safe mode behavior:

- Optional outputs off
- Risky commands blocked
- Mod-control outputs restored to safe defaults
- Normal button behavior preserved if possible
- UART status still available
- Service mode commands limited
- Touchscreen shows safe mode warning if installed

Safe mode should help recover from bad settings or unexpected behavior.

---

## Lockout Behavior

Lockout behavior prevents accidental command execution.

Possible lockout methods:

- Service mode required
- Lid switch must be open
- Physical service jumper required
- Touchscreen confirmation required
- Touchscreen PIN required
- Startup-only command window
- UART unlock command during bench testing

Lockout behavior should match the risk of the command.

---

## Startup Protocol

Startup communication should be predictable.

Recommended startup flow:

| Step | Device Behavior |
| --- | --- |
| 1 | Device powers up |
| 2 | Outputs initialize to safe states |
| 3 | UART initializes |
| 4 | Device reports `EVT BOOT` |
| 5 | Device reads initial states |
| 6 | Device reports `EVT READY` |
| 7 | Touchscreen sends `STATUS` |
| 8 | Device replies with current state |

The touchscreen should wait for the interposer to be ready before sending risky commands.

---

## Shutdown Protocol

Shutdown communication should be conservative.

Recommended shutdown behavior:

- Do not write settings during unstable power.
- Do not send new risky commands during power loss.
- Return noncritical outputs to safe states where possible.
- Avoid backfeeding through UART or I/O lines.
- Report shutdown or power-loss state only if reliable.
- Let hardware defaults handle unsafe power loss conditions.

Hardware design is required for safe shutdown behavior.

---

## Heartbeat Concept

A heartbeat message may be useful later.

Possible heartbeat uses:

- Confirm interposer is alive
- Confirm touchscreen is connected
- Detect communication loss
- Show link status on the display
- Enter safe behavior if communication fails

Example heartbeat idea:

| Message | Meaning |
| --- | --- |
| `PING` | Are you there? |
| `OK PONG` | Yes, responding |
| `EVT HEARTBEAT` | Periodic alive message |

Heartbeat should not be required for basic button behavior.

The interposer should keep working even if the touchscreen disappears.

---

## Feature Discovery

Future firmware may support feature discovery.

Possible feature discovery commands:

| Command | Purpose |
| --- | --- |
| `FEATURES?` | List supported features |
| `OUTPUTS?` | List available outputs |
| `INPUTS?` | List available inputs |
| `ALIASES?` | List supported command aliases |
| `CAPS?` | List capabilities |

Feature discovery can help the touchscreen hide unavailable pages.

Early firmware may not need this.

---

## Profiles

Build profiles may change available commands and behavior.

Possible profiles:

| Profile | Purpose |
| --- | --- |
| Basic | Simple button behavior and debug |
| ChipSlayer | ChipSlayer control |
| Modchip | Modchip enable/disable logic |
| Bluetooth Audio | Audio enable and pairing |
| BlueRetro | BlueRetro enable/reset |
| Premium Touchscreen | Full touchscreen control |
| Service/Test | Development and testing |

Profiles may be hardcoded at first.

Selectable profiles can be added later.

---

## Command Format Options

There are several possible command format styles.

| Format | Example | Notes |
| --- | --- | --- |
| Short text | `OUT1 ON` | Easy to type and read |
| Targeted text | `IO OUT1 ON` | Better for multiple devices |
| Key/value | `SET OUT1=ON` | Clear but slightly longer |
| Event style | `EVT OUT1 ON` | Good for reporting |
| Binary | Not defined yet | Compact but harder to debug |

Early development should use short text or targeted text.

---

## Line Endings

Commands should have a clear line ending.

Possible line endings:

| Line Ending | Notes |
| --- | --- |
| LF | Common in many serial terminals |
| CRLF | Common in Windows-style terminals |
| CR | Less common but possible |

Firmware should ideally tolerate LF and CRLF during development.

The final protocol can define a preferred line ending later.

---

## Case Sensitivity

Command parsing should probably be case-insensitive during early testing.

Examples that could all be accepted:

- `STATUS`
- `status`
- `Status`

Case-insensitive parsing makes manual testing easier.

If firmware space is limited, uppercase-only may be simpler.

---

## Command Length

Commands should be kept short.

Recommended early rules:

- Keep commands under 32 characters when possible.
- Avoid long strings.
- Avoid complicated nested commands.
- Ignore or reject commands that exceed the buffer.
- Protect against buffer overflow.

Small microcontrollers may have limited RAM.

---

## Timing Rules

Command timing should be predictable.

Recommended timing rules:

- Respond quickly when possible.
- Avoid long blocking delays.
- Use events for longer actions.
- Return `BUSY` if an action is already in progress.
- Rate-limit risky commands.
- Do not allow repeated reset or power commands too quickly.

A command parser should not interfere with button handling.

---

## Rate Limiting

Some commands should be rate-limited.

Possible rate-limited commands:

- Reset pulse
- Power request
- Bluetooth pairing
- BlueRetro reset
- Router reset
- Video power toggle
- Modchip toggle
- Save settings

Rate limiting prevents repeated accidental actions.

---

## Output Default States

Protocol commands should never leave outputs undefined.

Every output should have:

- Default state
- Active state
- Safe state
- Startup state
- Service mode behavior
- Error behavior

Command responses should make output state clear.

---

## Backfeeding Awareness

The protocol cannot fix bad hardware, but it can help avoid risky behavior.

Protocol rules that help:

- Do not enable outputs before hardware is ready.
- Do not send commands to unpowered devices.
- Report unknown states clearly.
- Do not assume an accessory is powered.
- Require status before risky actions.
- Disable or reject commands when a device is unavailable.

Hardware still needs proper backfeeding protection.

---

## Touchscreen Command Flow

A touchscreen command flow should be confirmed.

Example flow:

| Step | Action |
| --- | --- |
| 1 | User taps a touchscreen button |
| 2 | Touchscreen sends command |
| 3 | Interposer checks safety rules |
| 4 | Interposer accepts or rejects command |
| 5 | Interposer sends response |
| 6 | Touchscreen updates UI |
| 7 | Interposer sends event if state changed |

The touchscreen should not change the displayed state until it receives confirmation.

---

## Example Touchscreen Flow

Example:

| Message | Meaning |
| --- | --- |
| `TS -> BB: STATUS` | Touchscreen asks for current state |
| `BB -> TS: STATE MODS CHIPSLAYER=OFF RGB=ON` | Interposer reports state |
| `TS -> BB: CHIPSLAYER ON` | User requests ChipSlayer on |
| `BB -> TS: OK CHIPSLAYER ON` | Interposer accepts command |
| `BB -> TS: EVT CHIPSLAYER ON` | Interposer reports state change |

This flow keeps the touchscreen honest.

---

## Minimum Viable Protocol

The first useful protocol should be small.

Minimum useful commands:

| Command | Purpose |
| --- | --- |
| `PING` | Confirm communication |
| `STATUS` | Read current state |
| `BTN?` | Read button state |
| `LID?` | Read lid state |
| `OUT1 ON` | Turn output 1 on |
| `OUT1 OFF` | Turn output 1 off |
| `OUT1 TOGGLE` | Toggle output 1 |
| `OUT1?` | Read output 1 state |
| `VERSION?` | Read firmware version |

This is enough for early bench testing and touchscreen proof-of-concept work.

---

## Commands To Delay

Some commands should be delayed until the base system is stable.

Delay these features:

- Persistent setting writes
- Full profile editing
- SD2PSX control commands
- Full BlueRetro command integration
- Automatic video power management
- Internal router automation
- Complex RGB effects
- Advanced diagnostic logging
- Firmware update commands
- Customer-facing service unlock commands

These can be added after the basic protocol is reliable.

---

## Documentation Requirements

Every final command should document:

- Command name
- Purpose
- Required mode
- Required profile
- Accepted values
- Response format
- Possible errors
- Safety level
- Hardware requirement
- Firmware version added
- Notes

This will prevent confusion as the project grows.

---

## Testing Requirements

Protocol testing should include:

- Manual serial terminal testing
- Valid command testing
- Invalid command testing
- Unknown command testing
- Long command testing
- Repeated command testing
- Power-cycle testing
- Startup command testing
- Touchscreen command testing
- Service mode testing
- Safety lockout testing

The protocol should fail safely when it receives bad input.

---

## Test Logs

Useful protocol logs should be saved in:

`Test-Data/UART-Logs/`

Useful logs include:

- First boot logs
- Button event logs
- Lid switch logs
- Output control logs
- Touchscreen command logs
- Error handling logs
- Service mode logs
- Console test logs

Logs should include firmware version and hardware revision when possible.

---

## Open Questions

Open protocol questions:

- Should the first protocol be uppercase-only or case-insensitive?
- Should commands use generic output names, feature aliases, or both?
- Should responses always echo the command?
- Should device addressing be included from the start?
- Should the touchscreen use the same protocol as the debug UART?
- Should binary protocol be used later?
- Should checksums be added later?
- What baud rate should become the standard?
- Should service mode require a physical jumper?
- Should feature discovery be supported?
- Should settings be saved through commands?
- How should protocol versions be handled?

These questions should be answered during firmware testing.

---

## Risks

Possible protocol risks:

- Bad command parsing
- Buffer overflow on small MCUs
- Touchscreen assumes command success
- Commands trigger unsafe outputs
- UART noise creates false commands
- Too many commands for the selected PIC
- Commands behave differently across firmware versions
- Service mode left unlocked
- Modchip command changes boot behavior unexpectedly
- Video power command blanks display unexpectedly
- Saved settings create bad startup behavior

The protocol should start small and be expanded carefully.

---

## Summary

The `Protocol-Notes/` folder documents how Button Butler devices may communicate.

The first protocol should be:

- Simple
- Human-readable
- UART-based
- Easy to debug
- Safe by default
- Small enough for early PIC firmware
- Expandable later

The first useful command set should focus on communication checks, status reporting, button state, lid state, one or two outputs, and firmware version reporting.

Advanced commands for touchscreen control, mod control, power management, profiles, diagnostics, and service modes can be added after the base interposer firmware is stable.
