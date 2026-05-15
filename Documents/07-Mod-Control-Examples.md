# 07 - Mod Control Examples

## Overview

The **FBD PS2 Button Butler** is intended to act as a central control layer for internal PlayStation 2 mods.

This document lists example ways Button Butler could control, enable, disable, reset, trigger, or monitor different mods inside a custom PS2 build.

These are concept examples.

Not every feature will be supported in the first hardware revision, and not every build will need every control option.

---

## Purpose Of Mod Control

Modern PS2 builds often include multiple internal upgrades.

Examples include:

- Modchips
- ChipSlayer
- BlueRetro
- Bluetooth audio
- SD2PSX
- RGB lighting
- RetroGEM
- ElectronAnalog
- Internal routers
- PowerOR-style power boards
- LazyrSavre-style laser feedback boards

Each mod may need a button, switch, enable line, reset line, pairing trigger, power enable, or status indicator.

Button Butler is meant to reduce the need for separate external switches and make these features easier to control from one system.

---

## Control Methods

Button Butler may control mods in several ways.

Possible control methods:

| Control Method | Description |
| --- | --- |
| Digital output | Turns a signal on or off |
| Open-drain output | Pulls a signal low without actively driving it high |
| Enable line | Allows a device or circuit to turn on |
| Disable line | Forces a device or circuit off |
| Reset pulse | Momentarily resets a device |
| Pairing trigger | Starts Bluetooth pairing or setup mode |
| UART command | Sends a serial command to another controller |
| Power switch | Turns accessory power on or off |
| Touchscreen request | User requests an action through the touchscreen |
| Button shortcut | Physical power/reset button triggers a hidden action |
| Lid-switch modifier | Lid switch changes what the button does |

The final method depends on the mod being controlled.

---

## General Rules For Mod Control

Every controlled mod should follow basic safety rules.

Recommended rules:

- Define the default state.
- Define the active state.
- Avoid floating control lines.
- Avoid backfeeding unpowered boards.
- Avoid direct signal contention.
- Use open-drain style outputs when safer.
- Use level shifting when voltage levels differ.
- Test each control line before connecting it to the console.
- Make risky actions intentional.
- Document every wire and signal.

Button Butler should make the build cleaner, not more fragile.

---

## Mod Control Priority

Some controls are more important than others.

Possible priority order:

1. Console power/reset safety
2. Modchip or ChipSlayer state
3. Accessory power safety
4. BlueRetro or controller behavior
5. Bluetooth audio behavior
6. SD2PSX status or control
7. Video board power
8. RGB lighting
9. Chime or cosmetic feedback

This order may change by build.

The important point is that decorative features should never interfere with core console operation.

---

## Example 1 - ChipSlayer Control

ChipSlayer is one of the most useful targets for Button Butler.

Instead of using a separate external switch, Button Butler could control ChipSlayer through a dedicated output.

---

### Possible ChipSlayer Functions

Possible functions:

- Enable ChipSlayer
- Disable ChipSlayer
- Toggle ChipSlayer state
- Show ChipSlayer state on the touchscreen
- Show ChipSlayer state with an LED
- Change ChipSlayer state using a hidden button action
- Change ChipSlayer state based on lid switch state

---

### Possible ChipSlayer Inputs And Outputs

| Signal | Direction | Purpose |
| --- | --- | --- |
| ChipSlayer Enable | Output from Button Butler | Enables or disables ChipSlayer behavior |
| ChipSlayer State Feedback | Input to Button Butler | Reports current ChipSlayer state if available |
| ChipSlayer LED | Output from Button Butler | Shows state visually |
| Touchscreen Command | Input to Button Butler | Requests enable, disable, or toggle |

---

### Example ChipSlayer Button Behavior

