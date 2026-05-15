# 09 - Future Expansion

## Overview

The **FBD PS2 Button Butler** project is being designed as a modular platform, not just a single-purpose board.

The first goal is to prove the basic power/reset button interposer concept. After that works reliably, the project can expand into touchscreen control, I/O expansion, power management, mod control, diagnostics, and build-specific profiles.

This document collects future expansion ideas so they can be tracked without overloading the first hardware revision.

---

## Main Expansion Philosophy

Button Butler should grow in stages.

The project should not try to do everything in the first prototype.

Recommended philosophy:

1. Prove the base button interposer first.
2. Add simple I/O control second.
3. Add touchscreen communication third.
4. Add advanced mod control after the core system is stable.
5. Add diagnostics and automation last.

The long-term goal is a powerful FBD control platform, but the first goal is a safe and reliable button interposer.

---

## Expansion Categories

Future expansion ideas can be grouped into several categories.

| Category | Purpose |
| --- | --- |
| Hardware Expansion | Additional boards, connectors, outputs, and power control |
| Firmware Expansion | Profiles, command handling, safety logic, and stored settings |
| Touchscreen Expansion | Pages, themes, menus, and user controls |
| Mod Integration | BlueRetro, SD2PSX, ChipSlayer, modchips, Bluetooth audio, and video boards |
| Power Management | Accessory power control, sleep modes, and heat reduction |
| Diagnostic Expansion | Service pages, UART logs, laser feedback, and system status |
| Manufacturing Expansion | Kits, BOMs, QA checklists, and install guides |
| Product Expansion | Future FBD product versions and customer-facing options |

---

## Hardware Expansion

Future hardware revisions may include more than one board.

Possible hardware blocks:

- Button Interposer Board
- I/O Expansion Board
- Touchscreen Hub
- Power Management Board
- RGB or lighting board
- Chime or buzzer board
- Service/debug adapter
- Model-specific harnesses
- Flex cable interposers
- Future diagnostic input board

Each hardware block should solve a specific problem.

The project should avoid making one huge board that is difficult to install in different PS2 models.

---

## Button Interposer Board Expansion

The Button Interposer Board is the core of the system.

Possible future additions:

- More digital outputs
- More digital inputs
- Better fail-safe pass-through behavior
- Dedicated lid switch input
- Dedicated service jumper input
- Hardware watchdog
- Power-good input
- Onboard status LED
- Better ESD protection
- Smaller board layout
- Model-specific versions
- Cleaner connector system
- Improved programming pads

The base board should stay focused on safe button behavior.

---

## I/O Expansion Board Expansion

An I/O Expansion Board can provide extra control points without making the base board too large.

Possible future I/O expansion features:

- Additional digital outputs
- Additional digital inputs
- Open-drain outputs
- MOSFET-switched outputs
- LED outputs
- RGB control output
- Buzzer output
- Accessory enable outputs
- Service input pins
- Expansion connector
- UART or I2C control

This board can be optional for advanced builds.

---

## Touchscreen Hub Expansion

The touchscreen hub is the premium user interface layer.

Possible future touchscreen hub features:

- Multiple UART ports
- Dedicated interposer UART
- BlueRetro UART or status link
- SD2PSX UART or status link
- Muxed service UART
- Touchscreen sleep/wake control
- Display brightness control
- Build-specific splash screens
- Theme support
- Service page access
- Firmware update support

The touchscreen should remain optional.

The console should still work if the touchscreen is not installed.

---

## Power Management Board Expansion

A dedicated power management board may be useful for advanced builds.

Possible features:

- Accessory power switching
- Regulator enable control
- Load switch ICs
- Power-good monitoring
- Current monitoring
- Thermal monitoring
- Video board power control
- Internal router power control
- Touchscreen backlight control
- RGB power control
- Manual override input
- Safe startup sequencing

Power management should be added carefully because it can affect reliability.

---

