# 03 - Button Behavior

## Overview

The **FBD PS2 Button Butler** is built around one main idea:

The original PlayStation 2 power/reset button does not have to be limited to only its factory behavior.

With Button Butler installed, the button can become a smart input that changes behavior depending on the console state, lid switch state, press duration, installed mods, and optional touchscreen commands.

This document defines possible button behavior concepts for the project.

---

## Purpose Of Button Behavior Control

The purpose of button behavior control is to make the PS2 power/reset button more useful without adding extra switches or drilling extra holes in the console shell.

Instead of adding separate buttons for every internal mod, Button Butler can watch the original button and decide what action should happen.

Possible uses:

- Normal console power/reset behavior
- Hidden mod control
- Service mode entry
- Bluetooth pairing
- ChipSlayer control
- Modchip enable/disable
- RGB lighting control
- Accessory power control
- Touchscreen-requested power/reset actions

The button should still feel natural to the user, but it can have extra functions for advanced builds.

---

## Design Goals

Button behavior should be:

1. Predictable
2. Reliable
3. Easy to explain
4. Easy to test
5. Safe for the console
6. Flexible for different builds
7. Simple enough for repeat installs

The button should never feel random or confusing.

For customer builds, normal power/reset behavior should remain easy to understand.

Advanced actions should be hidden, intentional, or protected from accidental activation.

---

## Factory PS2 Button Behavior

The original PS2 power/reset button behavior depends on the console model and state.

In general, the button may be used for:

- Powering the console on
- Resetting the console
- Powering the console off
- Entering standby
- Waking from standby

Button Butler should respect the original behavior unless it is intentionally overriding or modifying it.

The first development goal is not to replace everything. The first goal is to safely intercept, observe, and pass through the original behavior.

---

## Button Butler Behavior Modes

Button Butler may support multiple behavior modes.

Possible modes:

- Pass-through mode
- Intercept mode
- Block mode
- Alternate action mode
- Touchscreen command mode
- Service mode
- Fail-safe mode

Each mode should be clearly defined in firmware and documentation.

---

## Pass-Through Mode

Pass-through mode is the safest and most basic behavior.

In this mode, Button Butler allows the PS2 power/reset button to behave normally.

Possible uses:

- Default state
- Fail-safe state
- Testing state
- Customer-friendly normal operation
- Fallback if firmware has no special action assigned

Pass-through mode should be the starting point for development.

If nothing special is happening, the button should act like a normal PS2 button.

---

## Intercept Mode

Intercept mode means Button Butler sees the button press before the console reacts.

In this mode, the firmware can decide whether to:

- Pass the press through
- Delay the press
- Block the press
- Convert the press into another action
- Trigger an output
- Send a UART command
- Wake or notify the touchscreen controller

This is the core behavior that makes Button Butler useful.

---

## Block Mode

Block mode prevents the normal button action from reaching the console.

This is useful when the button press is being used for something else.

Example:

If the lid switch is open and the button is pressed, Button Butler could block the normal power/reset action and instead toggle ChipSlayer.

Possible block mode uses:

- Prevent accidental reset
- Use the button for hidden functions
- Enter service mode
- Trigger Bluetooth pairing
- Toggle internal mods
- Allow touchscreen-controlled power behavior

Block mode must be used carefully.

The firmware should avoid leaving the console in a state where the button cannot power on or reset the console unless that behavior is intentional and recoverable.

---

## Alternate Action Mode

Alternate action mode means the button press performs a different function from normal.

Possible alternate actions:

- Toggle ChipSlayer
- Enable or disable a modchip
- Trigger Bluetooth audio pairing
- Toggle RGB lighting
- Reset BlueRetro
- Disable BlueRetro
- Enable accessory power
- Disable accessory power
- Enter service mode
- Send a UART command to another board

Alternate actions are one of the main reasons Button Butler exists.

---

## Touchscreen Command Mode

In touchscreen command mode, the touchscreen can request actions from the Button Butler interposer.

Possible touchscreen-requested actions:

- Power on console
- Power off console
- Reset console
- Toggle ChipSlayer
- Toggle modchip state
- Enable or disable BlueRetro
- Trigger Bluetooth pairing
- Enable or disable RGB lighting
- Control accessory power
- Enter service mode

The touchscreen should not directly fight the console button circuit.

The interposer should remain responsible for the actual power/reset behavior.

This keeps the risky console-control logic in one place.

---

## Service Mode

Service mode is a special behavior mode for testing, diagnostics, or installer functions.

Possible service mode actions:

- Show button state over UART
- Show lid switch state over UART
- Toggle each output manually
- Test LEDs
- Test chime or buzzer output
- Test ChipSlayer control
- Test modchip control
- Test accessory power control
- Enter future laser feedback pages
- Run installer diagnostics

