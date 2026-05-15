# PIC Button Interposer Firmware

## Overview

This folder contains firmware notes and source planning for the **PIC-based Button Interposer Board** used in the **FBD PS2 Button Butler** project.

The Button Interposer Board is the core controller for the project.

Its main job is to monitor the original PlayStation 2 power/reset button, decide what action should happen, and safely control the console-side button behavior or related mod-control outputs.

The touchscreen and I/O expansion boards are optional future layers.

The PIC Button Interposer firmware should work safely even when no touchscreen is installed.

---

## Purpose

The purpose of the PIC Button Interposer firmware is to provide safe and predictable control over the PS2 power/reset button behavior.

At a basic level, the firmware should:

- Read the physical PS2 power/reset button
- Debounce the button input
- Detect short press and long press behavior
- Read the lid switch input if installed
- Pass through normal button behavior when needed
- Block or redirect button behavior when needed
- Control one or more outputs for internal mods
- Report status over UART
- Accept simple service or touchscreen commands if enabled
- Keep all outputs in safe default states

This firmware is the foundation of Button Butler.

---

## Project Status

| Area | Status |
| --- | --- |
| Firmware concept | In planning |
| MCU selection | Not finalized |
| Pinout | Not finalized |
| Button behavior | Concept defined |
| UART protocol | Concept defined |
| Hardware prototype | Planned |
| Bench testing | Not started |
| Console testing | Not started |
| Customer-ready status | Not ready |

This firmware should be treated as experimental until tested on real hardware.

---

## Firmware Design Priorities

The firmware should follow these priorities:

1. Safe console behavior
2. Reliable button detection
3. Predictable startup states
4. Predictable output states
5. Simple logic
6. Easy debugging
7. Easy configuration
8. Touchscreen compatibility later
9. Serviceability
10. Repeatable behavior across builds

The firmware should not become overly complicated before the base hardware is proven.

---

## Core Responsibilities

The PIC firmware is responsible for:

- GPIO initialization
- Button input monitoring
- Button debounce
- Press timing
- Short press detection
- Long press detection
- Optional double press detection
- Lid switch input reading
- Console-side button output control
- Output state control
- UART debug reporting
- UART command handling
- Service mode behavior
- Safety lockouts
- Watchdog handling if used

The firmware should be simple enough to test one feature at a time.

---

## What The PIC Should Control

The PIC should directly handle only the signals that are part of the base interposer system.

Possible direct controls:

| Signal | Purpose |
| --- | --- |
| Button input | Reads the original physical PS2 power/reset button |
| Button output | Sends or blocks the button action to the console |
| Lid switch input | Allows lid-open alternate behavior |
| Output 1 | General mod control output |
| Output 2 | Spare output, LED, chime, or second mod output |
| UART TX | Sends status/debug data |
| UART RX | Receives commands |
| Service input | Optional jumper or installer input |
| Status LED | Optional local firmware/status indicator |

The exact pinout depends on the selected PIC.

---

## What The PIC Should Not Do First

The first firmware should not try to control everything.

Avoid adding these features too early:

- Full touchscreen menu logic
- Complex graphics
- Full BlueRetro communication
- Full SD2PSX communication
- Advanced power sequencing
- Laser feedback processing
- Large command parser
- Persistent configuration menus
- Complex multi-profile behavior
- Automatic behavior that has not been tested manually first

The first goal is to prove safe button behavior.

---

## Recommended Development Order

The firmware should be developed in small stages.

Recommended order:

1. Confirm the PIC boots
2. Confirm oscillator and timing
3. Set all outputs to safe inactive states
4. Blink a test LED if available
5. Read the physical button input
6. Send button state over UART
7. Add debounce
8. Detect button press and release
9. Detect short press
10. Detect long press
11. Add console-side output control
12. Test normal pass-through behavior
13. Test intentional blocking behavior
14. Add lid switch input
15. Add lid-open alternate behavior
16. Add one mod-control output
17. Add simple UART commands
18. Add service mode
19. Add touchscreen command support later