## RGB And Lighting Expansion

Button Butler may eventually control decorative or status lighting.

Possible RGB expansion features:

- Simple on/off lighting
- PWM brightness control
- RGB preset selection
- Addressable LED support
- Status lighting
- Error lighting
- Service mode lighting
- Build-specific lighting themes
- Touchscreen lighting page
- Automatic light sleep mode

Lighting should remain secondary to console reliability.

---

## Chime And Audio Feedback Expansion

Button Butler may support sound feedback.

Possible chime features:

- Startup chime
- Shutdown chime
- Button accepted tone
- Button rejected tone
- Service mode tone
- Bluetooth pairing tone
- Error tone
- Custom build-specific chime
- Touchscreen sound setting
- Mute option

Chime feedback should be optional.

Not every build needs sound.

---

## Firmware Expansion

Firmware can grow as the hardware becomes stable.

Possible firmware expansion features:

- Button behavior profiles
- Stored settings
- UART command parser
- Touchscreen command handling
- Output state memory
- Service mode
- Safety lockouts
- Watchdog support
- Firmware version reporting
- Hardware revision detection
- Build profile selection
- Event logging
- Error reporting
- Factory reset option

The first firmware should remain simple.

Start with button input, pass-through behavior, lid switch input, one output, and UART debug.

---

## Button Behavior Profile Expansion

Future firmware may support multiple button profiles.

Possible profiles:

- Basic profile
- ChipSlayer profile
- Modchip profile
- BlueRetro profile
- Bluetooth audio profile
- Premium touchscreen profile
- Service/testing profile
- Customer-safe profile
- Installer profile

Profiles allow the same hardware to behave differently depending on the build.

---

## Stored Settings Expansion

Button Butler may eventually store settings.

Possible stored settings:

- Active profile
- Output default states
- Button timing values
- Touchscreen brightness
- Chime enable
- RGB state
- Service lock state
- Last ChipSlayer state
- Last modchip state
- Auto power management setting
- Build ID or configuration value

Stored settings should be used carefully.

Bad stored settings should not make the console unusable.

---

## UART Protocol Expansion

The UART protocol can expand over time.

Possible future protocol features:

- Device addressing
- Command acknowledgements
- Error responses
- State reporting
- Event reporting
- Profile selection
- Firmware version query
- Output test commands
- Service unlock command
- Touchscreen page updates
- Device heartbeat
- Safe mode command

The protocol should remain easy to debug.

Readable commands are useful during early development.

---

## I/O Command Expansion

Future I/O commands may include:

| Command Idea | Purpose |
| --- | --- |
| Output on | Turn a mapped output on |
| Output off | Turn a mapped output off |
| Output toggle | Toggle a mapped output |
| Output pulse | Momentarily activate an output |
| Read input | Read one input state |
| Read all | Read all known inputs and outputs |
| Set profile | Change behavior profile |
| Save settings | Save current configuration |
| Restore defaults | Return to safe default settings |

These are concept ideas only.

The exact command format should be defined later.

---

## Touchscreen UI Expansion

The touchscreen can expand into a full control interface.

Possible future touchscreen pages:

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
- Router page
- Service page
- Diagnostic page
- Settings page
- About page

Unused pages should be hidden based on the build profile.

---

## Touchscreen Theme Expansion

Future touchscreen themes could match specific FBD builds.

Possible themes:

- FBD default
- Blue Steel
- Ultra Slim
- Steampunk
- Sly Cooper inspired
- God of War inspired
- Minimal dark
- Service/testing
- Build-specific customer theme

Themes should not be the first priority.

The first priority is reliable control and clear status.

---

## Touchscreen Branding Expansion

Premium builds could show build-specific branding.

Possible branding items:

- FBD logo
- Button Butler logo
- Build name
- Build ID
- Console model
- Hardware revision
- Firmware version
- Customer build profile
- Custom splash screen

This can help Button Butler feel like a finished FBD product.