Service mode should be protected from accidental entry.

Possible entry methods:

- Hold button while lid is open
- Long press during startup
- Touchscreen password page
- UART command
- Internal service jumper

Service mode should not be required for normal users.

---

## Fail-Safe Mode

Fail-safe behavior is very important.

If Button Butler has a firmware problem, wiring issue, or configuration issue, the system should fail in the safest practical way.

Ideal fail-safe behavior:

- Console can still power on
- Console can still reset
- Button is not held active
- Reset line is not held in an unsafe state
- Outputs do not float into dangerous states
- Accessories do not backfeed the console
- Mod control lines have defined default states

Depending on the final circuit, true fail-safe behavior may require hardware design choices, not just firmware.

Examples:

- Pull-ups or pull-downs on control lines
- Open-drain outputs where appropriate
- Series resistors where needed
- High-impedance defaults
- Pass-through default state
- Reset supervisor behavior
- Firmware startup delay

---

## Button Input Types

Button Butler may need to understand several button input conditions.

Possible input types:

- Press
- Release
- Short press
- Long press
- Double press
- Hold during power-up
- Press while lid is open
- Press while lid is closed
- Press while console is on
- Press while console is off
- Touchscreen virtual press

Each input type can map to a different action depending on the installed build profile.

---

## Short Press

A short press is a quick normal button press.

Possible uses:

- Normal PS2 power/reset action
- Wake touchscreen
- Select a basic option
- Toggle a simple output
- Confirm an action

For most builds, short press should probably remain close to normal PS2 behavior.

This keeps the console intuitive for the user.

---

## Long Press

A long press is when the button is held for a set amount of time.

Possible uses:

- Power off
- Reset
- Enter service mode
- Toggle modchip state
- Toggle ChipSlayer
- Trigger Bluetooth pairing
- Disable RGB lighting
- Force pass-through mode

The long press time should be long enough to avoid accidental activation.

Possible starting values:

| Behavior | Possible Time |
| --- | --- |
| Short press | Less than 500 ms |
| Medium press | 500 ms to 1500 ms |
| Long press | 2 seconds or more |
| Service hold | 5 seconds or more |

These values are starting ideas and should be tested.

---

## Double Press

A double press is two quick button presses within a short time window.

Possible uses:

- Toggle RGB lighting
- Wake touchscreen
- Cycle display page
- Toggle Bluetooth audio
- Toggle an accessory output

Double press may be useful, but it can also be harder to explain to customers.

It should be optional.

---

## Hold During Startup

Holding the button during startup could trigger a special startup action.

Possible uses:

- Enter service mode
- Disable modchip for one boot
- Enable ChipSlayer for one boot
- Force safe mode
- Disable touchscreen commands
- Reset Button Butler settings

This behavior should be tested carefully so it does not interfere with normal console startup.

---

## Lid Switch Based Behavior

The lid switch is one of the most useful extra inputs for Button Butler.

The lid switch can act like a hidden modifier.

For example:

- Button press with lid closed equals normal behavior
- Button press with lid open equals alternate behavior

This makes it possible to add hidden functions without adding another physical switch.

---

## Lid Closed Behavior

When the lid is closed, the button should usually behave normally.

Possible lid-closed behavior:

- Short press passes through normally
- Long press performs normal reset or power action
- Touchscreen commands are allowed
- Normal customer operation is maintained

This should be the default user-friendly state.

---

## Lid Open Behavior

When the lid is open, Button Butler can unlock alternate behavior.

Possible lid-open behavior:

- Short press toggles ChipSlayer
- Long press toggles modchip state
- Double press enters service mode
- Hold button to trigger Bluetooth pairing
- Touchscreen service menu becomes available
- Normal console reset is blocked

This is a useful way to hide installer functions.

---

## Example Behavior Map

This is an early concept map for possible button behavior.

| Console State | Lid State | Button Action | Possible Result |
| --- | --- | --- | --- |
| Off / standby | Closed | Short press | Power on console |
| On | Closed | Short press | Normal reset or factory behavior |
| On | Closed | Long press | Power off or alternate reset behavior |
| On | Open | Short press | Toggle ChipSlayer |
| On | Open | Long press | Toggle modchip state |
| On | Open | Double press | Enter service mode |
| Off / standby | Open | Long press | Force safe mode or service mode |
| Any | Any | Touchscreen command | Perform requested mapped action |

This table is only a starting concept and may change after hardware testing.

---

## Button Behavior Profiles

Button Butler may eventually support build-specific behavior profiles.

Possible profiles:

- Basic profile
- Modchip profile
- BlueRetro profile
- Bluetooth audio profile
- ChipSlayer profile
- Touchscreen profile
- Premium Ultra Slim profile
- Service/testing profile

