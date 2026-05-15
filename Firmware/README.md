# Firmware

## Overview

This folder contains firmware notes, source code, protocol ideas, and programming documentation for the **FBD PS2 Button Butler** project.

Button Butler is planned as a modular PlayStation 2 control system that can start as a simple power/reset button interposer and later expand into a touchscreen-based mod control hub.

The firmware is responsible for making the hardware behave safely, predictably, and consistently.

---

## Purpose Of This Folder

The `Firmware/` folder is used to organize all firmware-related work for Button Butler.

This may include:

- PIC firmware for the base Button Interposer Board
- Touchscreen controller firmware
- I/O expansion board firmware
- UART command protocol notes
- Button behavior logic
- Build profile logic
- Service mode logic
- Firmware flashing instructions
- Test firmware
- Debug notes

The firmware should be developed in stages so the base button behavior is proven before advanced touchscreen features are added.

---

## Firmware Goals

The main firmware goals are:

1. Preserve safe PS2 power/reset behavior
2. Monitor button input reliably
3. Detect short press and long press actions
4. Support lid-switch based alternate behavior
5. Control outputs safely
6. Provide UART debug/status communication
7. Accept touchscreen commands only when safe
8. Prevent unsafe output states
9. Support build-specific profiles later
10. Keep the console usable even if advanced features are not installed

Reliability is more important than feature count.

---

## Planned Firmware Areas

| Folder | Purpose |
| --- | --- |
| `PIC-Button-Interposer/` | Firmware for the base power/reset button interposer board |
| `Touchscreen-Controller/` | Firmware for the touchscreen user interface |
| `Protocol-Notes/` | UART command ideas, message formats, and communication notes |
| `IO-Expansion/` | Future firmware for an optional I/O expansion board |
| `Test-Firmware/` | Simple firmware used for board bring-up and bench testing |

Some folders may be empty at first and filled in as the project develops.

---

## Recommended Folder Structure

| Path | Purpose |
| --- | --- |
| `PIC-Button-Interposer/README.md` | Notes for the base PIC interposer firmware |
| `PIC-Button-Interposer/Source/` | Main source code for the PIC firmware |
| `PIC-Button-Interposer/Builds/` | Compiled test builds or release hex files if shared |
| `PIC-Button-Interposer/Notes/` | Firmware behavior notes and development notes |
| `Touchscreen-Controller/README.md` | Notes for the touchscreen firmware |
| `Touchscreen-Controller/Source/` | Touchscreen UI source code |
| `Touchscreen-Controller/Assets/` | Images, icons, fonts, splash screens, and UI graphics |
| `Touchscreen-Controller/Builds/` | Compiled touchscreen firmware builds if shared |
| `Protocol-Notes/README.md` | Overview of communication protocol ideas |
| `Protocol-Notes/UART-Command-Ideas.md` | Human-readable UART command planning |
| `Protocol-Notes/IO-Command-Ideas.md` | I/O command planning |
| `IO-Expansion/README.md` | Future I/O expansion firmware notes |
| `Test-Firmware/README.md` | Simple test firmware and bring-up notes |

---

## Firmware Development Order

The firmware should be developed in a safe order.

Recommended development order:

1. Bring up the base interposer MCU
2. Confirm GPIO direction and safe startup states
3. Read the physical power/reset button input
4. Add button debounce
5. Detect press and release events
6. Detect short press
7. Detect long press
8. Add safe pass-through behavior
9. Add lid switch input
10. Add one safe output
11. Add UART debug messages
12. Add simple command handling
13. Add touchscreen communication
14. Add profiles and advanced behavior later

The first firmware should be simple.

Do not start with the full touchscreen system.

---

## Base Interposer Firmware

The base interposer firmware is the most important part of the project.

Its job is to safely control or pass through the PS2 power/reset button behavior.

Possible responsibilities:

- Initialize all pins safely
- Read the physical button input
- Debounce the button
- Detect short press and long press
- Read lid switch state
- Decide whether to pass or block button behavior
- Control the console-side button output
- Control one or more mod outputs
- Report state over UART
- Accept simple commands if enabled
- Reject unsafe commands
- Fall back to safe behavior when possible

The base interposer should not depend on the touchscreen to perform normal console button behavior.

---

## Touchscreen Firmware

The touchscreen firmware is the user interface layer.

Possible responsibilities:

- Display main status page
- Display mod control pages
- Display power control page
- Display service pages
- Read touch input
- Send command requests to the interposer
- Receive command acknowledgements
- Show accepted or rejected commands
- Show error messages
- Hide unused features based on build profile
- Sleep or dim the display when idle

The touchscreen should request actions.

The interposer should approve and perform critical actions.

---

## I/O Expansion Firmware

The I/O expansion firmware is a future option.

Possible responsibilities:

- Control extra digital outputs
- Read extra digital inputs
- Control LEDs or lighting
- Trigger chime or buzzer output
- Control accessory enable lines
- Report state to the interposer or touchscreen
- Accept simple commands
- Maintain safe default states

This firmware should be optional and should not be required for the base interposer to work.

---

## Protocol Notes

The `Protocol-Notes/` folder should be used to document communication between boards.

Possible protocol topics:

- UART wiring
- Baud rate
- Logic voltage
- Command format
- Response format
- Event messages
- Error messages
- Device addressing
- Safety lockouts
- Service mode commands
- Version reporting

Early firmware may use a simple human-readable UART protocol.

A compact binary protocol can be considered later if needed.

---

## Possible UART Command Style

Early UART commands may be human-readable for easy debugging.

Example command ideas:

| Command | Purpose |
| --- | --- |
| `PING` | Check if the device is responding |
| `STATUS` | Request current state |
| `BTN?` | Request button state |
| `LID?` | Request lid switch state |
| `OUT1 ON` | Turn output 1 on |
| `OUT1 OFF` | Turn output 1 off |
| `OUT1 TOGGLE` | Toggle output 1 |
| `POWER REQ` | Request a power action |
| `RESET REQ` | Request a reset action |
| `SERVICE ON` | Request service mode |
| `SERVICE OFF` | Exit service mode |

These are planning examples only.

The final format may change after testing.

---

## Possible UART Responses

Possible response ideas:

| Response | Meaning |
| --- | --- |
| `OK` | Command accepted |
| `ERR` | Command failed or was rejected |
| `BUSY` | Device is busy |
| `LOCKED` | Command is locked or protected |
| `UNSAFE` | Command blocked for safety |
| `UNKNOWN` | Command not recognized |
| `STATE` | State information follows |
| `VERSION` | Firmware or hardware version follows |

The touchscreen should not assume a command worked unless the interposer confirms it.

---

## Firmware Safety Rules

Firmware should follow strict safety rules.

Recommended rules:

- Outputs must initialize to safe states.
- Reset or power control outputs must not glitch during startup.
- The console should not be held in reset accidentally.
- The button should not be blocked unless the firmware intentionally does so.
- Dangerous commands should require confirmation or service mode.
- Touchscreen commands should be validated before action.
- Unknown commands should be ignored or rejected.
- Inputs should be debounced or filtered.
- Floating inputs should be avoided with hardware pull-ups or pull-downs.
- Watchdog resets should not create false button presses.

Firmware should never assume the hardware is perfect.

---

## Startup Behavior

Firmware startup should be predictable.

Recommended startup order:

1. Configure outputs to safe inactive states
2. Configure input pins
3. Enable pull-ups or pull-downs as needed
4. Initialize timers
5. Initialize UART if used
6. Read initial button state
7. Read initial lid switch state
8. Load profile or defaults
9. Report ready state over UART
10. Enter normal operating loop

No output should turn on randomly during startup.

---

## Main Loop Concept

The base interposer firmware may use a simple loop.

Possible main loop tasks:

- Read button input
- Debounce button input
- Read lid switch input
- Update button state machine
- Check timers
- Process UART commands
- Update outputs
- Send events if needed
- Watch for safety conditions
- Feed watchdog if used

The firmware should remain easy to understand and test.

---

## Button State Machine

The button behavior should probably use a state machine.

Possible button states:

| State | Meaning |
| --- | --- |
| Idle | Button is not pressed |
| Debounce Press | Button may have been pressed |
| Pressed | Valid press detected |
| Held | Button is being held |
| Long Press | Long press threshold reached |
| Debounce Release | Button may have been released |
| Released | Valid release detected |
| Action Pending | Firmware is deciding what action to perform |

A state machine is cleaner than random delay-based logic.

---

## Suggested Timing Values

Early firmware timing values can start with simple defaults.