Each stage should be tested before moving to the next one.

---

## Minimum Viable Firmware

The minimum useful firmware should include:

- Safe startup pin states
- Button input read
- Button debounce
- Short press detection
- Long press detection
- Console output control
- One safe output
- UART debug reporting
- Safe default behavior after reset

This is enough to prove the Button Interposer concept.

---

## Suggested Folder Structure

| Path | Purpose |
| --- | --- |
| `Source/` | Main PIC source code |
| `Builds/` | Compiled test or release firmware files |
| `Notes/` | Development notes, pinout notes, and test notes |
| `README.md` | Firmware overview and planning document |

Additional folders can be added later if needed.

---

## Possible Source Files

The source structure will depend on the compiler and selected PIC, but a clean layout could include:

| File | Purpose |
| --- | --- |
| `main.c` | Main initialization and firmware loop |
| `config.h` | Build configuration, timing values, and feature flags |
| `pins.h` | Pin definitions and signal names |
| `button.c` | Button debounce and press detection |
| `button.h` | Button function declarations |
| `outputs.c` | Output control logic |
| `outputs.h` | Output function declarations |
| `uart.c` | UART transmit and receive handling |
| `uart.h` | UART function declarations |
| `commands.c` | UART command parser |
| `commands.h` | Command function declarations |
| `service.c` | Service mode behavior |
| `service.h` | Service function declarations |
| `profiles.c` | Future profile behavior |
| `profiles.h` | Future profile declarations |

For very small PICs, the final firmware may use fewer files.

---

## Pin Planning

The exact PIC pinout is not finalized yet.

Early pin planning should reserve pins for the core features.

| Signal | Direction | Purpose |
| --- | --- | --- |
| `BTN_IN` | Input | Reads the physical PS2 power/reset button |
| `BTN_OUT` | Output | Controls or passes button action to the console |
| `LID_IN` | Input | Reads lid switch state |
| `OUT1` | Output | General mod-control output |
| `OUT2` | Output | Spare output, status LED, chime, or second control output |
| `UART_TX` | Output | Sends debug/status data |
| `UART_RX` | Input | Receives commands |
| `SERVICE_IN` | Input | Optional service jumper or installer input |
| `MCLR` | Input | Programming/reset pin if used |
| `VCC` | Power | Logic power |
| `GND` | Power | Ground |

If the selected PIC has limited pins, some features may need to be optional.

---

## Pin Default Rules

All pins should start in safe states.

Recommended startup rules:

| Signal | Startup Behavior |
| --- | --- |
| `BTN_OUT` | Inactive or pass-through safe state |
| `OUT1` | Inactive |
| `OUT2` | Inactive |
| `UART_TX` | Idle state |
| `UART_RX` | Input |
| `BTN_IN` | Input with defined pull-up or pull-down |
| `LID_IN` | Input with defined pull-up or pull-down |
| `SERVICE_IN` | Input with defined pull-up or pull-down |

No output should briefly activate during startup.

---

## Button Input Logic

The firmware should monitor the physical power/reset button.

The button input should be debounced before the firmware acts on it.

Possible button states:

| State | Meaning |
| --- | --- |
| Idle | Button is not pressed |
| Debounce Press | Possible press detected |
| Pressed | Valid press confirmed |
| Held | Button remains pressed |
| Long Press | Long press threshold reached |
| Debounce Release | Possible release detected |
| Released | Valid release confirmed |
| Action Pending | Firmware decides what action to perform |

A state-machine approach is preferred.

---

## Button Debounce

Mechanical buttons can bounce when pressed or released.

Without debounce, one press may look like several presses.

Possible debounce methods:

- Timer-based debounce
- State-machine debounce
- Simple delay after edge detection
- Hardware RC filter plus firmware debounce
- Schmitt trigger input if needed

