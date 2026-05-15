# Documents

## Overview

This folder contains the main planning and design documentation for the **FBD PS2 Button Butler** project.

The goal of these documents is to keep the project organized as it grows from a basic power/reset button interposer into a larger PS2 mod control platform.

Button Butler is currently a work in progress. These documents include early concepts, design goals, possible features, installation ideas, and future expansion plans.

---

## Purpose Of This Folder

The `Documents/` folder is used for high-level project planning.

This folder should contain:

- Project overview notes
- Integration level descriptions
- Button behavior planning
- Touchscreen control ideas
- UART and I/O architecture notes
- Power management planning
- Mod control examples
- Installation goals
- Future expansion ideas

Hardware files, firmware source code, manufacturing notes, and test data should live in their own folders.

---

## Document Index

| File | Description |
| --- | --- |
| `01-Project-Overview.md` | High-level overview of the Button Butler project, purpose, scope, and long-term vision |
| `02-Integration-Levels.md` | Explains the different levels of Button Butler installs, from basic interposer to full touchscreen hub |
| `03-Button-Behavior.md` | Defines possible power/reset button behaviors, shortcuts, lid-switch actions, and service modes |
| `04-Touchscreen-Control.md` | Covers touchscreen UI ideas, page concepts, command flow, profiles, and user-facing controls |
| `05-UART-and-IO-Architecture.md` | Covers UART communication, digital I/O, expansion boards, command ideas, and signal planning |
| `06-Power-Management.md` | Covers accessory power control, power domains, heat reduction, startup/shutdown behavior, and safety |
| `07-Mod-Control-Examples.md` | Lists example control methods for ChipSlayer, modchips, BlueRetro, Bluetooth audio, SD2PSX, RGB, video boards, and other mods |
| `08-Installation-Goals.md` | Covers install-friendly design goals, wire routing, shell modification limits, board placement, serviceability, and QA planning |
| `09-Future-Expansion.md` | Collects future ideas for hardware, firmware, touchscreen features, diagnostics, kits, and product expansion |

---

## Recommended Reading Order

For a new reader, the recommended order is:

1. `01-Project-Overview.md`
2. `02-Integration-Levels.md`
3. `03-Button-Behavior.md`
4. `05-UART-and-IO-Architecture.md`
5. `06-Power-Management.md`
6. `07-Mod-Control-Examples.md`
7. `08-Installation-Goals.md`
8. `04-Touchscreen-Control.md`
9. `09-Future-Expansion.md`

This order starts with the basic project idea, then moves into behavior, architecture, power, installation, and future expansion.

---

## Current Documentation Status

| Document | Status |
| --- | --- |
| `01-Project-Overview.md` | Draft / concept documentation |
| `02-Integration-Levels.md` | Draft / concept documentation |
| `03-Button-Behavior.md` | Draft / behavior planning |
| `04-Touchscreen-Control.md` | Draft / touchscreen planning |
| `05-UART-and-IO-Architecture.md` | Draft / architecture planning |
| `06-Power-Management.md` | Draft / power planning |
| `07-Mod-Control-Examples.md` | Draft / use case planning |
| `08-Installation-Goals.md` | Draft / install planning |
| `09-Future-Expansion.md` | Draft / future planning |

All documents are subject to change as hardware and firmware prototypes are built and tested.

---

## Main Project Concept

Button Butler is intended to be a modular PlayStation 2 control system.

At the basic level, it is a power/reset button interposer.

At the intermediate level, it becomes an I/O controller for internal mods.

At the advanced level, it becomes a touchscreen-based control hub for premium FBD PS2 builds.

The project is intended to help control features such as:

- Power/reset button behavior
- Lid-switch based alternate actions
- ChipSlayer enable/disable
- Modchip enable/disable
- Bluetooth audio pairing and enable control
- BlueRetro enable/reset/channel control
- RGB lighting
- SD2PSX status or display support
- RetroGEM or ElectronAnalog power management
- Internal router power or reset control
- Future LazyrSavre-style laser feedback
- Service and diagnostic features