---

## BlueRetro Expansion

BlueRetro support can grow over time.

Possible future BlueRetro features:

- Enable BlueRetro
- Disable BlueRetro
- Reset BlueRetro
- Disable controller channel 1
- Disable controller channel 2
- Show controller connection state
- Show pairing status
- Trigger pairing if supported
- Show firmware information if available
- Touchscreen controller page
- Service reset option

Early support may only use simple digital control.

Advanced communication can be added later.

---

## SD2PSX Expansion

SD2PSX support should be added carefully.

Possible future SD2PSX features:

- Display SD2PSX detected status
- Display current Game ID if available
- Display active card/profile if available
- Display warning or error state
- Display firmware version if available
- Show save activity warning if available
- Provide limited supported controls
- Touchscreen SD2PSX page

Button Butler should avoid risky SD2PSX power or mode changes until behavior is fully understood.

Memory card behavior must stay reliable.

---

## ChipSlayer Expansion

ChipSlayer is a strong candidate for early integration.

Possible future ChipSlayer features:

- Enable ChipSlayer
- Disable ChipSlayer
- Toggle ChipSlayer
- Show ChipSlayer state
- Button shortcut control
- Touchscreen control
- Service mode testing
- LED status
- Build profile default state
- State memory if needed

Button Butler can make ChipSlayer easier to use without adding a separate external switch.

---

## Modchip Control Expansion

Modchip control is useful but should be handled carefully.

Possible future modchip features:

- Enable modchip
- Disable modchip
- Disable modchip for next boot
- Enable modchip for next boot
- Show modchip state
- Touchscreen control
- Hidden button shortcut
- Service mode override
- Build profile default state

Modchip state can affect boot behavior, so it should have clear defaults and confirmation steps.

---

## Bluetooth Audio Expansion

Bluetooth audio control can remove the need for extra pairing buttons.

Possible future Bluetooth audio features:

- Enable audio board
- Disable audio board
- Trigger pairing mode
- Reset audio board
- Show pairing state if available
- Show connected state if available
- Touchscreen audio page
- Button shortcut for pairing
- Auto-disable when idle
- Chime feedback during pairing

Bluetooth audio should be tested for noise, reconnect behavior, and power switching behavior.

---

## RetroGEM And ElectronAnalog Expansion

Button Butler may eventually manage power for video boards.

Possible future video board features:

- Enable video board power
- Disable video board power
- Show video power state
- Manual touchscreen override
- Auto power saving mode
- Power-good detection
- Heat reduction mode
- Service test mode

This should be tested carefully because video loss can confuse the user.

The touchscreen should clearly show when video power is disabled.

---

## Internal Router Expansion

Some FBD PS2 builds may use an internal router or network device.

Possible future router features:

- Power router on
- Power router off
- Restart router
- Wait for router ready signal
- Show router ready state
- Show IP or network mode if available
- Show USB storage mounted state if available
- Service UART access
- Startup delay coordination

Router power control is useful when the PS2 depends on network loading.

---

## LazyrSavre / Laser Feedback Expansion

Future versions may display information from LazyrSavre-style hardware or other laser feedback systems.

Possible future laser feedback features:

- Laser activity display
- Servo activity display
- Protection status
- Warning state
- Fault history
- Diagnostic values
- Health indicator
- Service recommendation
- Touchscreen laser feedback page

This should remain a future diagnostic feature until the base system is stable.

---

## Mechacon And Emotion Engine UART Expansion

Advanced builds may eventually expose or monitor Mechacon and Emotion Engine UARTs.

Possible future uses:

- Service access
- Diagnostic logs
- PMAP-related information
- Development testing
- Console status research
- Troubleshooting
- Experimental service pages

These UARTs should not be treated as required for the first version.

If used, they may be better connected through a mux or service-only connector.

---

## A5-V11 / Internal Network Device Expansion

Button Butler may eventually interface with an internal A5-V11 or similar network device.

