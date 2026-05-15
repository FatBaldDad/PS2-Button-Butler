# 02 - Integration Levels

## Overview

The **FBD PS2 Button Butler** is designed as a modular project that can be installed at different levels depending on the build, the customer, the console model, and the amount of control needed.

Not every PS2 build needs the full touchscreen version.

Some builds may only need a smarter power/reset button. Other builds may need several digital outputs for controlling internal mods. Premium builds may use the touchscreen as a full control hub for the console.

The goal is to make Button Butler useful at every level, from simple to advanced.

---

## Why Integration Levels Matter

PS2 builds can vary a lot.

A basic build may only have:

- A modchip
- Internal HDMI
- A serviced optical drive
- Basic homebrew support

A more advanced build may have:

- Internal BlueRetro
- SD2PSX
- Bluetooth audio
- RGB lighting
- ChipSlayer
- RetroGEM or ElectronAnalog
- Internal network hardware
- Custom power management
- Laser feedback or diagnostic hardware

Because the hardware needs can be very different, Button Butler should not be designed as an all-or-nothing system.

Instead, it should support several installation levels.

This keeps the project flexible, easier to test, and easier to sell as different options.

---

## Main Integration Levels

Button Butler is planned around three main integration levels:

1. Basic Button Interposer
2. Button Interposer With I/O Control
3. Touchscreen Control Hub

Each level builds on the one before it.

---

## Level 1 - Basic Button Interposer

Level 1 is the simplest version of Button Butler.

This version focuses only on the PS2 power/reset button behavior.

The goal is to place a small interposer board between the original PS2 power/reset button and the console so the button can be monitored, modified, or passed through.

---

### Level 1 Purpose

The purpose of Level 1 is to prove the core idea.

Button Butler should be able to safely watch the power/reset button and decide what to do with the button press.

This version should be simple, reliable, and install-friendly.

---

### Level 1 Possible Features

Possible Level 1 features include:

- Normal power/reset pass-through
- Short press detection
- Long press detection
- Optional double press detection
- Lid switch detection
- Safe button blocking
- One or two basic control outputs
- UART for debugging or future configuration
- Fail-safe or pass-through style behavior where possible

---

### Level 1 Example Behaviors

Examples of possible Level 1 behaviors:

- Short press performs normal power/reset action
- Long press performs a reset
- Press while lid is open triggers an alternate output
- Press while lid is open blocks the normal power/reset action
- Long press while lid is open enters a service mode
- Button press is logged over UART for debugging

These behaviors are concept examples and may change during testing.

---

### Level 1 Hardware

The Level 1 board would likely include:

- Small PIC or similar microcontroller
- Power/reset button input
- Console-side power/reset output
- Lid switch input
- One or more digital outputs
- UART pads or connector
- Basic programming pads
- Simple status LED pads if useful

This board should be as small and easy to install as possible.

---

### Level 1 Install Goal

The Level 1 install should avoid unnecessary shell modification.

Install goals:

- Minimal soldering
- Small board footprint
- Clear wire labels
- Easy test points
- No extra external switches required
- No touchscreen required
- Simple enough for repeat installs

This level should be the foundation of the entire project.

---

## Level 2 - Button Interposer With I/O Control

Level 2 expands the base interposer concept by adding more control outputs and inputs.

This version turns Button Butler into a basic internal mod controller.

Instead of only changing button behavior, it can control other internal boards or accessories.

---

### Level 2 Purpose

The purpose of Level 2 is to give the PS2 a cleaner way to control multiple internal mods without adding extra external switches.

This level is useful for builds that need hidden control over features like ChipSlayer, Bluetooth audio, RGB lighting, or modchip state.

---

### Level 2 Possible Features

Possible Level 2 features include:

- Everything from Level 1
- Additional digital outputs
- Additional digital inputs
- Mod enable or disable outputs
- LED or RGB control outputs
- Bluetooth audio pairing trigger
- ChipSlayer enable or disable control
- Modchip enable or disable control
- Accessory power enable outputs
- Simple UART command handling
- Optional I/O expansion board

---

### Level 2 Example Behaviors

Examples of possible Level 2 behaviors:

- Press power normally to control the console
- Hold power to toggle an internal mod
- Press power with the lid open to enable ChipSlayer
- Double press power to toggle RGB lighting
- Long press power to trigger Bluetooth pairing
- Use a UART command to turn an output on or off
- Automatically disable an accessory when the console is off
- Use an output to control a chime or buzzer circuit

These behaviors would be configurable by firmware or build-specific hardware options.

---

### Level 2 Hardware

The Level 2 system may include the base interposer board plus either more onboard I/O or a separate expansion board.

Possible hardware blocks:

- Button Interposer Board
- I/O Expansion Board
- MOSFET or transistor output stages
- Open-drain or open-collector style outputs
- LED output pads
- Accessory enable outputs
- UART connector
- Expansion connector