Firmware debounce should be tested on real hardware.

---

## Suggested Timing Values

Initial timing values can start simple.

| Parameter | Starting Value |
| --- | --- |
| Debounce time | 20 ms to 50 ms |
| Short press maximum | Less than 500 ms |
| Long press threshold | 2000 ms |
| Service hold threshold | 5000 ms |
| Double press window | 300 ms to 500 ms |
| Startup lockout | 500 ms to 2000 ms |

These values should be adjusted after real console testing.

---

## Short Press Behavior

A short press should normally preserve the expected PS2 behavior.

Possible short press behavior:

- Pass button press through to console
- Request normal power/reset action
- Wake touchscreen if installed
- Confirm a simple action
- Toggle one output only in special profiles

For most customer-facing builds, short press should remain close to stock behavior.

---

## Long Press Behavior

A long press can be used for alternate or protected behavior.

Possible long press behavior:

- Power off request
- Reset request
- Toggle ChipSlayer
- Toggle modchip control
- Trigger Bluetooth pairing
- Enter service mode
- Force safe mode
- Wake touchscreen menu

Long press behavior should not be easy to trigger accidentally.

---

## Lid Switch Behavior

The lid switch can act as a hidden modifier.

Possible lid switch logic:

| Lid State | Button Action | Possible Result |
| --- | --- | --- |
| Closed | Short press | Normal console behavior |
| Closed | Long press | Normal long-press behavior |
| Open | Short press | Alternate action |
| Open | Long press | Service or protected action |

Lid-open behavior is useful because it allows hidden controls without adding extra switches.

---

## Example Button Behavior Map

Early concept behavior:

| Console State | Lid State | Button Action | Result |
| --- | --- | --- | --- |
| Off / standby | Closed | Short press | Normal power on |
| On | Closed | Short press | Normal reset or factory behavior |
| On | Closed | Long press | Normal long-press behavior or power off |
| On | Open | Short press | Toggle `OUT1` |
| On | Open | Long press | Enter service mode or toggle protected output |
| Any | Any | UART command | Perform requested safe action |

This map is only an early concept.

Actual behavior must be verified on hardware.

---

## Output Control

The PIC may control one or more outputs.

Possible output functions:

- ChipSlayer enable or disable
- Modchip enable or disable
- Bluetooth audio pairing trigger
- Bluetooth audio enable
- BlueRetro reset
- RGB lighting enable
- Status LED
- Chime or buzzer output
- Accessory enable line

Outputs should have safe defaults.

---

## Output Types

Different outputs may need different electrical behavior.

Possible output types:

| Output Type | Use Case |
| --- | --- |
| Push-pull | Local logic where voltage is known |
| Open-drain style | Pulling a signal low safely |
| Pulse output | Reset or pairing trigger |
| Latched output | Enable or disable state |
| PWM output | LED brightness or buzzer tone |
| High-side control signal | Enables external power switch |
| Low-side MOSFET control | Drives LEDs or simple loads |

The firmware should match the output behavior to the hardware design.

---

## Output Safety Rules

Firmware output rules:

- Initialize outputs before enabling actions.
- Never leave outputs floating.
- Do not pulse reset lines on startup.
- Do not activate pairing triggers on startup.
- Do not toggle modchip state randomly.
- Do not allow invalid UART commands to change outputs.
- Do not let service mode actions run during normal mode.
- Always define what happens after firmware reset.
- Use confirmation for risky actions if touchscreen is involved.

Outputs should be predictable every time the PIC boots.

---

## UART Role

UART is used for debugging, status reporting, and future touchscreen communication.

Early UART use should be simple.

Possible UART functions:

- Report firmware boot
- Report ready state
- Report button press
- Report button release
- Report short press
- Report long press
- Report lid switch changes
- Report output changes
- Receive simple commands
- Return command acknowledgements
- Return firmware version

