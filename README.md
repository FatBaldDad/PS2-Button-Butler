<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="Images/Logos/FBD-PS2-Button-Butler-Logo.png">
    <source media="(prefers-color-scheme: light)" srcset="Images/Logos/FBD-PS2-Button-Butler-Logo-NoBack-1024.png">
    <img src="Images/Logos/FBD-PS2-EtherDrive-Logo.png" alt="FBD PS2-EtherDrive Logo" width="500">
  </picture>
</p>

# FBD PS2 Button Butler

**FBD PS2 Button Butler** is a modular PlayStation 2 power/reset button interposer, I/O controller, and optional touchscreen control hub designed for advanced internal PS2 mods.

This project is currently a **work in progress** and is one of my largest PlayStation 2 modding projects. The goal is to create a flexible FBD hardware platform that can scale from a simple power/reset button behavior controller all the way up to a full touchscreen-based mod management system.

At its most basic level, Button Butler is a replacement/interposer for the PS2 power/reset button circuit. At its highest level, it becomes a central control system for managing internal mods, lighting, Bluetooth features, power states, and future feedback systems.

---

## Project Status

**Status:** Early development / concept documentation  
**Hardware:** In planning and prototype stages  
**Firmware:** In planning  
**Commercial use:** Intended to become a future FBD product  

This repository is being used to document the idea, track development, organize hardware revisions, and preserve the design direction as the project grows.

---

## What Is Button Butler?

Button Butler is intended to sit between the PlayStation 2 console and the original power/reset button behavior.

Instead of the button only doing what Sony originally designed it to do, Button Butler allows the button behavior to be intercepted, modified, expanded, or passed through depending on the console state and installed mods.

For example:

- Pressing the button normally can still power or reset the console.
- Holding the button could trigger a different function.
- Pressing the button while the lid switch is open could trigger a special mode.
- Button behavior could be blocked, redirected, or used to control another mod.
- A touchscreen could take full control of console and mod behavior.

The goal is to give custom PS2 builds more control without drilling extra holes or adding a pile of switches to the shell.

---

## Core Idea

The base version of Button Butler is an interposer board with a small microcontroller, likely a PIC, that provides:

- Power/reset button monitoring
- Power/reset behavior control
- UART communication
- Digital inputs
- Digital outputs
- Lid switch awareness
- Mod enable/disable outputs
- Expansion support for other internal boards

This allows the PS2 power button to become more than just a power/reset button.

It becomes a programmable control point for the whole build.

---

## Why This Exists

Modern PS2 builds can include a lot of internal mods:

- BlueRetro
- SD2PSX
- MemCard Pro 2 / MMCE-related setups
- ModBo 5.0 or other modchips
- ChipSlayer
- Bluetooth audio
- RGB lighting
- RetroGEM
- ElectronAnalog
- Internal routers or network devices
- Laser protection or feedback boards
- Custom power systems

The problem is that every added feature usually needs one or more of the following:

- A switch
- A button
- An LED
- A hole in the shell
- Extra wiring
- A hard-to-reach internal control point

Button Butler is intended to clean that up.

Instead of adding a bunch of external switches, Button Butler gives the build one smarter control system.

---

## Main Goals

- Keep PS2 installs clean and simple
- Reduce the need to drill or cut extra holes in the PS2 shell
- Reuse the original power/reset button as a smart control input
- Support both simple and advanced installs
- Allow different mods to be turned on or off from one system
- Add optional touchscreen control for premium builds
- Provide raw digital I/O for lighting, control signals, and future features
- Help manage heat and power usage inside heavily modified consoles
- Create a true FBD-branded PS2 mod platform

---

## Integration Levels

Button Butler is intended to support multiple levels of installation.

---

### Level 1: Basic Button Interposer

The simplest version controls or modifies the normal PS2 power/reset button behavior.

Possible functions:

- Pass through normal button behavior
- Block button behavior under certain conditions
- Detect short press / long press
- Detect lid switch state
- Trigger an output when a button condition is met
- Provide a UART connection for future control or debugging

This version is intended to be the easiest to install.

---

### Level 2: Button Interposer With I/O Control

The next level adds general-purpose outputs and inputs.

Possible uses:

- Enable or disable ChipSlayer
- Turn a modchip on or off
- Trigger Bluetooth audio pairing
- Enable or disable Bluetooth audio
- Turn RGB lighting on or off
- Control status LEDs
- Trigger a chime or buzzer circuit
- Control internal power rails
- Detect lid switch or service mode states

This version turns the power/reset button into a basic internal mod controller.

---

### Level 3: Touchscreen Control Hub

The highest level adds a touchscreen interface.

The touchscreen could control:

- Console power on/off
- Console reset
- RGB lighting
- Bluetooth audio enable/disable
- Bluetooth audio pairing
- BlueRetro enable/disable
- BlueRetro channel control
- Modchip enable/disable
- ChipSlayer enable/disable
- SD2PSX information or control
- RetroGEM or ElectronAnalog power management
- Laser feedback from future LazyrSavre-style boards
- Service or diagnostic pages

This turns Button Butler into a full control panel for advanced FBD PS2 builds.