Possible future features:

- Router power control
- Router reset
- Router ready input
- UART service access
- Touchscreen network status page
- Startup coordination before console boot
- Service mode router restart
- Network loading status if available

This can tie into FBD internal router and UDPFS/SMB-related builds.

---

## Service Mode Expansion

Service mode can become a powerful installer tool.

Possible future service mode features:

- Read all inputs
- Test all outputs
- Show button timing
- Show lid switch state
- Show UART status
- Show touchscreen connection
- Test ChipSlayer output
- Test modchip output
- Test Bluetooth pairing trigger
- Test BlueRetro reset
- Test RGB lighting
- Test video power output
- Show power-good states
- Reset settings
- Show firmware version

Service mode should be protected from accidental access.

---

## Diagnostic Expansion

Diagnostics can help troubleshoot complex builds.

Possible diagnostic features:

- Input state display
- Output state display
- UART event log
- Last command received
- Last command accepted
- Last command rejected
- Error flags
- Power state
- Lid switch state
- Button state
- Accessory status
- Firmware uptime
- Watchdog reset count
- Fault history

Diagnostics are useful for development and service, but should not overwhelm normal users.

---

## Logging Expansion

Button Butler may eventually support simple logging.

Possible logs:

- Button events
- Lid switch changes
- Output changes
- Touchscreen commands
- Rejected commands
- Service mode entry
- Power state changes
- Error events
- Watchdog resets
- Accessory faults

Logging may be useful over UART first.

Persistent logging can be considered later if needed.

---

## Build Profile Expansion

Build profiles can make Button Butler easier to reuse.

Possible build profiles:

| Profile | Main Focus |
| --- | --- |
| Basic | Simple button pass-through and debug |
| ChipSlayer | ChipSlayer control from button or touchscreen |
| Modchip | Modchip enable/disable behavior |
| BlueRetro | BlueRetro enable/reset/channel control |
| Bluetooth Audio | Audio enable and pairing control |
| SD2PSX | Status display and conservative control |
| Internal Router | Router power and startup coordination |
| Premium Ultra Slim | Full touchscreen mod management |
| Service/Test | Input/output testing and diagnostics |

Profiles should hide unused features and keep the interface clean.

---

## Manufacturing Expansion

Future manufacturing documentation may include:

- Final BOM
- Assembly instructions
- PCB revision notes
- Firmware flashing instructions
- Test jig notes
- QA checklist
- Packaging notes
- Kit contents
- Label files
- Customer operation guide
- Troubleshooting guide

This should be added after hardware becomes stable.

---

## Kit Expansion

Button Butler could eventually become a kit.

Possible kit versions:

| Kit | Included Parts |
| --- | --- |
| Basic Interposer Kit | Button Interposer Board and wiring |
| ChipSlayer Control Kit | Interposer plus ChipSlayer control wiring |
| I/O Expansion Kit | I/O Expansion Board and harness |
| Touchscreen Kit | Touchscreen module, interposer, and wiring |
| Premium Build Kit | Touchscreen, interposer, I/O, power management, and harnesses |
| Service Kit | Debug adapter, test harness, and documentation |

Kit versions should only be created after repeatable installs are proven.

---

## Documentation Expansion

Future documentation may include:

- Wiring diagrams
- Model-specific install guides
- Console revision notes
- Touchscreen mounting guide
- Button behavior guide
- Firmware flashing guide
- UART protocol guide
- Troubleshooting guide
- Customer operation guide
- QA checklist
- Build profile reference
- Safety notes

Good documentation is part of the product.

---

## Model-Specific Expansion

Different PS2 models may need different install notes.

Possible model-specific docs:

- PS2 Slim SCPH-700xx
- PS2 Slim SCPH-750xx
- PS2 Slim SCPH-770xx
- PS2 Slim SCPH-790xx
- PS2 Slim SCPH-900xx
- PS2 Fat SCPH-300xx
- PS2 Fat SCPH-390xx
- PS2 Fat SCPH-500xx
- FBD Ultra Slim builds
- FBD custom shell builds