| Condition | Button Action | Result |
| --- | --- | --- |
| Lid closed | Short press | Normal console behavior |
| Lid closed | Long press | Normal long-press behavior or reset |
| Lid open | Short press | Toggle ChipSlayer |
| Lid open | Long press | Force ChipSlayer enabled |
| Touchscreen | Tap toggle | Toggle ChipSlayer state |

This allows ChipSlayer control without adding another hole or switch to the shell.

---

### Example ChipSlayer Touchscreen Page

Possible touchscreen options:

- ChipSlayer ON
- ChipSlayer OFF
- Toggle ChipSlayer
- Show current state
- Show warning if state is unknown
- Return to main menu

The touchscreen should show the state clearly.

---

## Example 2 - Modchip Enable / Disable Control

Button Butler could control a modchip enable or disable circuit.

This is useful for builds where the modchip should be disabled for certain boot modes, testing, or compatibility reasons.

---

### Possible Modchip Functions

Possible functions:

- Enable modchip
- Disable modchip
- Disable modchip for one boot
- Enable modchip for one boot
- Toggle modchip state
- Show modchip state on the touchscreen
- Use hidden button behavior to change state

---

### Modchip Control Notes

Modchip control must be handled carefully.

Possible concerns:

- Incorrect state may affect boot behavior.
- A floating control line could cause random behavior.
- Modchip state may need to be set before console boot.
- Some modchips may not like being toggled while the console is running.
- The console should not be forced into an unknown boot condition.

Modchip state should have a safe default.

---

### Possible Modchip Inputs And Outputs

| Signal | Direction | Purpose |
| --- | --- | --- |
| Modchip Enable | Output from Button Butler | Enables the modchip control circuit |
| Modchip Disable | Output from Button Butler | Disables the modchip control circuit |
| Modchip State Feedback | Input to Button Butler | Reports state if the circuit supports it |
| Touchscreen Command | Input to Button Butler | Requests state change |
| Service Jumper | Input to Button Butler | Forces a safe mode or default state |

---

### Example Modchip Button Behavior

| Condition | Button Action | Result |
| --- | --- | --- |
| Console off | Hold during startup | Disable modchip for one boot |
| Lid closed | Short press | Normal console behavior |
| Lid open | Short press | Toggle modchip state |
| Lid open | Long press | Force modchip disabled |
| Touchscreen | Tap enable/disable | Change modchip state if safe |

---

### Example Modchip Touchscreen Page

Possible touchscreen options:

- Modchip Enabled
- Modchip Disabled
- Disable For Next Boot
- Restore Default State
- Show warning if state is unknown
- Require confirmation before changing state

Because this affects boot behavior, confirmations may be useful.

---

## Example 3 - Bluetooth Audio Control

Button Butler could control an internal Bluetooth audio board.

This is useful because Bluetooth audio modules often need a pairing button, enable line, reset line, or power control.

---

### Possible Bluetooth Audio Functions

Possible functions:

- Enable Bluetooth audio
- Disable Bluetooth audio
- Trigger pairing mode
- Reset Bluetooth audio board
- Show pairing state if available
- Show connected state if available
- Turn off Bluetooth audio when not needed

---

### Possible Bluetooth Audio Signals

| Signal | Direction | Purpose |
| --- | --- | --- |
| BT Audio Enable | Output from Button Butler | Turns audio board on or enables audio path |
| BT Pair Trigger | Output from Button Butler | Starts pairing mode |
| BT Reset | Output from Button Butler | Resets the audio board |
| BT Status | Input to Button Butler | Reports connected or pairing state if available |
| Touchscreen Command | Input to Button Butler | Requests enable, disable, pair, or reset |

---

### Example Bluetooth Audio Button Behavior

| Condition | Button Action | Result |
| --- | --- | --- |
| Lid closed | Short press | Normal console behavior |
| Lid open | Short press | Toggle Bluetooth audio |
| Lid open | Long press | Trigger pairing mode |
| Touchscreen | Tap Pair | Enter pairing mode |
| Touchscreen | Tap Off | Disable Bluetooth audio |