Profiles would allow different builds to use different mappings without rewriting the entire firmware.

---

## Basic Profile

The basic profile should keep the button behavior close to stock.

Possible basic profile:

| Action | Result |
| --- | --- |
| Short press | Normal button pass-through |
| Long press | Normal button pass-through or reset |
| Lid open short press | No special action |
| Lid open long press | No special action |
| UART command | Debug only |

This is useful for first prototypes and simple installs.

---

## ChipSlayer Profile

The ChipSlayer profile would focus on controlling ChipSlayer from the button.

Possible ChipSlayer profile:

| Action | Result |
| --- | --- |
| Short press, lid closed | Normal button behavior |
| Long press, lid closed | Normal long press behavior |
| Short press, lid open | Toggle ChipSlayer |
| Long press, lid open | Show or confirm ChipSlayer state |
| Touchscreen command | Enable or disable ChipSlayer |

This would allow ChipSlayer control without an external switch.

---

## Modchip Profile

The modchip profile would focus on controlling modchip state.

Possible modchip profile:

| Action | Result |
| --- | --- |
| Short press, lid closed | Normal button behavior |
| Long press, lid closed | Normal long press behavior |
| Short press, lid open | Toggle modchip enable |
| Long press, lid open | Force modchip off for next boot |
| Hold during startup | Boot with modchip disabled |

This could be useful for builds that need clean modchip control.

---

## Bluetooth Audio Profile

The Bluetooth audio profile would focus on audio board control.

Possible Bluetooth audio profile:

| Action | Result |
| --- | --- |
| Short press, lid closed | Normal button behavior |
| Long press, lid closed | Normal long press behavior |
| Short press, lid open | Toggle Bluetooth audio |
| Long press, lid open | Enter Bluetooth pairing mode |
| Touchscreen command | Pair, enable, or disable Bluetooth audio |

This removes the need for a separate Bluetooth pairing button.

---

## BlueRetro Profile

The BlueRetro profile would focus on controller-related actions.

Possible BlueRetro profile:

| Action | Result |
| --- | --- |
| Short press, lid closed | Normal button behavior |
| Long press, lid closed | Normal long press behavior |
| Short press, lid open | Toggle BlueRetro enable |
| Long press, lid open | Reset BlueRetro |
| Touchscreen command | Enable/disable BlueRetro or channels |

This could be expanded later if the BlueRetro integration supports more commands.

---

## Touchscreen Profile

The touchscreen profile would allow the screen to act as the main user interface.

Possible touchscreen profile:

| Action | Result |
| --- | --- |
| Short press | Wake display or normal button action |
| Long press | Power menu or reset menu |
| Lid open short press | Unlock service page |
| Lid open long press | Enter installer/service mode |
| Touchscreen button | Request mapped action from interposer |

The touchscreen should make advanced features easier to use, not harder.

---

## Priority Rules

If multiple conditions are true at the same time, the firmware needs priority rules.

Example priority order:

1. Safety lockout
2. Service mode request
3. Lid-open alternate action
4. Touchscreen command
5. Long press behavior
6. Double press behavior
7. Short press behavior
8. Normal pass-through

This order may change after testing.

The important point is that the firmware should not guess randomly when multiple actions could apply.

---

## Debounce

The button input should be debounced.

Mechanical buttons can bounce electrically when pressed or released.

Without debounce, the firmware may see one press as several presses.

Possible debounce methods:

- Firmware delay
- State machine debounce
- Hardware RC filtering
- Schmitt trigger input
- Combination of hardware and firmware

Firmware debounce is likely enough for early testing, but hardware options can be considered if needed.

---

## Suggested Timing Values

Early timing values can start simple.

| Parameter | Starting Value |
| --- | --- |
| Debounce time | 20 ms to 50 ms |
| Short press maximum | 500 ms |
| Long press threshold | 2000 ms |
| Service hold threshold | 5000 ms |
| Double press window | 300 ms to 500 ms |
| Startup hold window | First 2 seconds after power-up |

These values should be tuned after real testing.

---

## State Detection

Button Butler behavior depends on knowing the system state.

Possible states:

- Console off
- Console in standby
- Console on
- Lid open
- Lid closed
- Touchscreen awake
- Touchscreen asleep
- Service mode active
- Modchip enabled
- Modchip disabled
- ChipSlayer enabled
- ChipSlayer disabled
- Accessory power enabled
- Accessory power disabled

Not all states will be available in early hardware.

The first version should use only states that can be detected reliably.

---

## Output Actions

Button actions may control outputs.

Possible output actions:

- Pulse output
- Hold output high
- Hold output low
- Toggle output state
- Momentary open-drain pull-down
- Enable accessory power
- Disable accessory power
- Send UART message
- Wake touchscreen
- Trigger chime
- Flash status LED

