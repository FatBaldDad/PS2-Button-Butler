# Touchscreen Controller Firmware

## Overview

This folder contains firmware planning notes for the **Touchscreen Controller** used in the **FBD PS2 Button Butler** project.

The touchscreen controller is the optional premium user interface layer for Button Butler.

The base Button Butler system should work without the touchscreen. The touchscreen adds a clean visual interface for controlling internal PS2 mods, viewing status, accessing service pages, and sending safe command requests to the Button Interposer Board.

The touchscreen should act as the user interface.

The Button Interposer Board should remain responsible for critical console power/reset behavior.

---

## Purpose

The purpose of the touchscreen firmware is to give premium FBD PS2 builds a clean control panel for internal mods.

Possible touchscreen functions:

- Show console state
- Show Button Butler connection state
- Request console power actions
- Request console reset actions
- Control ChipSlayer
- Control modchip enable/disable behavior
- Control Bluetooth audio
- Trigger Bluetooth pairing
- Control BlueRetro enable/reset/channel behavior
- Show SD2PSX status if available
- Control RGB lighting
- Control video board power
- Show service and diagnostic information
- Display build-specific branding
- Display firmware and hardware information

The touchscreen should make the build easier to use, not harder.

---

## Project Status

| Area | Status |
| --- | --- |
| Touchscreen concept | In planning |
| Display hardware | Not finalized |
| Touchscreen MCU | Not finalized |
| UI layout | Concept planning |
| UART protocol | Concept planning |
| Interposer communication | Planned |
| Build profiles | Future concept |
| Bench testing | Not started |
| Console testing | Not started |
| Customer-ready status | Not ready |

This firmware should be treated as experimental until the hardware and communication protocol are tested.

---

## Design Priorities

The touchscreen firmware should follow these priorities:

1. Keep the main screen simple
2. Show clear status
3. Request actions instead of directly forcing them
4. Wait for interposer confirmation
5. Hide unused features
6. Protect service functions
7. Avoid accidental power/reset actions
8. Keep the UI responsive
9. Support build-specific profiles later
10. Keep the console usable even if the touchscreen fails

The touchscreen should never become a single point of failure for basic console operation.

---

## Relationship To The Button Interposer

The touchscreen controller should communicate with the Button Interposer Board.

Recommended relationship:

| Device | Role |
| --- | --- |
| Button Interposer Board | Handles PS2 button behavior, safety rules, and critical outputs |
| Touchscreen Controller | Displays status and sends user command requests |
| I/O Expansion Board | Provides extra outputs and inputs if installed |
| Power Management Board | Controls accessory power if installed |

The touchscreen should not directly drive the PS2 reset or power button circuit unless a future design specifically proves that safe.

---

## Control Philosophy

The touchscreen should use a request-and-confirm model.

Basic flow:

| Step | Action |
| --- | --- |
| 1 | User taps a touchscreen button |
| 2 | Touchscreen sends a command request |
| 3 | Interposer checks safety rules |
| 4 | Interposer accepts or rejects the command |
| 5 | Interposer sends a response |
| 6 | Touchscreen updates the display based on the response |

The touchscreen should not assume an action worked until the interposer confirms it.

---

## Why The Interposer Stays In Charge

The interposer stays in charge because it is connected to the PS2 button circuit.

The interposer should handle:

- Power/reset button input
- Button debounce
- Lid switch input
- Console-side button output
- Safety lockouts
- Output default states
- Pass-through behavior
- Safe mode
- Service mode
- Command validation

The touchscreen is the interface. The interposer is the safety controller.

---

## Suggested Folder Structure

| Path | Purpose |
| --- | --- |
| `Source/` | Main touchscreen firmware source code |
| `Assets/` | Images, icons, logos, splash screens, and UI graphics |
| `Builds/` | Compiled firmware builds if shared |
| `Notes/` | Development notes, hardware notes, and UI notes |
| `README.md` | Touchscreen firmware overview |

Additional folders can be added later as the firmware becomes real.

---

## Possible Source Files

The source structure depends on the touchscreen controller hardware.

A possible layout could include:

| File | Purpose |
| --- | --- |
| `main.c` or `main.cpp` | Main firmware entry point |
| `config.h` | Build configuration and feature flags |
| `display.c` | Display driver wrapper |
| `display.h` | Display function declarations |
| `touch.c` | Touch input handling |
| `touch.h` | Touch function declarations |
| `ui.c` | Main UI logic |
| `ui.h` | UI declarations |
| `pages.c` | Page switching and page behavior |
| `pages.h` | Page declarations |
| `uart.c` | UART communication |
| `uart.h` | UART declarations |
| `commands.c` | Command send/receive handling |
| `commands.h` | Command declarations |
| `profile.c` | Build profile handling |
| `profile.h` | Profile declarations |
| `assets.h` | References to UI assets |
| `service.c` | Service page logic |
| `service.h` | Service declarations |

The first firmware should stay much simpler than this.

---

## Recommended Development Order

The touchscreen firmware should be developed in small stages.

Recommended order:

1. Bring up the display
2. Confirm the backlight works
3. Confirm touch input works
4. Draw a basic title screen
5. Draw one button
6. Detect one touch action
7. Send a UART `PING`
8. Receive a UART response
9. Request interposer status
10. Display button and lid state
11. Add one safe output toggle
12. Add command acknowledgement handling
13. Add error display
14. Add main page
15. Add service/test page
16. Add mod control pages later

Do not start with the full menu system.

Start with one screen, one button, and one command.

---

## Minimum Viable Touchscreen Firmware

The minimum useful touchscreen firmware should include:

- Display startup
- Touch input detection
- Basic main screen
- UART communication with interposer
- Connection status
- Firmware version display
- Button state display if available
- Lid state display if available
- One safe test command
- Command accepted/rejected feedback
- Basic error message display

This is enough to prove the touchscreen concept.

---

## Display Hardware Notes

The display hardware is not finalized yet.

Possible display types:

- Small round touchscreen
- Round display with separate touch input
- Waveshare-style round touchscreen
- RP2040-based display module
- RP2350-based display module
- ESP32-based display module
- Custom future FBD display board

The first hardware should be chosen for easy testing, clean mounting, and good documentation.

---

## Preferred Display Traits

Useful touchscreen traits:

- Small size
- Clean round appearance
- Good brightness
- Reasonable power draw
- Reliable touch input
- Good documentation
- Easy firmware flashing
- Available display library support
- Easy UART access
- Mountable inside or on a PS2 shell

A round display is attractive because it can look intentional on a custom PS2 build.

---

## Touchscreen Power Behavior

The touchscreen should not need to run at full brightness all the time.

Possible power states:

| State | Description |
| --- | --- |
| Full On | Screen and touch input active |
| Dimmed | Screen visible but reduced brightness |
| Display Off | Controller awake, display/backlight off |
| Sleep | Low-power mode if supported |
| Wake | Touchscreen returns to active state |

The final behavior depends on the display hardware.

---

## Wake Behavior

Possible wake triggers:

- Touch input
- Physical power/reset button press
- UART event from interposer
- Console power state change
- Service event
- Error or warning event
- Lid switch state change if useful

The touchscreen should wake cleanly without sending accidental commands.

---

## Startup Behavior

Touchscreen startup should be predictable.

Recommended startup sequence:

| Step | Action |
| --- | --- |
| 1 | Initialize display hardware |
| 2 | Initialize touch input |
| 3 | Initialize UART |
| 4 | Show FBD or Button Butler splash screen |
| 5 | Load build profile |
| 6 | Send `PING` to interposer |
| 7 | Wait for response |
| 8 | Request status |
| 9 | Draw main page |
| 10 | Show warning if interposer is not detected |

The touchscreen should not send risky commands automatically during startup.

---

## Shutdown Behavior

Touchscreen shutdown should be conservative.

Recommended shutdown behavior:

- Do not send new risky commands during unstable power
- Stop accepting touch input if power is unstable
- Save settings only when power is stable
- Dim or turn off display if requested
- Avoid backfeeding other boards through UART lines
- Leave critical console behavior to the interposer

Hardware design is also needed for safe shutdown behavior.

---

## Main Page Concept

The main page should show the most important information.

Possible main page items:

- FBD logo or build name
- Button Butler connection status
- Console state if known
- Power button
- Reset button
- ChipSlayer state
- Modchip state
- BlueRetro state
- Bluetooth audio state
- RGB state
- Video power state
- Warning icon if needed