---

### Example Bluetooth Audio Touchscreen Page

Possible touchscreen options:

- Bluetooth Audio ON
- Bluetooth Audio OFF
- Pair
- Reset Audio Board
- Show connected state
- Show pairing state
- Return to main menu

---

### Bluetooth Audio Safety Notes

Bluetooth audio control should avoid:

- Loud pops when switching power
- Random pairing mode triggers
- Power cycling during use unless requested
- Audio ground noise
- Backfeeding through audio or control lines

The board should be tested with real audio output.

---

## Example 4 - BlueRetro Control

Button Butler could control an internal BlueRetro installation.

BlueRetro control may be simple at first and more advanced later.

---

### Possible BlueRetro Functions

Possible functions:

- Enable BlueRetro
- Disable BlueRetro
- Reset BlueRetro
- Disable individual controller channels if supported
- Show controller status if available
- Enter pairing mode if supported
- Show BlueRetro status on the touchscreen

---

### Possible BlueRetro Control Methods

| Method | Description |
| --- | --- |
| Enable line | Turns BlueRetro control circuit on or off |
| Reset line | Resets the ESP32 or BlueRetro controller |
| Channel disable lines | Disables specific controller channels if wired |
| UART status | Receives status if firmware supports it |
| Touchscreen page | User-facing control page |

Early versions may only support enable, disable, or reset.

Advanced communication can be added later.

---

### Possible BlueRetro Signals

| Signal | Direction | Purpose |
| --- | --- | --- |
| BlueRetro Enable | Output from Button Butler | Enables or disables BlueRetro |
| BlueRetro Reset | Output from Button Butler | Resets BlueRetro controller |
| Channel 1 Enable | Output from Button Butler | Enables or disables player 1 wireless input if supported |
| Channel 2 Enable | Output from Button Butler | Enables or disables player 2 wireless input if supported |
| BlueRetro Status | Input or UART | Reports connection state if available |
| Touchscreen Command | Input to Button Butler | Requests BlueRetro action |

---

### Example BlueRetro Button Behavior

| Condition | Button Action | Result |
| --- | --- | --- |
| Lid closed | Short press | Normal console behavior |
| Lid open | Short press | Toggle BlueRetro enable |
| Lid open | Long press | Reset BlueRetro |
| Touchscreen | Tap Channel 1 | Toggle controller channel 1 if supported |
| Touchscreen | Tap Disable | Disable BlueRetro |

---

### Example BlueRetro Touchscreen Page

Possible touchscreen options:

- BlueRetro ON
- BlueRetro OFF
- Reset BlueRetro
- Channel 1 Enable / Disable
- Channel 2 Enable / Disable
- Pairing status if available
- Controller status if available

---

### BlueRetro Safety Notes

BlueRetro behavior should not interfere with wired controller operation unless that is intentional.

Possible concerns:

- Controller port signal contention
- Power state during console standby
- ESP32 boot behavior
- Firmware startup time
- Random inputs if a damaged GPIO or unstable signal exists
- Backfeeding through controller lines

BlueRetro control should be tested with both wired and wireless controllers.

---

## Example 5 - SD2PSX Support

Button Butler may provide touchscreen support for SD2PSX or related memory card emulator features.

Early support may only be display/status based.

Control should be conservative because memory card behavior is important for game saves.

---

### Possible SD2PSX Functions

Possible functions:

- Show SD2PSX present/not present
- Show SD2PSX status if available
- Show current Game ID if available
- Show active card/profile if available
- Show warning state if available
- Provide limited control if supported
- Display status on the touchscreen

---

### SD2PSX Control Notes

SD2PSX should be handled carefully.

Possible concerns:

- Do not interrupt saves.
- Do not power-cycle during gameplay.
- Do not cause memory card corruption.
- Do not interfere with OPL or game save behavior.
- Do not assume control features are available unless confirmed.