Model-specific support should be added as real installs are tested.

---

## Flex Cable Expansion

Future versions may use flex cables or interposer flexes.

Possible flex cable uses:

- Power/reset button interposer
- Lid switch connection
- Controller port interposer
- Memory card slot related signals
- Touchscreen connection
- Low-profile internal routing
- Model-specific harnessing

Flex cables can make installs cleaner, but they should be tested carefully for reliability and manufacturability.

---

## Custom Harness Expansion

Custom harnesses may make Button Butler easier to install repeatedly.

Possible harness features:

- Pre-crimped wires
- Color-coded signals
- Keyed connectors
- Labeled ends
- Model-specific lengths
- Service connector branch
- Touchscreen branch
- I/O expansion branch

Harnesses can reduce wiring mistakes in final product versions.

---

## Test Jig Expansion

A test jig could help validate boards before installation.

Possible test jig functions:

- Power the board safely
- Simulate button press
- Simulate lid switch
- Read outputs
- Test UART
- Test touchscreen communication
- Test LEDs
- Test power enable lines
- Run firmware self-test
- Confirm board revision

A test jig would be useful if Button Butler becomes a product.

---

## Customer-Facing Expansion

Future customer-facing features may include:

- Simple operation guide
- Touchscreen menu guide
- Button shortcut guide
- Build feature list
- Support page
- QR code to documentation
- Build ID lookup
- Firmware version display
- Safety notes
- Troubleshooting basics

The customer should not need to understand the engineering details to use the console.

---

## Product Version Expansion

Button Butler may eventually become a family of FBD products.

Possible product versions:

| Version | Description |
| --- | --- |
| Button Butler Basic | Power/reset button interposer only |
| Button Butler I/O | Interposer plus extra outputs |
| Button Butler Touch | Touchscreen control version |
| Button Butler Premium | Full mod control and touchscreen system |
| Button Butler Dev | Development/testing version with headers |
| Button Butler Flex | Flex-based install version |
| Button Butler Service Tool | External or internal service/debug accessory |

Product names can be refined later.

---

## Naming Expansion

Possible naming ideas:

- Button Butler
- Button Butler Basic
- Button Butler Touch
- Button Butler Pro
- Button Butler Premium
- Button Butler I/O
- Button Butler Hub
- Button Butler Service
- Button Butler Flex
- FBD Control Hub

The main project name should stay consistent.

---

## Possible Future Logos And Branding

Future branding ideas:

- FBD logo
- Button Butler logo
- Small butler-themed icon
- Power button icon
- Round touchscreen splash screen
- PCB silkscreen logo
- Product label
- Build certificate logo
- GitHub repo banner

Branding should support the project without making the hardware harder to read or install.

---

## Expansion Safety Rules

Future expansion should follow safety rules.

Recommended rules:

- Do not add features that make the console less reliable.
- Do not make the touchscreen required for basic console operation.
- Do not allow cosmetic features to interfere with power/reset behavior.
- Do not power-cycle memory-related devices during active use.
- Do not switch unknown loads without testing.
- Do not expose dangerous service actions to normal users.
- Do not allow outputs to float.
- Do not create backfeeding paths.
- Do not make firmware too complicated before hardware is proven.

Reliability comes first.

---

## Expansion Priority

Not all future ideas have the same priority.

Suggested priority:

| Priority | Expansion |
| --- | --- |
| High | Button interposer reliability |
| High | Safe output control |
| High | UART debug/status |
| High | ChipSlayer control |
| Medium | Bluetooth audio pairing |
| Medium | Touchscreen basic control |
| Medium | RGB enable |
| Medium | Modchip control |
| Medium | Video board power management |
| Low | Full BlueRetro status |
| Low | Full SD2PSX status |
| Low | Internal router status |
| Low | Laser feedback display |
| Low | Advanced diagnostics |
| Low | Themes and animations |