The main page should be clean and easy to understand.

---

## Main Page Rules

Recommended main page rules:

- Keep text short
- Use large touch targets
- Avoid clutter
- Hide unsupported features
- Show unknown states clearly
- Confirm risky actions
- Avoid tiny buttons near the edge
- Use clear on/off states
- Keep navigation simple

A small screen needs a simple UI.

---

## Possible Page List

Future touchscreen pages may include:

| Page | Purpose |
| --- | --- |
| Main | Basic status and quick controls |
| Power | Console and accessory power controls |
| Reset | Reset actions and confirmations |
| Mods | Modchip and ChipSlayer controls |
| Controllers | BlueRetro controls |
| Audio | Bluetooth audio controls |
| Storage | SD2PSX status or information |
| Lighting | RGB lighting controls |
| Video | RetroGEM or ElectronAnalog power controls |
| Service | Installer and test functions |
| Diagnostics | Live status and error information |
| Settings | Display and behavior settings |
| About | Build ID, firmware, and project information |

Not every build needs every page.

---

## Power Page

The power page may include:

- Console power request
- Console power-off request
- Restart request
- Accessory power states
- Video power state
- Router power state
- Manual override controls
- Confirmation for risky actions

Power actions should require confirmation when appropriate.

---

## Reset Page

The reset page may include:

- Console reset request
- BlueRetro reset request
- Bluetooth audio reset request
- Router reset request
- Button Butler subsystem reset if supported
- Confirmation for risky reset actions

Reset actions should be clearly labeled.

---

## Mods Page

The mods page may include:

- ChipSlayer enable/disable
- Modchip enable/disable
- Modchip next-boot options if supported
- Current mod state
- Unknown state warning
- Confirmation before risky changes

Modchip-related actions should be protected.

---

## Controllers Page

The controller page may include BlueRetro controls.

Possible controls:

- BlueRetro enable
- BlueRetro disable
- BlueRetro reset
- Channel 1 enable/disable
- Channel 2 enable/disable
- Pairing status if available
- Controller status if available

Early firmware may only support simple enable and reset controls.

---

## Audio Page

The audio page may include Bluetooth audio controls.

Possible controls:

- Bluetooth audio on
- Bluetooth audio off
- Pairing mode
- Reset audio board
- Connected status if available
- Pairing status if available
- Audio board power state

Pairing should usually be a pulse command, not a latched state.

---

## Storage Page

The storage page may show SD2PSX or memory card emulator information.

Possible display items:

- SD2PSX detected
- SD card detected
- Current Game ID if available
- Active card/profile if available
- Save activity warning if available
- Firmware version if available
- Error or warning state

Early firmware should treat SD2PSX as display/status only unless safe control is confirmed.

---

## Lighting Page

The lighting page may control RGB or status lighting.

Possible controls:

- Lighting on
- Lighting off
- Brightness
- Mode
- Status lighting
- Gameplay lighting
- Service lighting
- Restore default lighting

Early firmware may only need on/off control.

---

## Video Page

The video page may control internal video board power.

Possible controls:

- Video board power on
- Video board power off
- Auto power saving
- Manual override
- Power-good state
- Warning if video power is disabled

This page may apply to RetroGEM, ElectronAnalog, or other video hardware.

Video power actions should be protected because they can blank the display.

---

## Service Page

The service page is for installer and development use.

Possible service functions:

- Read all inputs
- Read all outputs
- Test output 1
- Test output 2
- Test ChipSlayer output
- Test Bluetooth pairing output
- Test RGB output
- Test video power output
- View firmware version
- View hardware revision
- Enter safe mode
- Exit service mode
- Show UART connection state

Service pages should not be easy to open by accident.

---

## Diagnostics Page

The diagnostics page may show live system information.

Possible diagnostics:

- Button state
- Lid switch state
- Service jumper state
- Output states
- Active profile
- Safe mode state
- Service mode state
- Last command sent
- Last response received
- Last error
- UART link state
- Firmware uptime
- Watchdog reset count if available

Diagnostics are useful during development and service.

---

## Settings Page

The settings page may eventually include:

- Brightness
- Sleep timeout
- Theme
- Chime enable/disable
- Build profile
- Button behavior profile
- Service lock setting
- Display orientation
- Startup page
- Restore defaults