---

## Example Use Cases

### Lid Switch + Power Button Behavior

One possible behavior:

If the lid switch is open and the power button is pressed, Button Butler could block the normal button behavior and perform a different action instead.

Examples:

- Enable ChipSlayer
- Disable ChipSlayer
- Put Bluetooth audio into pairing mode
- Toggle RGB lighting
- Enter a service menu
- Send a UART command to another board

This allows hidden functions without adding external buttons.

---

### Bluetooth Audio Control

Button Butler could provide outputs or commands for a Bluetooth audio board.

Possible functions:

- Turn Bluetooth audio on
- Turn Bluetooth audio off
- Enter pairing mode
- Show pairing status on the touchscreen
- Disable Bluetooth audio when not needed

---

### BlueRetro Control

Button Butler could work with internal BlueRetro installs.

Possible functions:

- Disable BlueRetro completely
- Disable specific controller channels
- Show BlueRetro status on a display
- Provide touchscreen controls for controller-related options
- Coordinate BlueRetro behavior with console power state

---

### Modchip / ChipSlayer Control

Button Butler could control systems such as ChipSlayer or other modchip enable/disable circuits.

Possible functions:

- Enable modchip
- Disable modchip
- Toggle ChipSlayer mode
- Use touchscreen control instead of a physical switch
- Use hidden button combinations for modchip state changes

---

### RetroGEM / ElectronAnalog Power Management

Button Butler could help manage power for internal video mods.

Possible functions:

- Power down RetroGEM or ElectronAnalog when the console is idle
- Reduce heat inside the console
- Save energy
- Control video board power from the touchscreen
- Coordinate power state with the PS2 console state

This is especially useful in compact builds where heat management matters.

---

### RGB and Lighting Control

Button Butler can also act as a simple lighting controller.

Possible functions:

- Turn case lighting on/off
- Control accent LEDs
- Trigger status colors
- Use lighting to show mod state
- Use lighting to show console mode

---

### Laser Feedback / LazyrSavre Expansion

Future versions may allow Button Butler to display feedback from laser protection or diagnostic boards.

Possible future data:

- Laser activity state
- Servo feedback
- Warning status
- Protection trigger status
- Diagnostic information
- Service mode feedback

This would allow advanced builds to show useful health information on the touchscreen.

---

## Hardware Blocks

The project may eventually include several boards.

---

### Button Interposer Board

The main board that connects to the PS2 power/reset button circuit.

Expected features:

- PIC microcontroller
- UART
- Digital inputs
- Digital outputs
- Button input
- Console control output
- Lid switch input
- Expansion connector

---

### I/O Expansion Board

A raw digital I/O board for extra functions.

Possible uses:

- Lighting control
- Mod enable lines
- Status LEDs
- Relay or MOSFET control
- Extra button inputs
- Future expansion

---

### Touchscreen Hub

The advanced display/control layer.

Possible features:

- Round touchscreen interface
- UART communication with Button Butler
- Menu system
- Mod control pages
- Status display
- Service pages
- BlueRetro or SD2PSX display support

---

### Power Management Board

Optional power-control circuitry for internal devices.

Possible uses:

- Power down video mods when idle
- Control internal accessory power
- Reduce heat
- Sequence power rails
- Coordinate console power state with accessory power

---

## Communication

Button Butler is expected to use simple communication methods that are easy to implement and debug.

Possible communication methods:

- UART
- Digital I/O
- Simple command protocol
- Future I2C or other expansion options if needed

The first goal is reliability and ease of installation, not unnecessary complexity.

---

## Design Philosophy

Button Butler is being designed around the way real PS2 mods are installed.

The priorities are:

1. Easy installation
2. Clean wiring
3. Minimal shell modification
4. Flexible control
5. Reliable behavior
6. Expandability
7. Practical use in real customer builds

This is not intended to be a gimmick. The goal is to solve real problems that come up when building heavily modified PS2 consoles.

---

## Project Goals

- Create a simple base interposer board
- Define button behavior modes
- Define UART command ideas
- Design a raw I/O expansion board
- Test outputs with common PS2 mods
- Add touchscreen support
- Add control profiles for different build types
- Test heat and power management features
- Build documentation for installation and manufacturing
- Develop this into a future FBD product

---

## Planned Features

- Power/reset button interposer
- Button press detection
- Short press / long press behavior
- Lid switch detection
- UART communication
- Digital I/O expansion
- Modchip control
- ChipSlayer control
- Bluetooth audio control
- BlueRetro control
- RGB control
- RetroGEM / ElectronAnalog power management
- SD2PSX display/control support
- Laser feedback display support
- Touchscreen UI
- Build-specific configuration profiles

---

## Repository Layout

| Folder | Purpose |
| --- | --- |
| `Documents/` | General project notes and design documentation |
| `Hardware/` | PCB concepts, schematics, KiCad files, and board notes |
| `Firmware/` | PIC firmware, touchscreen firmware, and protocol notes |
| `Images/` | Logos, concept art, board renders, and install photos |
| `Manufacturing/` | BOMs, assembly notes, QA checklists, and release planning |
| `Test-Data/` | Button tests, UART logs, power measurements, and validation data |
| `References/` | Datasheets, PS2 board notes, and related research |