Each output should have a defined default state.

---

## Output Safety

Outputs should be designed so they do not damage the console or connected mods.

Output safety rules:

- Avoid direct contention with console signals
- Use open-drain style outputs where appropriate
- Use pull-ups or pull-downs to define default states
- Avoid floating enable lines
- Avoid backfeeding unpowered boards
- Add current limiting where needed
- Use transistor or MOSFET drivers for loads
- Keep logic levels compatible
- Test every output before connecting to a real console

---

## Touchscreen Button Requests

The touchscreen may request button-like actions from the interposer.

Possible touchscreen requests:

- Request power button press
- Request reset
- Request power off
- Request toggle output
- Request enter service mode
- Request wake screen
- Request accessory power state
- Request mod state change

The interposer should validate the request before performing the action.

For example, a reset request should not be accepted if the system is in a lockout state.

---

## UART Button Reports

Button Butler may report button events over UART.

Possible reports:

| Event | Example Meaning |
| --- | --- |
| Button pressed | Physical button went active |
| Button released | Physical button returned inactive |
| Short press | Valid short press detected |
| Long press | Valid long press detected |
| Double press | Valid double press detected |
| Lid open | Lid switch changed to open |
| Lid closed | Lid switch changed to closed |
| Action blocked | Normal button action was blocked |
| Action passed | Button action was passed through |
| Output changed | An output state changed |

The exact message format can be defined later in the firmware protocol notes.

---

## LED Or Chime Feedback

Button actions may need feedback so the user knows something happened.

Possible feedback methods:

- Status LED blink
- RGB color change
- Touchscreen message
- Short chime
- Long chime
- Different chime for error or success
- On-screen confirmation

Feedback should be helpful but not annoying.

For hidden actions, feedback is especially important.

---

## Example Feedback Ideas

| Action | Possible Feedback |
| --- | --- |
| ChipSlayer enabled | One short chime or LED flash |
| ChipSlayer disabled | Two short chimes or LED flashes |
| Bluetooth pairing | Slow blinking LED |
| Modchip disabled | On-screen warning or status icon |
| Service mode entered | Different chime pattern |
| Action blocked | Error beep or red status indicator |
| Touchscreen command accepted | On-screen confirmation |

These are only concept ideas.

---

## User-Facing Behavior

For normal users, the button behavior should stay simple.

A customer should not need to memorize a complicated button manual just to use the console.

Recommended user-facing rule:

- Normal press should behave normally.
- Advanced actions should be intentional.
- Service actions should be hidden or protected.
- Dangerous actions should require confirmation.

The more advanced the build is, the more the touchscreen can help explain what is happening.

---

## Installer-Facing Behavior

For the installer, Button Butler can expose more powerful behavior.

Installer-facing functions may include:

- Test all outputs
- Show input states
- Toggle mod states
- View UART logs
- Test button timing
- Test lid switch behavior
- Test accessory power control
- Enter safe mode
- Reset configuration

These should be separate from normal customer-facing behavior.

---

## Development Testing Plan

Early testing should focus on the simplest behavior first.

Recommended testing order:

1. Read physical button input
2. Debounce button input
3. Detect press and release
4. Detect short press
5. Detect long press
6. Pass button through normally
7. Block button intentionally
8. Add lid switch input
9. Add lid-open alternate action
10. Add one output toggle
11. Add UART event reporting
12. Add touchscreen command testing later

Do not add advanced touchscreen features until the base button behavior is proven.

---

## Open Questions

Open questions for development:

- What is the safest way to electrically intercept the PS2 power/reset button line?
- Should the default hardware state be pass-through?
- How should the system behave if the microcontroller is unpowered?
- Should lid switch behavior be required or optional?
- How many button press patterns are practical for customers?
- Should profiles be selected by firmware, jumpers, or touchscreen?
- How should settings be stored?
- Should outputs default to off, high impedance, or a defined state?
- How should touchscreen commands be authenticated or protected?
- Should service mode be hidden behind a password or physical condition?

These questions should be answered as prototypes are tested.

---

## Risks

Possible risks:

- Button line interference
- Console stuck in reset
- Accidental power off
- Firmware lockup
- Bad default output states
- Backfeeding through control lines
- Customer confusion
- Too many button combinations
- Touchscreen command errors
- Different PS2 revisions behaving differently

These risks should be addressed with careful hardware design, firmware logic, and testing.

---

## Summary

Button behavior is the core of the Button Butler project.

The first goal is to safely monitor and pass through the original PS2 power/reset button behavior.

After that works, Button Butler can add hidden actions, lid-switch modifiers, mod control outputs, touchscreen commands, and service features.

The button should remain easy to use, but powerful enough to control advanced internal PS2 mods without adding extra holes, switches, or clutter to the console shell.