| Timing Parameter | Starting Value |
| --- | --- |
| Debounce time | 20 ms to 50 ms |
| Short press maximum | 500 ms |
| Long press threshold | 2000 ms |
| Service hold threshold | 5000 ms |
| Double press window | 300 ms to 500 ms |
| Startup lockout | First 500 ms to 2000 ms after boot |

These values should be adjusted after real hardware testing.

---

## Build Profiles

Firmware may eventually support build profiles.

Possible profiles:

| Profile | Purpose |
| --- | --- |
| Basic | Normal button pass-through and debug |
| ChipSlayer | ChipSlayer control through button or touchscreen |
| Modchip | Modchip enable/disable behavior |
| Bluetooth Audio | Audio enable and pairing trigger |
| BlueRetro | BlueRetro enable/reset/channel control |
| SD2PSX | Status display and conservative control |
| Premium Touchscreen | Full touchscreen control system |
| Service/Test | Input and output testing |

Profiles can be hardcoded at first.

Stored or selectable profiles can be added later.

---

## Service Mode

Service mode may be used for testing and installation.

Possible service mode functions:

- Read all inputs
- Test all outputs
- Show button timing
- Show lid switch state
- Show firmware version
- Show hardware revision
- Toggle test outputs
- Force pass-through mode
- Reset settings
- Run self-test
- Report UART logs

Service mode should be protected from accidental use.

Possible access methods:

- UART command
- Lid open plus long button hold
- Touchscreen protected page
- Internal jumper
- Special startup hold

---

## Debug Output

UART debug output is useful during development.

Possible debug events:

| Event | Meaning |
| --- | --- |
| `BOOT` | Firmware started |
| `READY` | Firmware initialized |
| `BTN_PRESS` | Button press detected |
| `BTN_RELEASE` | Button release detected |
| `SHORT_PRESS` | Short press detected |
| `LONG_PRESS` | Long press detected |
| `LID_OPEN` | Lid switch changed to open |
| `LID_CLOSED` | Lid switch changed to closed |
| `OUT_CHANGE` | Output state changed |
| `CMD_RX` | Command received |
| `CMD_OK` | Command accepted |
| `CMD_ERR` | Command rejected |
| `SAFE_MODE` | Safe mode active |

Debug messages should be short and consistent.

---

## Error Handling

Firmware should detect and report errors when possible.

Possible errors:

- Unknown command
- Unsafe command
- Invalid output number
- Touchscreen communication timeout
- Interposer state mismatch
- Output fault if detectable
- Profile mismatch
- Watchdog reset
- Service lock active
- Button stuck active
- Lid input stuck or invalid

Errors should not cause random output behavior.

---

## Watchdog Support

A watchdog timer may be useful after the base firmware is stable.

Possible watchdog goals:

- Recover from firmware lockup
- Return outputs to safe states
- Avoid holding console reset
- Report watchdog reset after reboot
- Prevent endless unsafe command loops

Watchdog behavior must be tested carefully.

A watchdog reset should not create an unwanted button press or output pulse.

---

## Firmware Versioning

Firmware versions should be tracked clearly.

Suggested version format:

| Version Type | Example |
| --- | --- |
| Early prototype | `v0.0.1-prototype` |
| Bench test | `v0.1.0-bench` |
| Console test | `v0.2.0-console-test` |
| Stable test | `v0.5.0-alpha` |
| Product candidate | `v1.0.0-rc1` |

Each firmware release should list:

- Firmware version
- Hardware revision
- Supported board
- Major changes
- Known issues
- Test status

---

## Firmware File Naming

Suggested compiled firmware file names:

| File Name Example | Purpose |
| --- | --- |
| `button-butler-interposer-v0.1.0.hex` | Base interposer firmware |
| `button-butler-touchscreen-v0.1.0.uf2` | Touchscreen firmware if UF2-based |
| `button-butler-io-expansion-v0.1.0.hex` | I/O expansion firmware |
| `button-butler-test-blink-v0.0.1.hex` | Simple board test firmware |
| `button-butler-uart-test-v0.0.1.hex` | UART test firmware |

File names should make the target board and version clear.

---

## Hardware Revision Matching

Firmware should be matched to the correct hardware revision.

Each firmware build should document:

- Target board name
- Target board revision
- MCU part number
- Pinout version
- Required voltage
- Required oscillator settings
- Required configuration bits
- Known limitations

Do not assume one firmware build works on every board revision.

---

## Programming Methods