For early Button Butler versions, SD2PSX should mostly be treated as a status/display feature.

---

### Possible SD2PSX Signals

| Signal | Direction | Purpose |
| --- | --- | --- |
| SD2PSX Status | Input or UART | Reports current state if available |
| SD2PSX Game ID | UART or data link | Reports active game if available |
| SD2PSX Control | UART or future protocol | Sends supported commands if available |
| Touchscreen Page | Display only or limited control | Shows SD2PSX information |

---

### Example SD2PSX Touchscreen Page

Possible touchscreen display items:

- SD2PSX detected
- SD card detected
- Active game ID
- Active memory card profile
- Save activity warning
- Firmware version if available
- Status or error message

---

### SD2PSX Safety Notes

Avoid aggressive control until behavior is fully understood.

Recommended early approach:

- Display status only
- Avoid power switching
- Avoid reset commands
- Avoid profile switching during gameplay
- Avoid anything that could affect active saves

---

## Example 6 - RGB Lighting Control

Button Butler can control decorative or status lighting.

Lighting is a good use for the I/O Expansion Board because it is useful but not safety-critical.

---

### Possible RGB Functions

Possible functions:

- Turn lighting on
- Turn lighting off
- Toggle lighting mode
- Set brightness
- Show console state
- Show mod state
- Show error state
- Disable lighting during gameplay
- Use lighting as status feedback

---

### Possible RGB Control Methods

| Method | Description |
| --- | --- |
| Simple digital output | Turns lighting on or off |
| PWM output | Controls brightness |
| RGB controller enable | Enables a separate lighting controller |
| Addressable LED output | Sends LED data if supported |
| Touchscreen page | User selects modes or presets |

Early versions should start with simple on/off control.

---

### Example RGB Button Behavior

| Condition | Button Action | Result |
| --- | --- | --- |
| Lid open | Double press | Toggle RGB lighting |
| Lid open | Long press | Cycle lighting mode |
| Touchscreen | Tap RGB Off | Turn lighting off |
| Touchscreen | Tap Preset | Select lighting mode |

---

### Example RGB Touchscreen Page

Possible touchscreen options:

- Lighting ON
- Lighting OFF
- Brightness
- Mode
- Status Mode
- Gameplay Mode
- Service Mode

---

### RGB Safety Notes

RGB lighting can draw more current than expected.

Important notes:

- Use current limiting.
- Measure current draw.
- Avoid overheating.
- Avoid noisy wiring near audio or video.
- Avoid pulling LED current through small logic traces.
- Use a proper driver or MOSFET for higher loads.

---

## Example 7 - RetroGEM Power Management

Button Butler may control power or enable behavior for RetroGEM in advanced builds.

This is intended to reduce heat or coordinate accessory power.

---

### Possible RetroGEM Functions

Possible functions:

- Enable RetroGEM power
- Disable RetroGEM power
- Show RetroGEM power state
- Auto-disable when console is idle
- Manual touchscreen override
- Service-mode power test

---

### RetroGEM Control Notes

Video hardware should be handled carefully.

Possible concerns:

- HDMI handshake behavior
- Video loss if power is disabled
- Startup timing
- Heat
- Backfeeding through video or control lines
- User confusion if display goes blank

Any RetroGEM power management should be tested thoroughly before being used in a customer build.

---

### Possible RetroGEM Signals

| Signal | Direction | Purpose |
| --- | --- | --- |
| RetroGEM Enable | Output from Button Butler | Enables video board power or regulator |
| RetroGEM Power Good | Input to Button Butler | Confirms power is stable if available |
| RetroGEM State | Input or UART if available | Reports status if available |
| Touchscreen Command | Input to Button Butler | Requests power on/off or override |

---

### Example RetroGEM Touchscreen Page

Possible touchscreen options:

- RetroGEM Power ON
- RetroGEM Power OFF
- Auto Power Saving ON/OFF
- Manual Override
- Show power-good state
- Show warning if video power is disabled

---

## Example 8 - ElectronAnalog Power Management

Button Butler may also control ElectronAnalog power in advanced builds.

ElectronAnalog is commonly used as an internal HDMI output solution in FBD-style PS2 builds.

---

### Possible ElectronAnalog Functions

Possible functions:

- Enable ElectronAnalog power
- Disable ElectronAnalog power
- Show power state
- Auto-disable when console is idle
- Manual touchscreen override
- Reduce heat in compact builds

---

### ElectronAnalog Control Notes

ElectronAnalog power control should be tested with real console video behavior.

Possible concerns:

- Video dropouts
- HDMI display behavior
- Startup timing
- Heat reduction benefit
- Backfeeding through video or audio paths
- Power rail noise

---

### Possible ElectronAnalog Signals

| Signal | Direction | Purpose |
| --- | --- | --- |
| ElectronAnalog Enable | Output from Button Butler | Enables video board power |
| ElectronAnalog Power Good | Input to Button Butler | Confirms power if available |
| Touchscreen Command | Input to Button Butler | Requests power state |
| Auto Power Mode | Firmware setting | Controls idle behavior |

---

### Example ElectronAnalog Touchscreen Page

Possible touchscreen options:

- HDMI Board ON
- HDMI Board OFF
- Auto Power Saving
- Manual Override
- Power Status
- Heat Reduction Mode

---

## Example 9 - Internal Router Control

Some PS2 builds may include an internal router or network device.

Button Butler could help manage its power or startup timing.

---

### Possible Internal Router Functions

Possible functions:

- Power router on
- Power router off
- Restart router
- Show router ready state
- Wait for router before console startup
- Show network mode on touchscreen
- Provide service reset

---

### Router Control Notes

Internal routers may take time to boot.

This matters if the PS2 depends on the router for SMB, UDPBD, UDPFS, or other network loading features.

Possible concerns:

- Router startup delay
- USB storage mount delay
- Wi-Fi connect delay
- Heat
- Current draw
- Power sequencing
- User confusion if the console starts before network storage is ready

---

### Possible Router Signals

| Signal | Direction | Purpose |
| --- | --- | --- |
| Router Power Enable | Output from Button Butler | Turns router power on |
| Router Reset | Output from Button Butler | Restarts router |
| Router Ready | Input to Button Butler | Reports ready state if available |
| Router UART | UART | Service/debug access if available |
| Touchscreen Command | Input to Button Butler | Requests router control |

---

### Example Router Touchscreen Page

Possible touchscreen options:

- Router ON
- Router OFF
- Restart Router
- Show ready state
- Show IP or mode if available
- Show storage mounted if available
- Service access

---

## Example 10 - LazyrSavre / Laser Feedback

Button Butler may eventually display information from a future LazyrSavre-style laser feedback or protection system.

This would be an advanced diagnostic feature.

---

### Possible Laser Feedback Functions

Possible functions:

- Show laser activity
- Show servo activity
- Show protection status
- Show warning state
- Show fault history
- Show health indicator
- Display service recommendation
- Show diagnostic values on touchscreen

---

### Laser Feedback Notes

This should be treated as a future advanced feature.

Possible concerns:

- Avoid interfering with sensitive optical drive signals.
- Keep diagnostic inputs high impedance.
- Do not add load to servo or laser circuits.
- Keep service information separate from normal user controls.
- Avoid making claims before real testing.

---

### Possible Laser Feedback Signals

| Signal | Direction | Purpose |
| --- | --- | --- |
| Laser Activity | Input to Button Butler or diagnostic board | Reports laser or servo behavior |
| Protection Trigger | Input | Reports that protection activated |
| Fault State | Input or UART | Reports diagnostic fault |
| Diagnostic UART | UART | Sends advanced feedback data |
| Touchscreen Page | Display | Shows status or warnings |