Settings should be added only after the base UI is stable.

Stored settings should not create unsafe behavior.

---

## About Page

The about page may show:

- FBD PS2 Button Butler
- Touchscreen firmware version
- Interposer firmware version if connected
- Hardware revision
- Protocol version
- Build ID
- Console model
- Installed feature profile
- FBD branding

This helps identify what is installed in a specific console.

---

## UI Profiles

Touchscreen profiles can hide unused features.

Possible profiles:

| Profile | Purpose |
| --- | --- |
| Basic | Power/reset and status only |
| ChipSlayer | ChipSlayer control page enabled |
| Modchip | Modchip control page enabled |
| Bluetooth Audio | Audio page enabled |
| BlueRetro | Controller page enabled |
| SD2PSX | Storage page enabled |
| Premium | Most pages enabled |
| Service/Test | Development and diagnostic pages enabled |

Profiles keep the UI clean.

---

## Basic Profile

The basic profile should be simple.

Possible pages:

- Main
- Power
- About

Possible features:

- Connection status
- Console status if available
- Power request
- Reset request
- Firmware version

This is useful for the first touchscreen prototype.

---

## Premium Profile

The premium profile is for advanced FBD builds.

Possible pages:

- Main
- Power
- Mods
- Controllers
- Audio
- Storage
- Lighting
- Video
- Service
- Diagnostics
- Settings
- About

This profile should only show features that are actually installed.

---

## Service/Test Profile

The service/test profile is for development.

Possible pages:

- Input test
- Output test
- UART test
- Button timing
- Lid switch
- Error log
- Firmware info
- Safe mode

This profile should not be the default customer-facing mode.

---

## Touch Input Rules

Touch input should be filtered to avoid accidental actions.

Recommended rules:

- Ignore touches during startup
- Ignore touches while changing pages
- Require release before accepting another press
- Use large touch targets
- Confirm risky actions
- Avoid critical buttons near screen edges
- Debounce touch input
- Disable buttons when a command is pending
- Display feedback after every action

Small touchscreens need forgiving UI design.

---

## Confirmation Rules

Some actions should require confirmation.

Actions that may need confirmation:

- Power off console
- Reset console
- Disable modchip
- Change modchip state
- Turn video power off
- Restore defaults
- Enter service mode
- Save persistent settings
- Change build profile
- Disable important accessory power

Confirmation can be a second tap, hold-to-confirm, or separate confirmation page.

---

## Protected Actions

Protected actions should not be available from normal accidental touches.

Possible protected actions:

- Service mode
- Modchip control
- Restore defaults
- Firmware update mode
- Dangerous power actions
- Advanced diagnostic functions
- Persistent profile changes

Protection methods:

- Long press
- Hidden corner hold
- Lid-open requirement
- Touchscreen PIN
- Service jumper
- UART unlock command

The protection method should match the risk.

---

## UART Communication

The touchscreen will likely communicate with the Button Interposer Board over UART.

Possible UART uses:

- Send command requests
- Receive command responses
- Receive event messages
- Request status
- Request firmware version
- Request feature list
- Report touchscreen status if needed

The touchscreen should follow the protocol notes in:

- `Firmware/Protocol-Notes/README.md`
- `Firmware/Protocol-Notes/UART-Command-Ideas.md`
- `Firmware/Protocol-Notes/IO-Command-Ideas.md`

---

## Suggested UART Settings

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

The final settings should match the interposer firmware.

---

## Minimum UART Commands

The touchscreen should first support only a small command set.

Minimum useful commands:

| Command | Purpose |
| --- | --- |
| `PING` | Confirm interposer communication |
| `STATUS` | Request current state |
| `BTN?` | Read button state |
| `LID?` | Read lid switch state |
| `OUT1?` | Read output 1 state |
| `OUT1 ON` | Turn output 1 on |
| `OUT1 OFF` | Turn output 1 off |
| `VERSION?` | Read firmware version |

This is enough for early touchscreen testing.

---

## Command Response Handling

The touchscreen should handle all command responses clearly.

Possible responses:

| Response | Touchscreen Behavior |
| --- | --- |
| `OK` | Show action accepted and update state |
| `ERR` | Show error message |
| `UNKNOWN` | Show unsupported command warning |
| `LOCKED` | Show locked/protected message |
| `UNSAFE` | Show safety warning |
| `BUSY` | Show busy/pending state |
| `STATE` | Update displayed status |
| `VALUE` | Update one displayed value |
| `VERSION` | Update version/about page |

The touchscreen should not silently ignore errors.

---

## Event Handling

The touchscreen may receive event messages from the interposer.

Possible events:

| Event | Touchscreen Behavior |
| --- | --- |
| `EVT BOOT` | Show controller restarted if needed |
| `EVT READY` | Mark interposer as connected |
| `EVT BTN_PRESS` | Update button state |
| `EVT BTN_RELEASE` | Update button state |
| `EVT LID_OPEN` | Update lid icon/status |
| `EVT LID_CLOSED` | Update lid icon/status |
| `EVT OUT1 ON` | Update output state |
| `EVT OUT1 OFF` | Update output state |
| `EVT ERROR` | Show warning or diagnostic entry |

Events help the UI stay current.

---

## Connection Status

The touchscreen should show whether the interposer is connected.

Possible connection states:

| State | Meaning |
| --- | --- |
| Connected | Interposer responded normally |
| Connecting | Waiting for first response |
| Disconnected | No response received |
| Error | Invalid or unexpected data |
| Version mismatch | Firmware/protocol mismatch |
| Service mode | Interposer is in service mode |
| Safe mode | Interposer is in safe mode |

Connection status should be visible or easy to find.

---

## Error Handling

The touchscreen should handle errors in a user-friendly way.

Possible errors:

- Interposer not detected
- UART timeout
- Command rejected
- Command locked
- Unsafe command blocked
- Unknown command
- Feature unavailable
- Profile mismatch
- Firmware version mismatch
- Touch input error
- Display initialization error

The UI should show short, clear error messages.

---

## Example Error Messages

| Error | Possible Message |
| --- | --- |
| Interposer not detected | Button Butler controller not detected |
| UART timeout | No response from controller |
| Command locked | This action is locked |
| Unsafe command | Action blocked for safety |
| Unknown command | Command not supported |
| Feature unavailable | Feature not installed |
| Version mismatch | Firmware version mismatch |
| Profile mismatch | Build profile mismatch |

Messages should be short because the screen is small.

---

## Visual Feedback

Every touch action should provide visual feedback.

Possible feedback:

- Button highlight
- Loading indicator
- Accepted message
- Rejected message
- State icon change
- Warning icon
- Disabled button state
- Temporary confirmation banner

The user should know whether the system reacted.

---

## Audio Or Chime Feedback

The touchscreen may request chime feedback from the interposer or an I/O board.

Possible chime uses:

- Touch accepted
- Command accepted
- Command rejected
- Service mode entered
- Bluetooth pairing started
- Error warning
- Startup sound

Chime feedback should be optional.

---

## Asset Planning

The `Assets/` folder may contain:

- FBD logo
- Button Butler logo
- Splash screen images
- Icons
- Page backgrounds
- Build-specific graphics
- Font files if legally allowed to include
- UI mockups
- Warning symbols

Do not include font files unless the license allows it.

Do not include copyrighted graphics without permission.

---

## Icon Ideas

Possible UI icons:

| Icon | Meaning |
| --- | --- |
| Power | Console power |
| Reset | Reset action |
| Shield | ChipSlayer or protection |
| Chip | Modchip |
| Bluetooth | Bluetooth audio |
| Controller | BlueRetro |
| Card | SD2PSX or memory card |
| Light | RGB lighting |
| Video | HDMI/video board |
| Wrench | Service |
| Warning | Error or unsafe state |
| Info | About page |

Icons should be simple and readable at small sizes.

---

## Theme Ideas

Future themes may include:

- FBD default
- Blue Steel
- Ultra Slim
- Steampunk
- Minimal dark
- Service/test
- Build-specific customer theme

Themes should not delay functional development.

The first UI should focus on control and status.

---

## Display Orientation

The firmware should eventually support display orientation settings if the hardware allows it.

Possible orientations:

- 0 degrees
- 90 degrees
- 180 degrees
- 270 degrees

This helps with mounting the touchscreen in different console layouts.

---