Possible programming methods depend on the selected MCU and touchscreen hardware.

Possible programming methods:

- PIC ICSP programmer
- USB bootloader
- UART bootloader
- SWD programmer
- USB drag-and-drop UF2
- Test pads
- Programming header
- Factory flashing before assembly

The final method should be documented per board.

---

## PIC Firmware Notes

For PIC-based boards, firmware notes should include:

- PIC part number
- Compiler or assembler
- IDE or build tool
- Configuration bits
- Oscillator setting
- Pin assignments
- Programming voltage requirements
- ICSP pinout
- Hex file location
- Flashing instructions
- Verification steps

PIC firmware should keep startup states safe.

---

## Touchscreen Firmware Notes

For touchscreen-based boards, firmware notes should include:

- Touchscreen module model
- Controller MCU
- Display driver
- Touch driver
- Build environment
- Required libraries
- Display orientation
- UART pin assignments
- Asset locations
- Firmware flashing method
- Known UI limitations

The touchscreen firmware should not be required for base console operation.

---

## Test Firmware

Simple test firmware is useful during board bring-up.

Possible test firmware:

| Test Firmware | Purpose |
| --- | --- |
| Blink test | Confirms MCU boots and LED pin works |
| UART test | Confirms serial TX/RX works |
| Button test | Reports button input over UART |
| Lid test | Reports lid switch state |
| Output test | Toggles outputs one at a time |
| Touchscreen ping test | Tests communication with display controller |
| Safe-state test | Confirms outputs default safely |

Test firmware should be clearly separated from normal firmware.

---

## Firmware Testing Checklist

Before using firmware in a real console, test:

- MCU boots reliably
- Outputs start in safe states
- Button input reads correctly
- Button debounce works
- Short press detection works
- Long press detection works
- Lid switch input works
- UART output works
- UART commands are rejected when invalid
- Output control works with dummy loads
- Console button pass-through works
- Console is not held in reset
- Power cycling does not cause random outputs
- Touchscreen commands do not bypass safety logic

Testing should be repeated after every major firmware change.

---

## Repository Rules For Firmware

Recommended repo rules:

- Keep source code organized by target board
- Do not mix test firmware with release firmware
- Document compiler/toolchain versions
- Document pin mappings
- Document configuration bits
- Keep generated files out of source folders where possible
- Label prototype firmware clearly
- Do not call firmware stable until it is tested in hardware
- Keep changelog notes for each firmware version

This helps avoid confusion as the project grows.

---

## Current Firmware Status

| Firmware Area | Status |
| --- | --- |
| PIC Button Interposer | Planned |
| Touchscreen Controller | Planned |
| I/O Expansion | Future concept |
| UART Protocol | Concept planning |
| Service Mode | Concept planning |
| Build Profiles | Future concept |
| Test Firmware | Planned |

This status should be updated as firmware is written and tested.

---

## Open Questions

Open firmware questions:

- Which PIC or MCU will be used for the first interposer?
- Should the first firmware be written in C or assembly?
- What is the safest default output state?
- Should the default behavior be hardware pass-through or firmware pass-through?
- How should the lid switch input be wired and interpreted?
- What UART baud rate should be used?
- Should commands be human-readable or binary?
- Should build profiles be compiled in or stored in memory?
- Should settings be saved in EEPROM or flash?
- Should the touchscreen controller act as the main hub?
- How should firmware updates be handled after installation?
- Should service mode require a physical condition, password, or both?

These should be answered through prototype testing.

---

## Risks

Possible firmware risks:

- Output glitches during startup
- Button debounce failure
- Console stuck in reset
- Touchscreen command bugs
- Unsafe output defaults
- Floating inputs
- Incorrect profile loaded
- Firmware lockup
- Watchdog reset side effects
- UART parsing bugs
- Customer confusion from too many button actions
- Hardware revision mismatch

Firmware should be developed slowly and tested carefully.

---

## Summary

The `Firmware/` folder tracks all firmware work for the FBD PS2 Button Butler project.

The first firmware goal is simple:

- Read the power/reset button
- Debounce the input
- Preserve safe console behavior
- Add lid switch detection
- Control one safe output
- Report status over UART

After the base interposer firmware is stable, the project can expand into touchscreen control, I/O expansion, build profiles, mod control, power management, and service diagnostics.

The firmware should always prioritize safe PS2 operation over advanced features.