This priority can change as the project develops.

---

## Recommended Expansion Roadmap

A practical roadmap:

1. Base Button Interposer Board
2. Button pass-through and button detection
3. Lid switch detection
4. One safe output
5. UART debug messages
6. ChipSlayer control
7. I/O Expansion Board concept
8. Touchscreen basic communication
9. Touchscreen main page
10. Bluetooth audio pairing control
11. RGB lighting control
12. Modchip control
13. Video board power management
14. BlueRetro control
15. SD2PSX status page
16. Service and diagnostics
17. Product documentation and kit planning

This keeps the project moving without jumping too far ahead.

---

## Minimum Viable Expansion

The first meaningful expansion after the base interposer should be simple.

Minimum viable expansion:

- One lid-switch modifier
- One safe output
- One UART debug/status line
- One controlled mod, such as ChipSlayer
- One documented behavior profile

This proves that Button Butler can do more than pass-through button behavior.

---

## Advanced Expansion Vision

The advanced version of Button Butler could become a full internal control system.

Possible advanced system:

- Touchscreen on the console
- Button Interposer Board
- I/O Expansion Board
- Power Management Board
- BlueRetro control
- Bluetooth audio control
- SD2PSX status
- ChipSlayer control
- Modchip control
- RGB lighting
- Video board power management
- Router startup coordination
- Laser feedback display
- Service diagnostics
- Build-specific profiles

This is the long-term vision.

---

## Open Questions

Open questions for future expansion:

- Which expansion feature should come first after the base board?
- Should ChipSlayer control be the first real use case?
- Should the I/O Expansion Board use UART, I2C, or direct GPIO?
- Should the touchscreen controller act as the UART hub?
- Which touchscreen hardware should become the standard?
- Should profiles be firmware-based or user-selectable?
- Should the system store settings?
- Should customer builds allow service mode access?
- How much of SD2PSX and BlueRetro can be safely controlled?
- Should video board power management be manual or automatic?
- What is the best way to prevent backfeeding between boards?
- Should final kits include harnesses?

These questions should be answered through prototype testing.

---

## Risks

Possible risks from future expansion:

- Feature creep
- Overly complex firmware
- Too many boards
- Too many wires
- Touchscreen dependency
- Customer confusion
- Unsafe power behavior
- Backfeeding
- Signal contention
- Inconsistent behavior between PS2 models
- Hard-to-service installs
- Difficult product support
- Poor documentation

The project should expand only when each stage is stable.

---

## Development Notes

Future ideas should be documented even if they are not implemented right away.

A good process:

1. Record the idea.
2. Mark it as concept, planned, testing, or proven.
3. Define what hardware it needs.
4. Define what firmware it needs.
5. Define safety concerns.
6. Test it on the bench.
7. Test it in a console.
8. Document the result.
9. Decide if it belongs in the final product.

This keeps the project organized as it grows.

---

## Status Labels

Future expansion ideas can use simple status labels.

| Status | Meaning |
| --- | --- |
| Concept | Idea only, not tested |
| Planned | Intended for future testing |
| Prototype | Hardware or firmware test in progress |
| Bench Tested | Tested outside a console |
| Console Tested | Tested in a real PS2 |
| Stable | Works reliably in repeated testing |
| Product Candidate | Possible final product feature |
| Deferred | Not planned for current revision |
| Rejected | Tested or reviewed and not useful |

These labels can help keep the repo clear.

---

## Summary

Button Butler has a lot of future expansion potential.

The project can grow from a simple power/reset button interposer into a full PS2 mod management platform with touchscreen control, I/O expansion, power management, diagnostics, and build-specific profiles.

The first version should stay focused and reliable.

Future expansion should be added in stages, with each feature tested and documented before it becomes part of a customer-ready FBD product.
