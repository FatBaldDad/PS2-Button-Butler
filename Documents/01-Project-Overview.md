# 01 - Project Overview

## FBD PS2 Button Butler

**FBD PS2 Button Butler** is a modular PlayStation 2 power/reset button interposer, I/O controller, and optional touchscreen control hub for advanced internal PS2 mods.

The project is being developed as a flexible FBD hardware platform that can scale from a simple power/reset button behavior controller to a full touchscreen-based mod management system.

At its most basic level, Button Butler gives the original PS2 power/reset button smarter behavior.

At its highest level, Button Butler becomes a central control system for managing internal mods, lighting, Bluetooth features, video board power, console state, and future diagnostic feedback systems.

---

## Project Purpose

The purpose of Button Butler is to make complex PS2 builds cleaner, easier to control, and easier to install.

Modern custom PS2 builds can contain many internal mods, including BlueRetro, SD2PSX, modchips, Bluetooth audio, RGB lighting, RetroGEM, ElectronAnalog, internal network devices, and other custom FBD boards.

Each added feature often needs its own switch, button, LED, or access point. That can quickly lead to too many wires, too many holes in the shell, and too many separate control methods.

Button Butler is intended to solve that problem by creating one smart control system that can manage multiple internal features.

---

## Basic Concept

The base version of Button Butler is an interposer board that connects between the PS2 power/reset button circuit and the console.

Instead of the button only performing its original function, Button Butler can monitor the button press and decide what should happen.

Possible behavior examples:

- Pass the button press through normally
- Block the normal button behavior
- Detect a short press
- Detect a long press
- Detect a double press
- Detect the lid switch state
- Trigger an output when a certain condition is met
- Send a command to another board
- Allow a touchscreen to request power or reset actions

This turns the original PS2 power/reset button into a programmable control point.

---

## Why This Project Exists

The main reason for Button Butler is install cleanliness.

A heavily modified PS2 can quickly become crowded with extra boards, wires, switches, and indicators. Every extra physical control usually means more shell modification, more installation time, and more chances for the final build to look cluttered.

Button Butler is designed to reduce that problem.

The project is focused on:

- Fewer external switches
- Less drilling and cutting
- Cleaner shell appearance
- More flexible internal control
- Easier customer-facing operation
- Better organization of complex internal mods
- A more finished and premium FBD build experience

The goal is not just to add another board. The goal is to make the whole PS2 modding experience cleaner and more controlled.

---

## Intended Use

Button Butler is intended for custom PlayStation 2 builds where multiple internal mods need to be controlled or monitored.

Possible supported systems include:

- BlueRetro
- SD2PSX
- MemCard Pro 2 / MMCE-related setups
- ModBo 5.0 or other modchips
- ChipSlayer
- Bluetooth audio boards
- RGB lighting
- RetroGEM
- ElectronAnalog
- Internal routers or network devices
- Future LazyrSavre-style laser feedback boards
- Future FBD power and I/O boards

Not every build will need every feature. The project is intended to be modular so the installer can choose the level of integration needed.

---

## Integration Levels

Button Butler is planned around multiple levels of installation.

---

### Level 1 - Basic Button Interposer

This is the simplest version of the project.

The base interposer board would monitor the PS2 power/reset button and allow simple behavior changes.

Possible features:

- Normal power/reset pass-through
- Short press detection
- Long press detection
- Lid switch detection
- One or more control outputs
- UART for configuration, debugging, or future expansion

This level is intended to be easy to install and useful even without a touchscreen.

---

### Level 2 - Button Interposer With I/O Control

This level adds more control outputs and inputs.

Possible uses:

- Enable or disable ChipSlayer
- Enable or disable a modchip
- Trigger Bluetooth audio pairing
- Enable or disable Bluetooth audio
- Turn RGB lighting on or off
- Control status LEDs
- Trigger a chime or buzzer circuit
- Control accessory power
- Monitor lid switch or service states

This turns Button Butler into a practical internal mod controller.

---

### Level 3 - Touchscreen Control Hub

This is the premium version of Button Butler.

A touchscreen interface would give the user direct control over installed mods and console functions.

Possible touchscreen functions:

- Power on the console
- Power off the console
- Reset the console
- Enable or disable BlueRetro
- Control BlueRetro channels
- Enable or disable Bluetooth audio
- Put Bluetooth audio into pairing mode
- Enable or disable a modchip
- Enable or disable ChipSlayer
- Control RGB lighting
- Control RetroGEM or ElectronAnalog power
- Display SD2PSX information
- Display laser feedback or diagnostic information
- Access service or settings pages