## Brightness Control

Brightness control may be useful.

Possible brightness settings:

- Low
- Medium
- High
- Auto dim
- Service full brightness
- Sleep brightness

Brightness should balance readability, heat, and power draw.

---

## Sleep Timeout

The touchscreen may support a sleep timeout.

Possible timeout settings:

| Setting | Description |
| --- | --- |
| Off | Screen never sleeps |
| 30 seconds | Quick sleep |
| 1 minute | Short timeout |
| 5 minutes | Longer timeout |
| Service mode | Stay awake during service |

The screen should wake reliably after sleeping.

---

## Build Branding

The touchscreen can show build-specific branding.

Possible branding items:

- FBD logo
- Button Butler logo
- Build name
- Build ID
- Console model
- Hardware revision
- Firmware version
- Installed feature list

This can make premium builds feel more finished.

---

## Firmware Versioning

Suggested touchscreen firmware version format:

| Version | Meaning |
| --- | --- |
| `v0.0.1` | First display bring-up |
| `v0.1.0` | Touch input test |
| `v0.2.0` | UART communication test |
| `v0.3.0` | Basic main page |
| `v0.4.0` | Output control test |
| `v0.5.0` | Early interposer integration |
| `v1.0.0` | First stable release candidate |

Each firmware build should document the target hardware and tested features.

---

## Firmware File Naming

Suggested compiled firmware names:

| File Name | Purpose |
| --- | --- |
| `button-butler-touchscreen-v0.0.1-display-test.uf2` | Display bring-up test |
| `button-butler-touchscreen-v0.1.0-touch-test.uf2` | Touch input test |
| `button-butler-touchscreen-v0.2.0-uart-test.uf2` | UART test firmware |
| `button-butler-touchscreen-v0.3.0-main-page.uf2` | Basic UI test |
| `button-butler-touchscreen-v0.5.0-interposer-test.uf2` | Interposer communication test |

The file extension depends on the selected controller.

---

## Hardware Revision Matching

Touchscreen firmware should document the target hardware.

Each build should list:

- Display module
- Controller MCU
- Display driver
- Touch driver
- Pinout
- UART pins
- Power voltage
- Display resolution
- Orientation
- Hardware revision
- Known limitations

Do not assume one firmware build works on every touchscreen module.

---

## Programming Methods

Possible programming methods depend on the selected touchscreen controller.

Possible methods:

- USB drag-and-drop UF2
- USB bootloader
- SWD programmer
- UART bootloader
- Vendor flashing tool
- Arduino-style upload
- PlatformIO upload
- MicroPython or CircuitPython copy if used

The final method should be documented after the hardware is selected.

---

## Test Firmware Ideas

Simple test firmware should be created before the full UI.

Possible test firmware:

| Test Firmware | Purpose |
| --- | --- |
| Display color test | Confirms display driver works |
| Touch coordinate test | Confirms touch input works |
| Backlight test | Confirms brightness control |
| UART ping test | Confirms serial communication |
| Button draw test | Confirms touch buttons |
| Page switch test | Confirms navigation |
| Status display test | Shows interposer status |
| Error display test | Confirms warning messages |

These tests should be clearly separated from release firmware.

---

## Bench Testing Checklist

Before installing in a console, bench test:

- Display powers up
- Backlight works
- Touch input works
- Touch coordinates are correct
- UI buttons are readable
- UART TX works
- UART RX works
- `PING` command works
- `STATUS` request works
- Error message displays correctly
- Sleep/wake behavior works if implemented
- Power draw is acceptable
- Display does not overheat
- No accidental commands are sent at startup

Bench testing should happen before console installation.

---

## Console Testing Checklist

Before using in a console build, test:

- Touchscreen powers from intended rail
- Interposer communication works inside the console
- Display does not cause noise issues
- Touchscreen wiring does not interfere with shell closure
- Touch input still works after mounting
- Main page is readable
- Buttons are not too small
- Power/reset requests are confirmed by interposer
- Error messages display correctly
- Sleep/wake behavior works
- No backfeeding through UART
- Touchscreen failure does not stop normal console operation

Do not use untested touchscreen firmware in a customer build.

---

## UI Testing Checklist

UI testing should include:

- Main page readability
- Button size
- Page navigation
- Confirmation dialogs
- Error messages
- Disabled button states
- Service page protection
- Settings page behavior
- Touch accuracy
- Accidental edge touches
- Behavior after sleep/wake
- Behavior after interposer disconnect
- Behavior after interposer restart

The UI should be simple enough to use without explanation for basic functions.

---

## Safety Rules

Touchscreen firmware should follow strict safety rules.

Recommended rules:

- Do not send power/reset commands during startup.
- Do not assume a command succeeded.
- Do not expose service tools by default.
- Do not allow accidental reset from a single stray touch.
- Do not allow modchip state changes without protection.
- Do not allow video power-off without confirmation.
- Do not allow persistent settings until recovery is defined.
- Do not spam commands if the interposer is disconnected.
- Do not make the console dependent on the touchscreen.
- Do not bypass interposer safety logic.

Safety matters more than fancy UI.

---

## Minimum Safe UI

A minimum safe UI should include:

- Main status display
- Connection indicator
- Safe test output button
- Confirmation for reset or power actions
- Error display
- About/version page
- No dangerous service actions exposed by default

This is a good first customer-facing direction.

---

## Open Questions

Open touchscreen firmware questions:

- Which touchscreen module should be used first?
- Should the controller be RP2040, RP2350, ESP32, or another MCU?
- Should the touchscreen firmware be written in C, C++, Arduino, MicroPython, CircuitPython, or another environment?
- Should the touchscreen act as the UART hub?
- Should BlueRetro and SD2PSX connect directly to the touchscreen controller?
- Should the screen stay powered in standby?
- Should the display sleep when the console is off?
- Should service pages require a PIN?
- Should settings be stored on the touchscreen or interposer?
- Should the touchscreen support themes in the first version?
- Should the touchscreen display a build ID?
- Should the UI be round-display-first or generic-display-first?

These questions should be answered during prototype testing.

---

## Risks

Possible touchscreen risks:

- UI becomes too complicated
- Touch input triggers accidental actions
- Touchscreen sends unsafe commands
- Interposer communication fails
- Touchscreen assumes command success
- Display draws too much power
- Display adds heat
- Display mounting is difficult
- Touch input is inaccurate after mounting
- Firmware profile does not match installed hardware
- User sees unsupported pages
- Touchscreen failure affects console operation
- Backfeeding through UART or power lines

The base console should remain usable even if the touchscreen is disconnected.

---

## Documentation Requirements

Every touchscreen firmware revision should document:

- Firmware version
- Target display module
- Target controller MCU
- Hardware revision
- Pinout
- UART settings
- Supported pages
- Supported commands
- Supported profiles
- Known issues
- Test status
- Date tested
- Console model tested if applicable

Good documentation will be important if this becomes an FBD product.

---

## Future Expansion

Future touchscreen firmware may include:

- Build-specific themes
- Build ID display
- FBD splash screen
- Profile-based page hiding
- BlueRetro status page
- SD2PSX status page
- Bluetooth audio page
- RGB lighting page
- Video power page
- Router status page
- Laser feedback page
- Service diagnostics
- Error logs
- Firmware update helper
- Touchscreen sleep scheduling

These should be added only after the basic UI and interposer communication are reliable.

---

## Known Issues

Current known issues:

- Touchscreen hardware is not finalized.
- Display driver is not selected.
- Touch driver is not selected.
- Firmware environment is not selected.
- UI layout is not finalized.
- UART protocol is not finalized.
- Build profiles are not finalized.
- Hardware mounting is not finalized.
- Firmware has not been bench tested.
- Firmware has not been console tested.

This section should be updated as prototypes are built.

---

## Summary

The Touchscreen Controller firmware is the optional premium interface for Button Butler.

Its first job is simple:

- Start the display
- Read touch input
- Communicate with the Button Interposer Board
- Show connection status
- Request safe actions
- Display confirmed states
- Show errors clearly

The touchscreen should make advanced PS2 mods easier to use while leaving critical power/reset behavior under the control of the Button Interposer Board.

The first touchscreen firmware should stay simple.

Advanced pages for BlueRetro, SD2PSX, Bluetooth audio, ChipSlayer, modchip control, RGB lighting, video power, service diagnostics, and future laser feedback can be added after the basic communication and UI are proven.
