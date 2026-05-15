# 04 - Touchscreen Control

## Overview

The **FBD PS2 Button Butler** touchscreen layer is the advanced control interface for premium PlayStation 2 builds.

The base Button Butler system starts with a power/reset button interposer. The touchscreen expands that idea by giving the user a clean visual interface for controlling internal mods, viewing status, and accessing service features.

The touchscreen is not required for every Button Butler install.

It is intended for higher-end builds where the console has enough internal features to justify a central control panel.

---

## Purpose Of The Touchscreen

The purpose of the touchscreen is to reduce the need for extra switches, buttons, LEDs, and shell modifications.

Instead of adding several physical controls to the PS2 shell, the touchscreen can provide one organized user interface.

Possible touchscreen uses:

- Console power control
- Console reset control
- Modchip enable/disable
- ChipSlayer enable/disable
- BlueRetro enable/disable
- BlueRetro channel control
- Bluetooth audio enable/disable
- Bluetooth audio pairing
- RGB lighting control
- SD2PSX information display
- Video board power control
- Service and diagnostic pages
- Future laser feedback display

The touchscreen should make the console easier to use, not more complicated.

---

## Design Philosophy

The touchscreen should feel like a premium control panel, not a confusing engineering menu.

Design goals:

1. Simple main screen
2. Clear buttons
3. Clear status indicators
4. Fast access to common features
5. Hidden or protected advanced features
6. Minimal clutter
7. Build-specific configuration
8. Reliable communication with the interposer board
9. Easy service access for the installer

The touchscreen should improve the user experience while keeping risky console-control logic handled by the Button Butler interposer.

---

## Relationship To The Button Interposer

The touchscreen should not directly control sensitive PS2 power/reset signals by itself.

The recommended architecture is:

- The **Button Interposer Board** handles the actual PS2 power/reset behavior.
- The **Touchscreen Controller** sends requests to the interposer.
- The **Interposer** decides whether the request is safe and then performs the action.
- The **Interposer** reports status back to the touchscreen.

This keeps the most important console-control logic in one place.

---

## Why The Interposer Should Stay In Charge

The interposer should stay in charge because it is directly connected to the console button circuit.

This allows the interposer to:

- Maintain safe power/reset behavior
- Block unsafe touchscreen commands
- Fail to a safer default state
- Handle button debounce
- Track lid switch state
- Track console state where possible
- Control output defaults
- Prevent conflicting commands

The touchscreen is the user interface. The interposer is the controller that actually performs the critical actions.

---

## Touchscreen Hardware Ideas

The touchscreen hardware may vary depending on the build.

Possible display types:

- Small round touchscreen
- Small round display with separate input method
- Waveshare-style round touchscreen module
- RP2040 or RP2350 based display module
- ESP32-based display controller
- Custom display board in the future

The first goal is to use hardware that is easy to test and mount cleanly.

---

## Preferred Touchscreen Traits

Useful touchscreen traits:

- Small enough to fit cleanly on or inside a PS2 shell
- Bright enough to read indoors
- Low enough power draw for internal use
- Simple communication interface
- Available documentation
- Reliable startup behavior
- Easy firmware development
- Reasonable mounting options
- Good-looking display shape for custom builds

A round display is especially attractive because it can look intentional and decorative instead of looking like a random screen cut into the shell.

---

## Touchscreen Mounting Goals

The touchscreen should be installed in a way that looks clean and deliberate.

Mounting goals:

- Minimal shell cutting
- No messy external wiring
- Secure mounting
- Serviceable if needed
- Good viewing angle
- Does not interfere with internal components
- Does not block cooling
- Looks like part of the build design

For premium FBD builds, the touchscreen should look like a feature, not an afterthought.

---

## Main Screen Concept

The main screen should show the most important information first.

Possible main screen items:

- Console power state
- Modchip state
- ChipSlayer state
- BlueRetro state
- Bluetooth audio state
- SD2PSX state
- Video board power state
- RGB lighting state
- Service warning icon if needed

The main screen should not be overloaded.

A simple status dashboard is better than a cluttered menu.

---

## Main Screen Example Layout

Possible main screen layout:

| Area | Possible Content |
| --- | --- |
| Top | Build name or FBD logo |
| Center | Console power state |
| Bottom left | Power button |
| Bottom right | Reset button |
| Status icons | BlueRetro, SD2PSX, audio, modchip, lighting |
| Hidden access | Long press or corner hold for service page |

This is only a concept and may change depending on the display resolution and shape.

---

## Touchscreen Page Ideas

The touchscreen may eventually include several pages.

Possible pages:

- Main page
- Power page
- Reset page
- Modchip page
- ChipSlayer page
- BlueRetro page
- Bluetooth audio page
- SD2PSX page
- RGB lighting page
- Video power page
- Service page
- Diagnostics page
- Settings page
- About page

Not every build needs every page.

The UI should be build-specific so unused features can be hidden.

---

## Main Page

The main page is the default user screen.

Possible functions:

- Show console state
- Show key mod states
- Provide power control
- Provide reset control
- Provide quick access to common pages
- Show warning icons
- Show build identity

The main page should be safe for normal users.

---

## Power Page

The power page could provide detailed power control.

Possible functions:

- Power console on
- Power console off
- Restart console
- Show accessory power state
- Enable or disable accessory power
- Show video board power state
- Confirm power-off actions

Power actions should be protected from accidental taps.

---

## Reset Page

The reset page could provide reset-related actions.

Possible functions:

- Soft reset request
- Hard reset request if supported
- Reset BlueRetro
- Reset Bluetooth audio board
- Reset accessory controller
- Reset Button Butler subsystem
- Confirm before performing risky reset actions

Reset actions should be clearly labeled.

---

## Modchip Page

The modchip page could control modchip-related behavior.

Possible functions:

- Show modchip enabled/disabled state
- Enable modchip
- Disable modchip
- Disable modchip for next boot
- Force modchip state until changed
- Show warning if state is unknown

This page should be used carefully because modchip state can affect boot behavior.

---

## ChipSlayer Page

The ChipSlayer page could control ChipSlayer behavior.

Possible functions:

- Show ChipSlayer state
- Enable ChipSlayer
- Disable ChipSlayer
- Toggle ChipSlayer
- Show state using color or icon
- Provide service notes if needed

This page could replace a physical ChipSlayer switch in premium builds.

---

## BlueRetro Page

The BlueRetro page could control or display internal BlueRetro status.

Possible functions:

- Enable BlueRetro
- Disable BlueRetro
- Show controller connection state if available
- Enable or disable controller channels
- Reset BlueRetro
- Enter pairing mode if supported
- Show firmware or mode information if available

BlueRetro control depends on what communication or control lines are available.

Early versions may only support simple enable/disable behavior.

---

## Bluetooth Audio Page

The Bluetooth audio page could control an internal Bluetooth audio board.

Possible functions:

- Enable Bluetooth audio
- Disable Bluetooth audio
- Enter pairing mode
- Reset the audio board
- Show pairing status if available
- Show connected state if available
- Mute audio if supported

This could eliminate the need for a dedicated Bluetooth pairing button on the shell.

---

## SD2PSX Page

The SD2PSX page could display or control memory card emulator features.

Possible functions:

- Show SD2PSX present/not present
- Show current status if available
- Show Game ID if available
- Show memory card mode if available
- Show active card/profile if available
- Provide simple control options if supported

This depends on the SD2PSX communication options available to the build.

Early versions may only provide status display or placeholder support.

---

## RGB Lighting Page

The RGB lighting page could control decorative or status lighting.

Possible functions:

- Turn RGB lighting on
- Turn RGB lighting off
- Select preset
- Select brightness
- Select status mode
- Link lighting to console state
- Link lighting to mod state
- Disable lighting during gameplay

The lighting page should stay simple unless advanced RGB control is actually needed.

---

## Video Power Page

The video power page could control internal video hardware.

Possible targets:

- RetroGEM
- ElectronAnalog
- Other internal video boards
- HDMI adapter power rail
- Accessory power enable line

Possible functions:

- Show video board power state
- Enable video board power
- Disable video board power
- Auto-disable video board when console is idle
- Manual override
- Heat reduction mode

This is useful for compact PS2 builds where internal heat matters.

---

## Service Page