This version turns Button Butler into a full control panel for advanced FBD PS2 builds.

---

## Core Hardware Blocks

The Button Butler project may eventually include several hardware blocks.

---

### Button Interposer Board

The Button Interposer Board is the core of the project.

This board connects to the PS2 power/reset button circuit and allows the button behavior to be monitored, modified, or passed through.

Expected features:

- Small microcontroller, likely a PIC
- Power/reset button input
- Console power/reset control output
- Lid switch input
- UART connection
- Digital inputs
- Digital outputs
- Expansion connector

This board is the first major development focus.

---

### I/O Expansion Board

The I/O Expansion Board is intended to provide extra raw inputs and outputs for builds that need more control points.

Possible uses:

- Lighting control
- Mod enable lines
- Status LEDs
- Relay or MOSFET control
- Extra button inputs
- Accessory enable outputs
- Future expansion signals

This board gives Button Butler more flexibility without forcing every feature onto the main interposer board.

---

### Touchscreen Hub

The Touchscreen Hub is the user interface layer for advanced builds.

This may use a small round display or touchscreen module mounted in a clean location on the console.

Possible uses:

- Menu system
- Mod control pages
- Power control page
- Status display
- Service page
- Settings page
- Diagnostic display

The touchscreen is not required for the basic version of Button Butler, but it is part of the long-term vision.

---

### Power Management Section

Button Butler may include or control power management hardware for internal accessories.

Possible uses:

- Power down RetroGEM when the console is idle
- Power down ElectronAnalog when not needed
- Reduce internal heat
- Save energy
- Control accessory rails
- Coordinate internal devices with console power state

This is especially useful for compact or heavily modified PS2 builds where heat and power management matter.

---

## Example Control Ideas

Button Butler could support many different control behaviors depending on the build.

Examples:

- Press power normally to turn the console on or off
- Hold power to reset
- Press power while the lid is open to toggle ChipSlayer
- Hold power while the lid is open to enter a service mode
- Use the touchscreen to disable BlueRetro
- Use the touchscreen to pair Bluetooth audio
- Use the touchscreen to disable the modchip
- Automatically power down video hardware when the console is idle
- Use lighting to show which mode is active
- Display laser or servo feedback from a future diagnostic board

These are concept examples and may change as the hardware and firmware are developed.

---

## Communication

Button Butler is expected to use simple and reliable communication methods.

Possible communication methods:

- UART
- Digital I/O
- Simple command protocol
- Future I2C or other expansion options if needed

The first goal is to keep the system easy to test and debug.

A complicated communication system is not useful if it makes installation unreliable. The project should stay practical and serviceable.

---

## Design Philosophy

Button Butler is being designed around real PS2 modding needs.

The project priorities are:

1. Clean installation
2. Minimal shell modification
3. Reliable button behavior
4. Simple wiring
5. Flexible mod control
6. Easy troubleshooting
7. Expandable hardware
8. Practical use in customer builds
9. Premium FBD presentation

This project is intended to solve real problems that come up when building heavily modified PS2 consoles.

---

## What Makes Button Butler Different

Button Butler is not just a power button board.

The project is different because it is intended to act as a bridge between the user, the console, and several internal mods.

Instead of each mod needing its own switch or access point, Button Butler can become the common control layer.

The original PS2 power/reset button becomes more useful.

The touchscreen version expands that idea even further by giving the console a modern control interface without turning the shell into a mess of switches and holes.

---

## Current Development Focus

The first development focus is the base Button Interposer Board.

Initial goals:

- Define the PS2 power/reset button behavior
- Study how to safely intercept the button signal
- Determine how to monitor the lid switch
- Choose the first PIC or microcontroller
- Define basic input and output needs
- Add UART support for debugging and future expansion
- Create a prototype schematic
- Build a test PCB
- Validate basic button behavior
- Prove the concept before adding touchscreen control

The touchscreen version should come after the base interposer behavior is proven.

---

## Planned Project Phases

### Phase 1 - Concept Documentation

Goals:

- Document the project idea
- Define the feature levels
- Define possible use cases
- Organize the repo
- Track related FBD projects
- Create early hardware notes

---

### Phase 2 - Base Interposer Prototype

Goals:

- Design the first button interposer board
- Select the microcontroller
- Define input and output pins
- Add UART support
- Test power/reset pass-through
- Test button press detection
- Test lid switch behavior
- Confirm safe behavior on real PS2 hardware

---

### Phase 3 - I/O Expansion

Goals:

- Define the raw I/O expansion board
- Add outputs for lights and mod control
- Test control of ChipSlayer
- Test control of Bluetooth audio functions
- Test status LEDs or RGB functions
- Document wiring examples

---

### Phase 4 - Touchscreen Interface

Goals:

- Choose touchscreen hardware
- Define the UI pages
- Define the command structure
- Add console power/reset controls
- Add mod control pages
- Add status display features
- Test communication between the touchscreen and interposer

---

### Phase 5 - Advanced Mod Integration

Goals:

- Add BlueRetro control ideas
- Add SD2PSX display or control ideas
- Add RetroGEM and ElectronAnalog power management
- Add Bluetooth audio control
- Add laser feedback display concepts
- Add build-specific profiles

---

### Phase 6 - Product Development

Goals:

- Finalize hardware revisions
- Create BOMs
- Create assembly notes
- Create install documentation
- Create QA checklists
- Test in multiple PS2 models
- Prepare for possible FBD product release

---

## Project Scope

Button Butler is intended to cover:

- PS2 power/reset button behavior control
- Button interposer hardware
- Lid switch based behavior options
- Digital input and output control
- UART communication
- Optional touchscreen control
- Mod control outputs
- Internal accessory power management
- Installation documentation
- Testing and validation notes
- Future expansion support

---

## Out of Scope For Early Prototypes

The early versions of Button Butler are not expected to do everything.

The following features may be delayed until later:

- Full touchscreen UI
- Advanced graphical menus
- Full BlueRetro configuration
- Full SD2PSX integration
- Complete laser diagnostic display
- Automatic service procedures
- Finished customer-ready product packaging
- Support for every PS2 motherboard revision

The first goal is to prove the base interposer board.

---

## Safety and Reliability Goals

Button Butler must behave safely and predictably.

Important safety goals:

- Do not accidentally hold the console in reset
- Do not prevent the console from powering on
- Do not fight the original PS2 button circuit
- Do not create unsafe backfeeding paths
- Do not leave mod control outputs floating
- Do not cause random resets
- Do not require permanent shell damage for basic installation
- Fail in a safe or pass-through state whenever possible

Because this project interacts with console power and reset behavior, reliability is more important than flashy features.

---

## Install Goals

Button Butler should be designed with installation in mind from the start.

Install goals:

- Minimal solder points where possible
- Clear wiring labels
- Simple harness options
- Clean board placement
- No unnecessary shell drilling
- No unnecessary shell cutting
- Easy access for testing
- Serviceable after installation
- Compatible with premium FBD build layouts

A major goal is to make advanced PS2 mods look intentional instead of improvised.

---

## Future Potential

Button Butler has a lot of possible future expansion.

Possible future features:

- Build-specific configuration profiles
- Touchscreen themes
- Password-protected service pages
- Laser feedback display
- SD2PSX status display
- BlueRetro status display
- Modchip state display
- RGB presets
- Power sequencing
- Console idle detection
- Accessory temperature or power monitoring
- UART logging
- Diagnostic screens

Not all of these features are required for the first version, but they show the long-term direction of the project.

---

## Related FBD Projects

Button Butler may eventually connect to or control other FBD projects.

Related projects include:

- ChipSlayer
- PowerOR
- LazyrSavre
- Internal BlueRetro installs
- Internal SD2PSX installs
- PS2 Ultra Slim builds
- PS2 Blue Steel-style builds
- RetroGEM / ElectronAnalog power management concepts

Button Butler is intended to act as a central control layer that can tie these types of projects together.

---

## Development Notes

This project is still early.

Many details may change, including:

- Microcontroller choice
- Connector choice
- Board size
- Pinout
- Communication protocol
- Touchscreen hardware
- Power control method
- Supported mods
- Firmware behavior

The repo should be treated as a living design notebook until the hardware is tested and finalized.

---

## Long-Term Vision

The long-term vision is for Button Butler to become a flagship FBD PS2 mod platform.

At the basic level, it is a smarter power/reset button.

At the intermediate level, it is a mod control and I/O system.

At the advanced level, it is the touchscreen control center for the entire console.

The end goal is to make complex PS2 builds easier to use, cleaner to install, and more professional-looking without losing the handmade FBD character of the build.

---

## Disclaimer

This project is not affiliated with Sony, PlayStation, BlueRetro, SD2PSX, RetroGEM, ElectronAnalog, or any other third-party project mentioned in this document.

PlayStation 2 and related names belong to their respective owners.

This project involves internal console modification and should only be attempted by people comfortable with electronics, soldering, and hardware troubleshooting.

Prototype hardware should be tested carefully before being installed into a customer console.