---

### Example Laser Feedback Touchscreen Page

Possible display items:

- Laser status
- Servo activity
- Protection active/inactive
- Warning indicator
- Last fault
- Service note
- Diagnostic mode

---

## Example 11 - Chime Or Buzzer Control

Button Butler may control a chime or buzzer circuit.

This can provide feedback for button actions and hidden functions.

---

### Possible Chime Functions

Possible functions:

- Startup chime
- Shutdown chime
- Button accepted tone
- Button rejected tone
- Bluetooth pairing tone
- Service mode tone
- Error tone
- Confirmation tone

---

### Possible Chime Signals

| Signal | Direction | Purpose |
| --- | --- | --- |
| Buzzer Output | Output from Button Butler | Drives active or passive buzzer |
| Audio Trigger | Output | Triggers a sound module if used |
| Touchscreen Setting | Command | Enables or disables sounds |

---

### Example Chime Feedback

| Action | Possible Feedback |
| --- | --- |
| ChipSlayer enabled | One short chime |
| ChipSlayer disabled | Two short chimes |
| Bluetooth pairing started | Rising tone |
| Service mode entered | Longer confirmation tone |
| Command rejected | Short error tone |
| Power on | Startup chime |

Chime feedback should be optional.

---

## Example 12 - Status LED Control

Button Butler may control one or more status LEDs.

Status LEDs are useful during development and may also be useful in final builds.

---

### Possible Status LED Functions

Possible functions:

- Show power state
- Show service mode
- Show ChipSlayer state
- Show modchip state
- Show Bluetooth audio state
- Show error state
- Show communication activity
- Show touchscreen connection

---

### Example LED Patterns

| Pattern | Possible Meaning |
| --- | --- |
| Solid on | Enabled or active |
| Off | Disabled or inactive |
| Slow blink | Pairing or waiting |
| Fast blink | Error or service mode |
| Single flash | Command accepted |
| Double flash | Command rejected or alternate state |
| Breathing effect | Standby or idle |

LED patterns should be documented if used in customer builds.

---

## Example 13 - Accessory Power Control

Button Butler can control accessory power rails through external power switching circuits.

---

### Possible Accessory Power Functions

Possible functions:

- Enable accessory rail
- Disable accessory rail
- Restart accessory rail
- Auto-disable after idle
- Show power-good status
- Manual override from touchscreen
- Service test output

---

### Possible Accessory Power Targets

Possible targets:

- Video board
- Bluetooth audio
- RGB lighting
- Internal router
- Touchscreen backlight
- Chime circuit
- Support boards
- Future diagnostic boards

---

### Accessory Power Safety Notes

Accessory power control must avoid:

- Overloading the PS2 supply
- Backfeeding through signal lines
- Power cycling memory-related devices during writes
- Creating video dropouts unexpectedly
- Switching high current without proper hardware
- Heat buildup in regulators or MOSFETs

Use proper load switches, MOSFETs, regulators, and protection where needed.

---

## Example 14 - Service Mode Control

Button Butler could include a service mode for testing and installer functions.

---

### Possible Service Mode Functions

Possible functions:

- Test all outputs
- Read all inputs
- Show button state
- Show lid switch state
- Show firmware version
- Toggle ChipSlayer
- Toggle modchip state
- Test Bluetooth pairing trigger
- Test BlueRetro reset
- Test RGB output
- Test video power output
- Show UART logs
- Reset settings

---

### Service Mode Access Methods

Possible access methods:

- Lid open plus long button hold
- Touchscreen hidden gesture
- Touchscreen password or PIN
- UART command
- Internal service jumper
- Special startup hold

Service mode should not be easy to enter accidentally.

---

## Example 15 - Build Profile Control

Different builds may need different mod control behavior.

Button Butler may eventually use build profiles.

---

### Possible Build Profiles

Possible profiles:

- Basic build
- Modchip build
- ChipSlayer build
- BlueRetro build
- Bluetooth audio build
- SD2PSX build
- Internal router build
- Premium Ultra Slim build
- Service/testing build

---

### Example Profile Behavior

| Profile | Main Control Focus |
| --- | --- |
| Basic | Normal button behavior and simple output |
| Modchip | Modchip enable/disable behavior |
| ChipSlayer | ChipSlayer control and status |
| BlueRetro | BlueRetro enable/reset/channel control |
| Bluetooth Audio | Audio enable and pairing |
| SD2PSX | Status display and conservative control |
| Internal Router | Router power and startup coordination |
| Premium Ultra Slim | Full touchscreen and mod management |
| Service/Test | Input/output diagnostics |

Profiles make the system easier to reuse across different builds.

---

## Example Combined Premium Build

A premium FBD Ultra Slim or custom shell build might use Button Butler to control many features together.

---

### Possible Installed Features

Possible installed features:

- Button Interposer Board
- Touchscreen Hub
- I/O Expansion Board
- BlueRetro
- Bluetooth audio
- SD2PSX
- ModBo 5.0
- ChipSlayer
- ElectronAnalog or RetroGEM
- RGB lighting
- Chime circuit
- Internal router
- Future laser feedback board

---

### Possible Premium Build Controls

| Feature | Possible Button Butler Control |
| --- | --- |
| Console power | Power on/off request |
| Console reset | Reset request |
| Modchip | Enable/disable |
| ChipSlayer | Enable/disable/toggle |
| BlueRetro | Enable/disable/reset/channel control |
| Bluetooth audio | Enable/disable/pair/reset |
| SD2PSX | Display status |
| Video board | Power enable/disable |
| RGB lighting | On/off/mode |
| Router | Power/restart/status |
| Laser feedback | Display diagnostic status |
| Chime | Feedback for actions |

---

## Example Hidden Button Shortcuts

Hidden button shortcuts can reduce the need for external controls.

Possible shortcuts:

| Condition | Button Action | Possible Result |
| --- | --- | --- |
| Lid closed | Short press | Normal PS2 behavior |
| Lid closed | Long press | Normal long-press behavior |
| Lid open | Short press | Toggle ChipSlayer |
| Lid open | Long press | Toggle modchip state |
| Lid open | Double press | Toggle RGB lighting |
| Lid open | Hold 5 seconds | Enter service mode |
| Startup hold | Hold button during boot | Safe mode or modchip off |
| Touchscreen command | Virtual button press | Requested mapped action |

These shortcuts should be optional and documented per build.

---

## Example Touchscreen Mod Menu

A premium touchscreen menu could include:

| Page | Controls |
| --- | --- |
| Main | Power, reset, status icons |
| Mods | Modchip, ChipSlayer |
| Controllers | BlueRetro enable/reset/channels |
| Audio | Bluetooth audio enable/pair/reset |
| Storage | SD2PSX status |
| Video | RetroGEM/ElectronAnalog power |
| Lighting | RGB on/off/modes |
| Service | Inputs, outputs, firmware, testing |
| About | Build ID, firmware, hardware revision |

Unused pages should be hidden for simpler builds.

---

## Recommended Development Order

Recommended development order for mod control:

1. Add one safe digital output
2. Test output with LED or dummy load
3. Use output to control a simple external circuit
4. Add ChipSlayer control
5. Add Bluetooth audio pairing trigger
6. Add RGB enable output
7. Add modchip control only after safe defaults are proven
8. Add video board power control after power testing
9. Add touchscreen control of outputs
10. Add BlueRetro or SD2PSX status later
11. Add advanced service pages last

Start with simple outputs before controlling important console behavior.

---

## Minimum Viable Mod Control

The minimum useful mod control setup could include:

- One physical button shortcut
- One lid switch modifier
- One output for ChipSlayer or mod control
- One debug UART
- One visible status indicator
- Known safe default states