The service page is intended for installer or advanced user access.

Possible service functions:

- Show input states
- Show output states
- Test each output
- Test button behavior
- Test lid switch state
- Test UART communication
- Show firmware version
- Enter diagnostic mode
- Reset settings
- Run hardware self-test

The service page should be protected from accidental access.

---

## Diagnostic Page

The diagnostic page may show live system information.

Possible diagnostic information:

- Button state
- Lid switch state
- Console state
- Output states
- UART status
- Touchscreen status
- Interposer status
- Accessory power states
- Error flags
- Last command received
- Last command accepted or rejected

This page is mainly for development and troubleshooting.

---

## Laser Feedback Page

A future laser feedback page could display information from LazyrSavre-style hardware or other diagnostic systems.

Possible future information:

- Laser activity
- Servo feedback
- Protection status
- Warning status
- Fault state
- Last protection event
- Health indicator
- Service recommendation

This is a future idea and should not block early Button Butler development.

---

## Settings Page

The settings page could contain build-specific configuration.

Possible settings:

- Button behavior profile
- Touchscreen brightness
- Sleep timeout
- Sound or chime enable
- RGB behavior
- Default modchip state
- Default ChipSlayer state
- Accessory power behavior
- Service lock setting
- Display orientation
- Theme selection

Settings should be stored safely and should not create unsafe default behavior.

---

## About Page

The about page could show basic build information.

Possible information:

- FBD PS2 Button Butler
- Firmware version
- Hardware revision
- Build ID
- Console model
- Installed feature list
- FBD branding
- Support or documentation note

This page can help identify what is installed in a specific console.

---

## User Interface Rules

The touchscreen UI should follow simple rules.

Recommended rules:

- Use plain labels
- Avoid too many menus
- Hide unused features
- Show clear on/off states
- Confirm risky actions
- Do not expose service tools to normal users by default
- Use consistent button placement
- Use simple icons where useful
- Keep the main screen clean
- Make error states obvious

A clean interface is more valuable than a flashy but confusing one.

---

## Confirmation Rules

Some actions should require confirmation.

Actions that may need confirmation:

- Power off console
- Reset console
- Disable modchip
- Enable service mode
- Reset settings
- Disable accessory power
- Run diagnostic actions
- Change default startup behavior

Simple actions like toggling lights may not need confirmation.

---

## Protected Actions

Some actions should be protected from accidental use.

Possible protected actions:

- Service mode access
- Modchip state changes
- Factory reset of Button Butler settings
- Firmware update mode
- Dangerous power control actions
- Advanced diagnostic functions

Protection methods:

- Hold button for several seconds
- Hidden touchscreen gesture
- Lid-open requirement
- Password or PIN
- Internal jumper
- UART unlock command

The protection method should match the risk level of the action.

---

## Touchscreen Sleep Behavior

The touchscreen should not have to stay fully active all the time.

Possible sleep behavior:

- Dim after inactivity
- Turn display off after inactivity
- Wake on touch
- Wake on physical button press
- Wake on console power state change
- Wake on error or warning event
- Stay awake while in service mode

Sleep behavior can reduce power draw, heat, and screen wear.

---

## Touchscreen Startup Behavior

When power is applied, the touchscreen should start in a predictable state.

Possible startup behavior:

- Show FBD logo
- Show Button Butler name
- Show firmware version
- Check interposer connection
- Load build profile
- Read current states
- Go to main page
- Show warning if communication fails

The touchscreen should not send risky commands automatically during startup unless the build specifically requires it.

---

## Communication With Interposer

The touchscreen should communicate with the Button Butler interposer using a simple and reliable protocol.

Possible communication method:

- UART

Possible communication goals:

- Send command requests
- Receive command acknowledgements
- Receive state updates
- Receive error flags
- Request output states
- Request firmware version
- Request service data

The protocol should be easy to debug with a serial adapter.

---

## Touchscreen Command Examples

Possible touchscreen commands:

| Command Idea | Meaning |
| --- | --- |
| Request power on | Ask interposer to power on console |
| Request power off | Ask interposer to power off console |
| Request reset | Ask interposer to reset console |
| Set output on | Turn a mapped output on |
| Set output off | Turn a mapped output off |
| Toggle output | Toggle a mapped output |
| Request state | Ask for current input/output states |
| Enter service | Request service mode |
| Exit service | Leave service mode |
| Set profile | Change behavior profile |