The UART protocol should be easy to read with a USB-to-serial adapter.

---

## Suggested UART Settings

Initial UART settings:

| Setting | Suggested Value |
| --- | --- |
| Baud rate | 115200 or 9600 |
| Data bits | 8 |
| Parity | None |
| Stop bits | 1 |
| Flow control | None |
| Logic level | Match selected hardware, likely 3.3V or 5V |

The final baud rate should be selected after confirming the PIC oscillator accuracy and hardware needs.

---

## UART Debug Messages

Possible debug messages:

| Message | Meaning |
| --- | --- |
| `BOOT` | Firmware started |
| `READY` | Firmware initialized safely |
| `BTN_PRESS` | Button press detected |
| `BTN_RELEASE` | Button release detected |
| `SHORT_PRESS` | Short press detected |
| `LONG_PRESS` | Long press detected |
| `LID_OPEN` | Lid switch reports open |
| `LID_CLOSED` | Lid switch reports closed |
| `OUT1_ON` | Output 1 turned on |
| `OUT1_OFF` | Output 1 turned off |
| `SERVICE_ON` | Service mode entered |
| `SERVICE_OFF` | Service mode exited |
| `ERR` | Error or rejected action |

Debug messages can be changed later as the protocol matures.

---

## UART Command Ideas

Possible early commands:

| Command | Purpose |
| --- | --- |
| `PING` | Check if firmware responds |
| `STATUS` | Request current firmware state |
| `BTN?` | Request button state |
| `LID?` | Request lid switch state |
| `OUT1 ON` | Turn output 1 on |
| `OUT1 OFF` | Turn output 1 off |
| `OUT1 TOGGLE` | Toggle output 1 |
| `OUT2 ON` | Turn output 2 on |
| `OUT2 OFF` | Turn output 2 off |
| `OUT2 TOGGLE` | Toggle output 2 |
| `PASS` | Force pass-through mode if supported |
| `SAFE` | Force safe mode if supported |
| `SERVICE ON` | Enter service mode |
| `SERVICE OFF` | Exit service mode |
| `VERSION?` | Request firmware version |

These are planning commands only.

Final commands should be documented in `Firmware/Protocol-Notes/`.

---

## UART Responses

Possible command responses:

| Response | Meaning |
| --- | --- |
| `OK` | Command accepted |
| `ERR` | Command rejected or failed |
| `UNKNOWN` | Command not recognized |
| `LOCKED` | Command is protected |
| `UNSAFE` | Command blocked for safety |
| `BUSY` | Firmware is busy |
| `STATE` | State data follows |
| `VERSION` | Firmware version data follows |

The touchscreen should only update status after receiving a valid response.

---

## Service Mode

Service mode is intended for testing and installation.

Possible service mode features:

- Read button state
- Read lid switch state
- Toggle outputs manually
- Pulse outputs manually
- Test status LED
- Test chime output
- Force pass-through mode
- Force safe mode
- Report firmware version
- Report active profile
- Report error state

Service mode should not be easy for a customer to enter accidentally.

---

## Possible Service Mode Entry Methods

Possible service mode entry methods:

| Method | Notes |
| --- | --- |
| UART command | Useful during development |
| Lid open plus long press | Useful for hidden installer access |
| Service jumper | Good for bench testing |
| Startup hold | Possible safe-mode entry |
| Touchscreen protected page | Future premium builds |

Early firmware can start with UART command or service jumper entry.

---

## Safe Mode

Safe mode should place the firmware into a conservative behavior state.

Possible safe mode behavior:

- Disable optional outputs
- Ignore risky commands
- Preserve normal button behavior
- Report safe mode over UART
- Prevent mod-control changes
- Allow status queries
- Allow service exit only through defined method

Safe mode is useful during testing and troubleshooting.

---

## Pass-Through Mode

Pass-through mode should allow the console button to behave normally.

Possible pass-through behavior:

- Physical button action reaches the console
- Optional outputs remain inactive
- UART still reports events
- Firmware does not perform alternate actions
- Service commands may still be available if enabled

Pass-through should be the safest default behavior when possible.

---

## Firmware Profiles

Future firmware may include build profiles.

Possible profiles:

| Profile | Purpose |
| --- | --- |
| Basic | Normal button pass-through and debug |
| ChipSlayer | Lid-open button shortcut controls ChipSlayer |
| Modchip | Modchip enable/disable behavior |
| Bluetooth Audio | Pairing and audio enable behavior |
| BlueRetro | BlueRetro enable/reset behavior |
| Touchscreen | Touchscreen command support enabled |
| Service/Test | Testing and output verification |

Early firmware can start with a single hardcoded profile.

---

## Configuration Storage

The first firmware may not need stored settings.

Future stored settings may include:

- Active profile
- Output default states
- Button timing values
- Service lock state
- Last output state
- Chime enable
- Touchscreen control enable
- Safe mode flag

Stored settings should never be allowed to make the console unusable.

A reset-to-safe-default method should exist if settings are stored.

---

## Watchdog

A watchdog timer may be used after basic firmware is stable.

Possible watchdog goals:

- Recover from firmware lockup
- Return outputs to safe states
- Avoid holding console reset
- Report watchdog reset after reboot
- Prevent endless command loops

Watchdog behavior must be tested carefully.

A watchdog reset should not create a false button press or output pulse.

---

## Startup Sequence

Recommended startup sequence:

1. MCU resets
2. Configure all outputs inactive
3. Configure all inputs
4. Enable internal pull-ups or pull-downs if used
5. Initialize timers
6. Initialize UART
7. Read initial button state
8. Read initial lid switch state
9. Load default profile or settings
10. Report `BOOT`
11. Report `READY`
12. Enter main loop

No optional output should activate during startup.

---

## Main Loop Concept

The firmware main loop may perform these tasks:

1. Read button input
2. Debounce button input
3. Read lid switch input
4. Update button state machine
5. Check timing thresholds
6. Process pending button actions
7. Process UART receive buffer
8. Execute safe commands
9. Update outputs
10. Report events
11. Feed watchdog if enabled

The main loop should avoid long blocking delays where possible.

---

## Event Reporting

The firmware may send event reports over UART.

Possible events:

| Event | Meaning |
| --- | --- |
| `EVT BOOT` | Firmware restarted |
| `EVT READY` | Firmware is ready |
| `EVT BTN_PRESS` | Button pressed |
| `EVT BTN_RELEASE` | Button released |
| `EVT SHORT_PRESS` | Short press detected |
| `EVT LONG_PRESS` | Long press detected |
| `EVT LID_OPEN` | Lid opened |
| `EVT LID_CLOSED` | Lid closed |
| `EVT OUT1_CHANGE` | Output 1 changed |
| `EVT OUT2_CHANGE` | Output 2 changed |
| `EVT SERVICE_ON` | Service mode entered |
| `EVT SAFE_MODE` | Safe mode entered |
| `EVT ERROR` | Error occurred |

Event messages help with debugging and touchscreen updates.

---

## Error Handling

The firmware should handle errors predictably.

Possible errors:

| Error | Possible Meaning |
| --- | --- |
| Unknown command | UART command not recognized |
| Unsafe command | Command blocked by safety rule |
| Locked command | Command requires service mode |
| Button stuck | Button input stayed active too long |
| Lid input invalid | Lid input state is unexpected |
| Output fault | Output state did not match expected value if feedback exists |
| Watchdog reset | Firmware restarted unexpectedly |
| Profile error | Invalid or unsupported profile |

Errors should be reported but should not cause random output behavior.

---

## Build And Toolchain Notes

The exact toolchain depends on the selected PIC.

Possible tools:

- MPLAB X
- XC8 compiler
- MPASM for older PIC assembly projects
- VS Code with external build tools
- PICkit programmer
- SNAP programmer
- Other compatible ICSP programmer

The final toolchain should be documented after the MCU is selected.

---

## PIC Selection Notes

The selected PIC should support the required firmware features.

Useful PIC traits:

- Enough GPIO pins
- Hardware UART if possible
- Internal oscillator
- Reliable timer resources
- Watchdog timer
- EEPROM or flash storage if settings are needed
- Easy programming support
- Available in small packages
- Available from common suppliers
- Enough program memory for command handling

The first version should avoid choosing a PIC that is too limited unless the firmware is intentionally simple.

---

## Possible PIC Requirements

Minimum useful requirements:

| Requirement | Reason |
| --- | --- |
| 1 button input | Physical power/reset button |
| 1 console output | Button pass-through or control |
| 1 lid switch input | Hidden modifier behavior |
| 2 outputs | Mod control and spare/status |
| UART TX/RX | Debug and touchscreen communication |
| Timer | Debounce and press timing |
| Watchdog | Optional reliability feature |
| Programming pins | Firmware flashing and development |

More pins will make development easier.

---

## Hardware Revision Matching

Firmware must match the board revision.

Each firmware build should document:

- Board name
- Board revision
- PIC part number
- Pin mapping
- Clock settings
- Voltage level
- UART settings
- Supported features
- Known issues

Do not flash firmware made for one pinout onto a different board revision without checking it first.

---

## Firmware Versioning

Suggested firmware version format:

| Version | Meaning |
| --- | --- |
| `v0.0.1` | First bring-up test |
| `v0.1.0` | Button input and UART debug |
| `v0.2.0` | Debounce and press detection |
| `v0.3.0` | Console output control |
| `v0.4.0` | Lid switch and output control |
| `v0.5.0` | Early console test firmware |
| `v1.0.0` | First stable release candidate |

Use clear version numbers so test results can be matched to firmware behavior.

---

## Firmware File Naming

Suggested compiled firmware names:

| File Name | Purpose |
| --- | --- |
| `button-butler-pic-interposer-v0.0.1.hex` | First test firmware |
| `button-butler-pic-interposer-v0.1.0-uart.hex` | UART test build |
| `button-butler-pic-interposer-v0.2.0-button.hex` | Button test build |
| `button-butler-pic-interposer-v0.3.0-output.hex` | Output control test |
| `button-butler-pic-interposer-v0.5.0-console-test.hex` | Real console test build |

Compiled firmware should be clearly marked as prototype, test, or release candidate.

---

## Programming Notes

PIC programming notes should eventually include:

- Programmer model
- Programming voltage
- ICSP pinout
- Required connections
- Configuration bits
- Fuse settings
- Oscillator setting
- Brown-out reset setting
- Watchdog setting
- Verification steps

Programming instructions should be added after the PIC is selected.

---

## ICSP Planning

If ICSP is used, the board should expose the needed pins.

Typical ICSP signals may include:

| Signal | Purpose |
| --- | --- |
| VPP / MCLR | Programming voltage or reset |
| VDD | Target power reference |
| VSS | Ground |
| PGD | Programming data |
| PGC | Programming clock |
| AUX | Optional, depending on programmer and device |

The exact ICSP pinout depends on the selected PIC.

---

## Test Firmware Ideas

Simple test builds should be created before the full firmware.

Possible test builds:

| Test Firmware | Purpose |
| --- | --- |
| Blink test | Confirms MCU boots |
| UART hello test | Confirms UART TX works |
| UART echo test | Confirms UART RX and TX work |
| Button monitor | Reports button state over UART |
| Lid monitor | Reports lid switch state over UART |
| Output toggle | Toggles outputs for board testing |
| Safe state test | Confirms outputs stay inactive during startup |
| Service mode test | Tests service commands |

These test builds should be clearly separated from the main firmware.

---