This is enough to prove the core idea before adding more features.

---

## Output Default State Table

Each controlled mod needs a documented default state.

Example default table:

| Controlled Feature | Suggested Default | Reason |
| --- | --- | --- |
| ChipSlayer | Build-profile dependent | Depends on intended modchip behavior |
| Modchip control | Safe known state | Avoid boot confusion |
| Bluetooth audio pairing | Inactive | Avoid accidental pairing |
| Bluetooth audio power | Off or profile dependent | Reduce power and heat |
| BlueRetro enable | Profile dependent | Some builds may need always-on behavior |
| RGB lighting | Off | Avoid extra current draw |
| Video board power | On when console is on | Avoid user confusion |
| SD2PSX control | Monitor only | Avoid save/data risk |
| Router power | Profile dependent | May need startup before console |
| Chime output | Off | Avoid unwanted sound |

Defaults should be changed only after testing.

---

## Safety Checklist

Before connecting Button Butler to any mod, check:

- What voltage level does the control signal use?
- Is the control line active-high or active-low?
- Does the line already have a pull-up or pull-down?
- Can the line be safely pulled low?
- Can the line be safely driven high?
- Does the target board share ground?
- Can the target board be unpowered while Button Butler is powered?
- Could a signal line backfeed the target board?
- What happens if the Button Butler output floats?
- What happens if firmware crashes?
- What is the safest default state?

Do not connect unknown signals directly.

---

## Documentation Requirements

Every mod control example should eventually be documented with:

- Mod name
- Board revision
- Signal name
- Wire color if used
- Button Butler pin
- Voltage level
- Active state
- Default state
- Pull-up or pull-down notes
- Safety notes
- Tested console model
- Tested firmware version
- Photos or diagrams

Good documentation makes repeat builds possible.

---

## Test Data To Save

Useful test data should be saved in the repo.

Possible test data:

- Button behavior logs
- UART logs
- Output voltage measurements
- Current measurements
- Startup timing
- Shutdown timing
- BlueRetro behavior notes
- Bluetooth pairing test notes
- ChipSlayer state tests
- Modchip boot behavior tests
- Video power testing
- Heat testing
- Customer-safe behavior notes

Suggested folder:

`Test-Data/`

---

## Open Questions

Open questions for mod control:

- Which mod should be controlled first?
- Should ChipSlayer be the first real output target?
- What is the safest default state for ChipSlayer?
- How should modchip state be stored?
- Should modchip changes require a reboot?
- Should Bluetooth audio pairing be a pulse or held signal?
- Should BlueRetro stay powered in standby?
- Should SD2PSX be display-only at first?
- Should video board power control be manual only at first?
- How should the touchscreen show unknown states?
- Should service mode require the lid to be open?
- Should profiles be selected by firmware build, jumper, or touchscreen setting?

These questions should be answered through prototype testing.

---

## Risks

Possible risks:

- Accidentally disabling the wrong mod
- Modchip state causing boot confusion
- Signal contention
- Backfeeding
- Floating outputs
- Unstable BlueRetro behavior
- Bluetooth audio reconnect issues
- RGB current draw too high
- Video board power cycling issues
- SD2PSX data risk
- Touchscreen command mistakes
- Customer confusion from too many options

The safest path is to add one control feature at a time.

---

## Summary

Button Butler can become a central control system for many internal PS2 mods.

The most practical early examples are:

- ChipSlayer control
- Bluetooth audio pairing
- RGB lighting enable
- Simple accessory power control
- Basic status LED or chime feedback

More advanced examples include:

- Modchip enable/disable
- BlueRetro control
- SD2PSX status display
- RetroGEM or ElectronAnalog power management
- Internal router startup coordination
- LazyrSavre-style laser feedback display

The project should start with safe, simple outputs and grow into advanced touchscreen-based mod control after the base interposer hardware is proven.