The exact command format should be defined later in the firmware protocol notes.

---

## Status Reporting

The interposer should report status back to the touchscreen.

Possible status reports:

| Status | Meaning |
| --- | --- |
| Console on | Console appears to be powered on |
| Console off | Console appears to be off or in standby |
| Lid open | Lid switch reports open |
| Lid closed | Lid switch reports closed |
| Button pressed | Physical button is active |
| Button released | Physical button is inactive |
| Output on | A mapped output is active |
| Output off | A mapped output is inactive |
| Command accepted | Requested command was accepted |
| Command rejected | Requested command was rejected |
| Error | A fault or invalid state was detected |

The touchscreen should not assume a command worked unless the interposer confirms it.

---

## Error Handling

The touchscreen should clearly show when something is wrong.

Possible error cases:

- Interposer not responding
- Unknown command response
- Command rejected
- Console state unknown
- Output state unknown
- Touchscreen profile mismatch
- Firmware version mismatch
- Unsafe action blocked
- Service lock active

The UI should explain errors in simple terms where possible.

---

## Example Error Messages

Possible user-facing error messages:

| Error | Possible Message |
| --- | --- |
| Interposer not responding | Button Butler controller not detected |
| Command rejected | Action blocked for safety |
| Console state unknown | Console state unknown |
| Service locked | Service mode is locked |
| Output fault | Output state could not be confirmed |
| Profile mismatch | Build profile does not match hardware |

Error messages should be short and useful.

---

## Touchscreen Profiles

Different builds may need different touchscreen profiles.

Possible profiles:

- Basic profile
- BlueRetro profile
- Bluetooth audio profile
- ChipSlayer profile
- SD2PSX profile
- Premium Ultra Slim profile
- Service/testing profile
- Demo profile

Profiles allow unused pages to be hidden and keep the UI clean.

---

## Basic Touchscreen Profile

The basic profile should show only simple controls.

Possible pages:

- Main page
- Power page
- About page

Possible features:

- Power status
- Power on/off request
- Reset request
- Firmware version
- Build ID

This profile is useful for early touchscreen testing.

---

## Premium Build Profile

The premium build profile could expose most of the advanced features.

Possible pages:

- Main page
- Power page
- Modchip page
- ChipSlayer page
- BlueRetro page
- Bluetooth audio page
- SD2PSX page
- RGB page
- Video power page
- Service page
- About page

This profile would be used for high-end FBD builds with multiple internal mods.

---

## Service/Test Profile

The service/test profile is intended for development and troubleshooting.

Possible pages:

- Input test page
- Output test page
- UART test page
- Button timing page
- Lid switch page
- Firmware page
- Error log page

This profile should not be the default customer-facing mode.

---

## Touchscreen Safety Rules

The touchscreen should follow safety rules.

Recommended safety rules:

- Do not assume the console state
- Do not repeat risky commands endlessly
- Do not send power/reset commands during boot unless intended
- Do not change modchip state without confirmation
- Do not expose service pages by default
- Do not allow random touch noise to trigger actions
- Do not leave outputs in unknown states
- Do not override interposer safety decisions

The touchscreen should request actions. The interposer should approve or reject them.

---

## Touch Debounce And Input Filtering

Touch input should be filtered so accidental touches do not cause problems.

Possible input filtering:

- Ignore touches during startup
- Require button release before accepting another press
- Add short delay after page changes
- Confirm dangerous actions
- Ignore touches near screen edge if needed
- Require long press for service entry
- Use larger buttons for important actions

A small display needs a simple UI with large touch targets.

---

## Visual Feedback

The touchscreen should give clear visual feedback.

Possible feedback:

- Button changes state after press
- Status icons update
- Confirmation message appears
- Error message appears
- Progress indicator during reset or power request
- Warning icon if communication fails
- Disabled buttons for unavailable functions

The user should know whether an action was accepted, rejected, or still pending.

---

## Audio Or Chime Feedback

Button Butler may also support chime or buzzer feedback.

Possible touchscreen-related chimes:

- Touch accepted
- Command accepted
- Command rejected
- Service mode entered
- Pairing mode started
- Warning or error
- Startup chime

Chimes should be optional.

Not every build needs sound feedback.

---

## Display Themes

The touchscreen may eventually support display themes.

Possible themes:

- FBD default theme
- BlueRetro themed page
- Blue Steel theme
- Build-specific theme
- Minimal black theme
- Service/testing theme

Themes should not delay early development.

The first UI should focus on function and reliability.

---

## Build-Specific Branding

The touchscreen could show build-specific branding.

Possible branding items:

- FBD logo
- Build name
- Build ID
- Console model
- Customer build profile
- Custom splash screen
- Theme matched to the console style

This could make premium builds feel more finished.

---

## Possible Startup Screen

A simple startup screen could show:

- FBD logo
- Button Butler name
- Firmware version
- Hardware revision
- Build ID
- Loading status

Example startup flow:

| Step | Action |
| --- | --- |
| 1 | Show FBD logo |
| 2 | Initialize display |
| 3 | Check interposer communication |
| 4 | Load profile |
| 5 | Read current states |
| 6 | Go to main screen |

---

## Possible Main Page Flow

Possible main page flow:

| User Action | Result |
| --- | --- |
| Tap Power | Opens power page or sends power request |
| Tap Reset | Opens reset confirmation |
| Tap Mods | Opens mod control page |
| Tap Audio | Opens Bluetooth audio page |
| Tap Controllers | Opens BlueRetro page |
| Tap Lighting | Opens RGB page |
| Hold corner | Opens protected service access |

This is only a concept.

---

## Development Testing Order

The touchscreen should be developed after the base interposer behavior is stable.

Recommended testing order:

1. Bring up display hardware
2. Confirm touch input works
3. Create a basic main page
4. Send a simple UART ping
5. Receive interposer response
6. Request input/output state
7. Display state on screen
8. Send a harmless output toggle command
9. Add power/reset request commands
10. Add confirmations for risky actions
11. Add build profiles
12. Add advanced pages later

Do not start with the full UI.

Start with one button, one command, and one response.

---

## Early Prototype UI

The first prototype UI should be very simple.

Recommended early UI:

- FBD logo or title
- Connection status
- Button state display
- Lid switch state display
- One test output button
- One reset request button
- UART log indicator

This proves the screen, touch input, and communication before adding complex menus.

---

## Open Questions

Open questions for touchscreen development:

- Which touchscreen module should be used first?
- Should the touchscreen controller be separate from the interposer MCU?
- Should the screen communicate by UART only?
- Should the display sleep when the console is off?
- Should the display be powered from standby power or accessory power?
- How should firmware updates be handled?
- Should profiles be selected from the screen or flashed per build?
- How should service pages be protected?
- How much status can BlueRetro and SD2PSX realistically provide?
- Should the screen be optional and removable?
- What is the cleanest mounting method for different PS2 shells?

These questions should be answered during prototype testing.

---

## Risks

Possible touchscreen risks:

- Too much complexity too early
- Touch input causing accidental actions
- Communication failure between screen and interposer
- Display using too much power
- Display adding too much heat
- Poor mounting fitment
- UI becoming confusing
- Feature creep
- Screen failure disabling normal console use
- Build profile mismatch

The base console should not become unusable just because the touchscreen fails.

---

## Minimum Viable Touchscreen

The minimum useful touchscreen version should only do a few things well.

Minimum viable features:

- Start reliably
- Show connection to the interposer
- Show console/button state if available
- Send one or two safe commands
- Receive command acknowledgement
- Show basic errors
- Sleep or dim after inactivity

Everything else can come later.

---

## Summary

The touchscreen layer is the premium user interface for Button Butler.

It should provide clean control over internal PS2 mods without requiring a pile of switches, buttons, and shell holes.

The touchscreen should request actions from the Button Butler interposer, while the interposer remains responsible for safe console control.

The first touchscreen prototype should stay simple.

Once communication and basic control are proven, additional pages can be added for BlueRetro, SD2PSX, Bluetooth audio, ChipSlayer, modchip control, RGB lighting, video power management, and future diagnostic features.