## Bench Testing Checklist

Before testing in a PS2 console, bench test:

- Correct supply voltage
- Correct current draw
- PIC boots
- UART output works
- Button input reads correctly
- Lid switch input reads correctly
- Outputs default inactive
- Outputs can be toggled intentionally
- No output glitches during power-up
- No output glitches during reset
- UART commands are rejected when invalid
- Watchdog reset does not create unsafe outputs if enabled

Bench testing should happen before console testing.

---

## Console Testing Checklist

Before using firmware in a real console build, test:

- Console powers on normally
- Console resets normally
- Button pass-through works
- Button blocking works only when intended
- Short press behavior is correct
- Long press behavior is correct
- Lid-open behavior is correct
- Outputs do not affect boot unless intended
- UART still reports correctly inside the console
- No random resets occur
- No backfeeding is detected
- Shell can close without changing behavior
- Firmware recovers safely after power cycling

Do not use prototype firmware in a customer console without testing.

---

## Touchscreen Compatibility

The PIC firmware should eventually support touchscreen commands.

The touchscreen should be able to request actions such as:

- Request status
- Request power action
- Request reset action
- Toggle output
- Enter service mode
- Exit service mode
- Request firmware version

The PIC should approve or reject each command based on safety rules.

The touchscreen should not bypass the PIC for critical console control.

---

## Future Expansion

Future PIC firmware features may include:

- More button profiles
- Stored settings
- Service mode lockout
- Watchdog reporting
- Output state feedback
- Touchscreen command protocol
- Build ID reporting
- Error logging
- Safe mode entry
- Firmware update support if possible
- I/O expansion communication

These features should be added only after the base firmware is stable.

---

## Documentation Requirements

Every firmware revision should document:

- Firmware version
- Target PIC
- Target board revision
- Pinout
- UART settings
- Button timing values
- Default output states
- Supported commands
- Known issues
- Test status
- Date tested
- Console model tested if applicable

This makes it easier to track problems across revisions.

---

## Known Issues

Known issues should be listed here as development begins.

Current known issues:

- PIC part number is not finalized.
- Board pinout is not finalized.
- Hardware pass-through method is not finalized.
- UART baud rate is not finalized.
- Output electrical style is not finalized.
- Firmware has not been bench tested.
- Firmware has not been console tested.

---

## Open Questions

Open firmware questions:

- Which PIC should be used for the first prototype?
- Should the default button behavior be hardware pass-through or firmware-controlled pass-through?
- Should `BTN_OUT` be open-drain style, analog switch controlled, relay controlled, or another method?
- What is the safest default state if the PIC is unpowered?
- Should the lid switch input be required or optional?
- How many outputs are needed on the first board?
- Should UART be required on the base board?
- Should settings be stored in EEPROM or compiled into firmware?
- Should service mode be entered by UART, jumper, button hold, or touchscreen?
- Should output states persist after power loss?
- Should long press behavior be customer-facing or installer-only?

These questions should be answered through hardware testing.

---

## Risks

Possible firmware risks:

- Button input not debounced correctly
- Console stuck in reset
- Console unable to power on
- Output glitch during startup
- Mod control output activates unexpectedly
- UART command parser accepts bad commands
- Touchscreen sends unsafe commands
- Firmware lockup
- Watchdog reset causes unwanted action
- Wrong firmware flashed to wrong board revision
- Customer confusion from too many button actions

The safest path is to test each feature separately.

---

## Summary

The PIC Button Interposer firmware is the core firmware for Button Butler.

Its first job is simple:

- Read the PS2 power/reset button
- Debounce the input
- Detect press types
- Preserve safe console behavior
- Read the lid switch if used
- Control one safe output
- Report status over UART

After that behavior is reliable, the firmware can expand into service mode, touchscreen command support, build profiles, mod control, and advanced diagnostics.

The firmware should always prioritize safe PS2 operation over extra features.