---

## Design Priorities

The Button Butler project should be guided by a few main priorities.

1. Reliability first
2. Clean installation
3. Minimal shell modification
4. Safe default behavior
5. Simple wiring where possible
6. Serviceable board placement
7. Modular feature levels
8. Clear documentation
9. Practical use in real PS2 builds
10. Future product potential

The goal is not to add complexity for the sake of complexity.

The goal is to make advanced PS2 builds cleaner, easier to control, and easier to support.

---

## Development Direction

The recommended development path is:

1. Prove the basic power/reset button interposer.
2. Add lid switch detection.
3. Add one safe digital output.
4. Add UART debug/status reporting.
5. Add ChipSlayer or simple mod control.
6. Add I/O expansion.
7. Add touchscreen communication.
8. Add touchscreen UI pages.
9. Add advanced mod integration.
10. Add power management and diagnostics.
11. Create manufacturing and install documentation.
12. Prepare possible FBD product versions.

The project should grow in stages.

The first prototype should not try to include every future feature.

---

## Document Maintenance Notes

As the project develops, these documents should be updated with:

- Tested hardware behavior
- Confirmed wiring diagrams
- Finalized pinouts
- Known good button behavior
- Known bad ideas
- Firmware notes
- Supported PS2 models
- Unsupported PS2 models
- Installation photos
- Test results
- Safety findings
- Product revision notes

Concept ideas should be clearly separated from tested features.

---

## Suggested Status Labels

Future documentation can use these labels to track feature maturity.

| Status | Meaning |
| --- | --- |
| Concept | Idea only, not tested yet |
| Planned | Intended for future development |
| Prototype | Hardware or firmware is being tested |
| Bench Tested | Tested outside the console |
| Console Tested | Tested in a real PS2 |
| Stable | Repeatedly tested and behaving reliably |
| Product Candidate | Possible feature for a final FBD product |
| Deferred | Not planned for the current revision |
| Rejected | Reviewed or tested and not useful |

Using status labels will help avoid confusing early ideas with proven features.

---

## Related Project Folders

Other repo folders should be used for more specific project material.

| Folder | Purpose |
| --- | --- |
| `Hardware/` | PCB designs, schematics, KiCad files, board renders, and hardware notes |
| `Firmware/` | Source code, firmware notes, command protocols, and programming instructions |
| `Images/` | Logos, concept art, screenshots, board renders, and installation photos |
| `Manufacturing/` | BOMs, assembly notes, kit planning, QA checklists, and release notes |
| `Test-Data/` | UART logs, button tests, current measurements, heat tests, and validation data |
| `References/` | Datasheets, PS2 board notes, pinouts, related projects, and research links |

The `Documents/` folder should remain focused on planning, design direction, and written explanations.

---

## Important Notes

Button Butler is experimental and under active development.

The documents in this folder may describe features that are not built, tested, or finalized yet.

Prototype hardware should be tested carefully before being installed into a customer console.

Any feature that affects PS2 power, reset, modchip state, memory card behavior, or accessory power should be treated carefully and tested thoroughly.

---

## Documentation Goals

The long-term goal is for this folder to become a complete design reference for Button Butler.

Good documentation should make it easier to:

- Understand the project
- Build prototypes
- Compare design options
- Avoid repeating mistakes
- Create wiring diagrams
- Test features
- Build customer-safe installs
- Prepare final product versions
- Support future FBD builds

Documentation is part of the product.

---

## Summary

The `Documents/` folder is the main planning area for the FBD PS2 Button Butler project.

These files define what the project is, why it exists, how it may be installed, what features it may support, and how it could grow into a full PS2 mod management platform.

The first goal is a safe and reliable button interposer.

The long-term goal is a modular FBD control system for advanced PlayStation 2 builds.