The I/O Expansion Board keeps the base interposer smaller while allowing more advanced builds to add extra outputs.

---

### Level 2 Use Cases

Level 2 could support several useful real-world PS2 modding cases.

---

#### ChipSlayer Control

Button Butler could control a ChipSlayer enable line.

Possible actions:

- Enable ChipSlayer
- Disable ChipSlayer
- Toggle ChipSlayer from the power button
- Toggle ChipSlayer from a hidden button behavior
- Show ChipSlayer state with an LED

---

#### Modchip Control

Button Butler could control a modchip enable or disable circuit.

Possible actions:

- Enable the modchip
- Disable the modchip
- Toggle the modchip state
- Change modchip state during startup
- Use a hidden button action to avoid an external switch

---

#### Bluetooth Audio Control

Button Butler could control a Bluetooth audio board.

Possible actions:

- Enable Bluetooth audio
- Disable Bluetooth audio
- Trigger pairing mode
- Reset the Bluetooth audio board
- Show pairing state with an LED or touchscreen later

---

#### RGB and Lighting Control

Button Butler could control lighting features.

Possible actions:

- Turn lighting on or off
- Toggle lighting modes
- Show console status through lighting
- Show mod state through lighting
- Disable lights when the console is idle

---

#### Accessory Power Control

Button Butler could enable or disable accessory power rails.

Possible actions:

- Power down video hardware when not needed
- Disable internal accessories when the console is off
- Reduce heat inside compact builds
- Save power
- Coordinate power state with the console

---

## Level 3 - Touchscreen Control Hub

Level 3 is the full premium version of Button Butler.

This version adds a touchscreen interface and turns Button Butler into a control hub for the entire console.

The touchscreen does not replace the base interposer board. It builds on top of it.

The interposer handles the real power/reset behavior. The touchscreen gives the user a clean interface to request actions and view status.

---

### Level 3 Purpose

The purpose of Level 3 is to create a clean, modern control interface for high-end FBD PS2 builds.

Instead of hiding multiple switches or drilling several holes in the shell, the touchscreen can provide one organized control point.

This level is intended for premium builds where the console has several internal mods that benefit from user control.

---

### Level 3 Possible Features

Possible Level 3 features include:

- Everything from Level 1 and Level 2
- Touchscreen menu system
- Console power control
- Console reset control
- Modchip control page
- ChipSlayer control page
- BlueRetro control page
- Bluetooth audio control page
- SD2PSX information page
- RGB lighting control page
- Video board power control page
- Service page
- Diagnostic page
- Settings page
- Build-specific profiles
- Status display for internal hardware

---

### Level 3 Example Behaviors

Examples of possible Level 3 behaviors:

- Tap a button on the screen to power the console on
- Tap a button on the screen to reset the console
- Turn BlueRetro on or off from the screen
- Disable individual BlueRetro channels
- Put Bluetooth audio into pairing mode
- Turn RGB lighting on or off
- Enable or disable a modchip
- Enable or disable ChipSlayer
- Show SD2PSX status
- Show laser feedback from a future LazyrSavre-style board
- Power down RetroGEM or ElectronAnalog when the console is idle
- Open a password-protected service page

---

### Level 3 Hardware

The Level 3 system may include:

- Button Interposer Board
- I/O Expansion Board
- Touchscreen module
- Touchscreen controller
- UART connections
- Accessory power control outputs
- Optional status LEDs
- Optional chime or buzzer output
- Optional service/debug connector

The touchscreen controller may communicate with the interposer board using UART or another simple communication method.

---

## Optional Level 4 - Full Build Management System

Level 4 is not required for the first version, but it represents the long-term direction of Button Butler.

At this level, Button Butler becomes a full internal build management system.

This would combine:

- Touchscreen control
- Button interposer control
- Mod control
- Accessory power control
- UART status from other devices
- Diagnostic information
- Build-specific configuration
- Service menus
- Logging or status reporting

This level would be for the most advanced FBD builds.

---

### Level 4 Possible Features

Possible future Level 4 features include:

- BlueRetro status display
- SD2PSX status display
- Game ID related information
- Laser feedback display
- Console state detection
- Accessory current or voltage monitoring
- Temperature monitoring
- Power sequencing
- Idle detection
- Service mode controls
- Firmware configuration menus
- Build profile selection
- Diagnostic logs

This is a future concept and should not block the first prototypes.

---

## Recommended Development Order

The recommended order is:

1. Build and test Level 1 first
2. Add Level 2 I/O control after the button behavior is proven
3. Add the touchscreen after the interposer and I/O logic are stable
4. Add advanced status and diagnostic features last

This avoids making the first prototype too complicated.

The most important early goal is to prove that Button Butler can safely and reliably control the PS2 power/reset button behavior.

---

## Feature Comparison Table