---

## Suggested Folder Structure

| Path | Purpose |
| --- | --- |
| `Documents/01-Project-Overview.md` | High-level overview of the Button Butler project |
| `Documents/02-Integration-Levels.md` | Basic, intermediate, and touchscreen-based install levels |
| `Documents/03-Button-Behavior.md` | Notes for short press, long press, lid switch behavior, and special modes |
| `Documents/04-Touchscreen-Control.md` | Touchscreen control ideas, menu layout, and possible UI functions |
| `Documents/05-UART-and-IO-Architecture.md` | UART, digital I/O, and expansion communication notes |
| `Documents/06-Power-Management.md` | Power control ideas for video boards and internal accessories |
| `Documents/07-Mod-Control-Examples.md` | Example control cases for BlueRetro, ChipSlayer, Bluetooth audio, and other mods |
| `Documents/08-Installation-Goals.md` | Install-friendly design goals and shell-modification limits |
| `Documents/09-Future-Expansion.md` | Future ideas and possible expansion features |
| `Hardware/Button-Interposer/` | Base power/reset button interposer board files |
| `Hardware/IO-Expansion-Board/` | Raw I/O expansion board files |
| `Hardware/Touchscreen-Hub/` | Touchscreen control hub notes and hardware files |
| `Hardware/Power-Management/` | Accessory power-control board notes |
| `Firmware/PIC-Button-Interposer/` | Firmware for the base PIC interposer |
| `Firmware/Touchscreen-Controller/` | Firmware for the touchscreen controller |
| `Firmware/Protocol-Notes/` | UART command ideas and communication notes |
| `Manufacturing/BOM/` | Bills of materials |
| `Manufacturing/Assembly-Notes/` | Assembly instructions and install notes |
| `Manufacturing/QA-Checklist.md` | Testing checklist for completed boards |
| `Test-Data/` | Logs, measurements, and validation data |
| `References/` | Datasheets, pinouts, and research notes |

---

## Current Development Focus

The first development focus is the base Button Interposer board.

Initial goals:

- Intercept the PS2 power/reset button
- Detect button press behavior
- Monitor lid switch state
- Provide at least one UART connection
- Provide digital outputs for mod control
- Keep the install clean and simple
- Prove the concept before adding the touchscreen layer

---

## Long-Term Vision

The long-term vision is for Button Butler to become the central control system for premium FBD PS2 builds.

Instead of every mod needing its own switch, LED, or external access point, Button Butler becomes the smart interface between the user, the console, and the internal mods.

At the basic level, it is a smarter power/reset button.

At the advanced level, it is the control center for the whole console.

---

## Possible Future Touchscreen Pages

The touchscreen layer could eventually include several pages or modes.

Possible pages:

- Main power page
- Reset page
- Modchip / ChipSlayer page
- BlueRetro page
- Bluetooth audio page
- SD2PSX page
- RGB lighting page
- Video output power page
- Service page
- Diagnostic page
- Laser feedback page
- Settings page

These pages are only concept ideas at this stage.

---

## Possible Button Actions

Possible button actions may include:

- Short press
- Long press
- Double press
- Press while lid is open
- Press while console is off
- Press while console is on
- Press and hold during startup
- Touchscreen-requested virtual button press

Each action could be mapped to a different function depending on the build.

---

## Possible Outputs

Button Butler may control outputs for:

- Modchip enable
- ChipSlayer enable
- Bluetooth audio enable
- Bluetooth audio pairing
- BlueRetro enable
- RGB lighting
- Status LEDs
- Chime or buzzer output
- Accessory power enable
- RetroGEM power control
- ElectronAnalog power control
- SD2PSX display or control functions
- Future laser protection or feedback boards

---

## Important Notes

This project is experimental and under active development.

Do not install prototype hardware into a customer console without proper testing.

This repository may include unfinished ideas, untested circuits, early board layouts, and design notes that are subject to change.

---

## Intellectual Property / Project Notice

This repository documents the development of the FBD PS2 Button Butler concept and related hardware ideas.

Button Butler is intended to become an FBD-branded project and possible commercial product.

Unless a specific license is added later, this repository should be treated as documentation of an in-progress design, not as permission to manufacture or sell derivative products.

---

## Related FBD Projects

Button Butler may eventually interface with or support other FBD PS2 projects, including:

- ChipSlayer
- PowerOR
- LazyrSavre
- Internal BlueRetro installs
- Internal SD2PSX installs
- PS2 Ultra Slim builds
- PS2 Blue Steel-style builds
- RetroGEM / ElectronAnalog power management

---

## Credits

Project by **Fat Bald Dad / FBD Retro Game**.

This project is part of ongoing PlayStation 2 modding research, experimentation, and custom console development.

---

## Disclaimer

This project is not affiliated with Sony, PlayStation, RetroGEM, BlueRetro, SD2PSX, or any other third-party project mentioned in this repository.

PlayStation 2 and related names belong to their respective owners.

This project involves internal console modification and should only be attempted by people comfortable with electronics, soldering, and hardware troubleshooting.