| Feature | Level 1 Basic Interposer | Level 2 I/O Control | Level 3 Touchscreen Hub | Future Level 4 Build Manager |
| --- | --- | --- | --- | --- |
| Normal power/reset pass-through | Yes | Yes | Yes | Yes |
| Short press detection | Yes | Yes | Yes | Yes |
| Long press detection | Yes | Yes | Yes | Yes |
| Lid switch detection | Yes | Yes | Yes | Yes |
| UART support | Basic | Yes | Yes | Yes |
| Digital outputs | Limited | Yes | Yes | Yes |
| Digital inputs | Limited | Yes | Yes | Yes |
| ChipSlayer control | Possible | Yes | Yes | Yes |
| Modchip control | Possible | Yes | Yes | Yes |
| Bluetooth audio control | Possible | Yes | Yes | Yes |
| RGB lighting control | No / Limited | Yes | Yes | Yes |
| Accessory power control | No / Limited | Yes | Yes | Yes |
| Touchscreen UI | No | No | Yes | Yes |
| SD2PSX display/control | No | Possible | Yes | Yes |
| BlueRetro display/control | No | Possible | Yes | Yes |
| Laser feedback display | No | Possible | Possible | Yes |
| Diagnostic pages | No | No / Limited | Possible | Yes |
| Build profiles | No | Possible | Possible | Yes |

---

## Build Type Examples

Different FBD builds may use different Button Butler levels.

---

### Simple Modchip Build

Recommended level:

- Level 1 or Level 2

Possible features:

- Normal power/reset behavior
- Hidden modchip enable/disable option
- Lid switch based alternate action
- Simple status LED

---

### BlueRetro Build

Recommended level:

- Level 2 or Level 3

Possible features:

- BlueRetro enable/disable
- Controller channel enable/disable
- Pairing support
- Touchscreen control in premium builds

---

### Bluetooth Audio Build

Recommended level:

- Level 2 or Level 3

Possible features:

- Bluetooth audio enable/disable
- Pairing mode trigger
- Status LED
- Touchscreen pairing button

---

### SD2PSX Build

Recommended level:

- Level 3 or Future Level 4

Possible features:

- SD2PSX status display
- Possible UART communication
- Menu page for memory card related information
- Build-specific display features

---

### Premium FBD Ultra Slim Build

Recommended level:

- Level 3 or Future Level 4

Possible features:

- Touchscreen console control
- BlueRetro control
- SD2PSX information
- Modchip control
- ChipSlayer control
- RGB control
- Bluetooth audio control
- Video board power management
- Service or diagnostic pages

---

## Design Rules For Each Level

Each integration level should follow the same basic design rules.

---

### Keep Level 1 Simple

Level 1 should not become overloaded.

It should focus on:

- Button input
- Console output
- Lid switch input
- Basic I/O
- UART
- Reliable pass-through behavior

If Level 1 becomes too complicated, the whole project becomes harder to test.

---

### Keep Level 2 Flexible

Level 2 should provide useful I/O without forcing every feature into every build.

It should support:

- Optional outputs
- Optional expansion
- Build-specific wiring
- Clear labels
- Easy testing

---

### Keep Level 3 User-Friendly

Level 3 should be clean and simple for the user.

The touchscreen should not feel like a confusing service tool.

It should provide:

- Clear menu names
- Simple on/off controls
- Status indicators
- Safety confirmations where needed
- Hidden or protected service functions

---

### Keep Future Level 4 Experimental

Level 4 should remain experimental until the base system is proven.

Advanced features should be added only after the core hardware is reliable.

---

## Safety Notes

Button Butler interacts with power and reset behavior, so each integration level must be tested carefully.

Important safety rules:

- The console should not be held in reset accidentally
- The console should not be prevented from powering on
- Outputs should not float into unsafe states
- Accessory control lines should not backfeed the console
- The interposer should not fight the original button circuit
- The system should fail safely where possible
- Touchscreen commands should not trigger dangerous behavior accidentally
- Customer builds should not receive untested prototype behavior

Reliability matters more than feature count.

---

## Installation Notes

The installation should remain clean at every level.

Design goals:

- Use clear labels
- Use simple wire routing
- Avoid unnecessary shell holes
- Keep board placement practical
- Make debugging accessible
- Keep the system serviceable
- Avoid making the install depend on one fragile wire
- Avoid features that are too hard to repeat across multiple builds

The whole point of Button Butler is to make advanced mods cleaner, not more chaotic.

---

## Summary

Button Butler is designed as a scalable PS2 mod control system.

At Level 1, it is a smarter power/reset button interposer.

At Level 2, it becomes a practical I/O controller for internal mods.

At Level 3, it becomes a touchscreen control hub for premium builds.

At a future Level 4, it could become a full internal build management and diagnostic system.

The recommended development path is to start small, prove the base button interposer, then expand into I/O control and touchscreen features after the core behavior is reliable.
