# References

## Overview

This folder contains reference material for the **FBD PS2 Button Butler** project.

The purpose of this folder is to collect datasheets, pinouts, PS2 board notes, research links, related project notes, and other external information that may help with hardware design, firmware development, installation planning, testing, and future expansion.

Button Butler is a work-in-progress PlayStation 2 mod platform, so good reference documentation is important.

---

## Purpose Of This Folder

The `References/` folder is used to keep supporting information organized.

This folder may include:

- Component datasheets
- MCU datasheets
- Logic IC datasheets
- Load switch datasheets
- Regulator datasheets
- PS2 board notes
- PS2 pinouts
- Button and lid switch notes
- UART notes
- Related project links
- Community research
- Forum notes
- GitHub links
- Vendor links
- Test references

Reference material should support the project without cluttering the main design folders.

---

## Current Status

| Area | Status |
| --- | --- |
| Datasheets | Planned |
| PS2 board notes | Planned |
| Pinouts | Planned |
| Research links | Planned |
| Related projects | Planned |
| Community notes | Planned |
| Verified references | Not yet organized |
| Final reference list | Not finalized |

This folder should be updated as parts are selected, measurements are made, and real prototypes are tested.

---

## Suggested Folder Structure

```text
References/
├── README.md
├── Datasheets/
│   └── README.md
├── PS2-Board-Notes/
│   └── README.md
├── Pinouts/
│   └── README.md
├── Research-Links.md
├── Related-Projects.md
└── Source-Links.md
```

This structure can be adjusted as the project grows.

---

## Reference Categories

| Category | Purpose |
| --- | --- |
| Datasheets | Official component datasheets and technical documents |
| PS2 Board Notes | Console-specific board observations and measurements |
| Pinouts | Connector, signal, and board pinout references |
| Research Links | Useful research links, forum posts, and documentation sources |
| Related Projects | Projects that connect to or influence Button Butler |
| Source Links | Original source links for hardware, firmware, and research references |

---

## Datasheets

The `Datasheets/` folder should contain or link to datasheets for parts used or considered in Button Butler.

Possible datasheet topics:

- PIC microcontrollers
- RP2040 or RP2350 controllers
- ESP32 modules
- Logic buffers
- Analog switches
- MOSFETs
- Transistors
- Load switch ICs
- Voltage regulators
- Level shifters
- UART muxes
- GPIO expanders
- Connectors
- Touchscreen modules
- Display controllers
- Touch controllers

Datasheets should be treated as the source of truth for electrical limits.

---

## Datasheet Rules

When adding datasheets:

- Use the official manufacturer datasheet when possible.
- Record the manufacturer name.
- Record the part number.
- Record the revision or date if known.
- Avoid relying only on reseller descriptions.
- Keep part names consistent with the BOM.
- Do not rename files in a confusing way.
- Add notes if a datasheet applies only to a possible part, not a selected final part.

A clear datasheet folder will make future board revisions easier.

---

## Suggested Datasheet File Naming

Suggested format:

```text
Manufacturer-PartNumber-Description.pdf
```

Examples:

```text
Microchip-PIC16F15214-Datasheet.pdf
TI-SN74LVC1G07-Open-Drain-Buffer.pdf
TI-SN74LVC1G66-Analog-Switch.pdf
Diodes-2N7002-MOSFET.pdf
Waveshare-Round-Touchscreen-Module.pdf
```

Use consistent names so datasheets are easy to find.

---

## PS2 Board Notes

The `PS2-Board-Notes/` folder should collect notes specific to PlayStation 2 models and motherboard revisions.

Possible notes:

- PS2 Slim board revisions
- PS2 Fat board revisions
- Power/reset button behavior
- Lid switch behavior
- Standby power rails
- Switched power rails
- Ground points
- Button board wiring
- RF shield clearance
- Internal fitment notes
- Model-specific install differences
- Known safe solder points
- Known risky solder points

These notes should be based on real measurements whenever possible.

---

## PS2 Models To Track

Button Butler may eventually need notes for several PS2 model families.

| Model Family | Notes |
| --- | --- |
| SCPH-300xx | PS2 Fat model family |
| SCPH-390xx | PS2 Fat model family |
| SCPH-500xx | PS2 Fat model family |
| SCPH-700xx | Early PS2 Slim family |
| SCPH-750xx | PS2 Slim family |
| SCPH-770xx | PS2 Slim family |
| SCPH-790xx | Late PS2 Slim family |
| SCPH-900xx | Final PS2 Slim family |
| FBD Ultra Slim | Custom FBD build style |
| Future FBD custom shells | Future build-specific notes |

Do not assume every model behaves the same.

---

## Board Note Requirements

Each PS2 board note should include:

- Console model
- Motherboard revision if known
- Region if relevant
- Test date
- Tester
- Measured signals
- Voltage readings
- Button behavior notes
- Lid switch notes
- Power rail notes
- Solder point photos if available
- Fitment notes
- Known risks
- Firmware or board revision used during testing

Good board notes help prevent repeat mistakes.

---

## Pinouts

The `Pinouts/` folder should contain connector and signal pinout references.

Possible pinouts:

- Button Interposer connector
- Touchscreen connector
- I/O Expansion connector
- Power Management connector
- UART connector
- Programming connector
- PIC ICSP header
- Lid switch connector
- PS2 button board signals
- BlueRetro control wiring
- Bluetooth audio control wiring
- ChipSlayer control wiring
- Video power control wiring
- Internal router control wiring

Pinouts should be clear, consistent, and revision-specific.

---

## Pinout Documentation Rules

Every pinout should include:

- Connector name
- Connector part number if known
- Pin count
- Pin 1 location
- Signal names
- Signal direction
- Voltage level
- Default state
- Active state
- Notes
- Board revision
- Date updated

Pinouts should not be vague.

A wrong pinout can damage hardware.

---

## Example Pinout Table Format

Use this format when documenting pinouts:

| Pin | Signal | Direction | Voltage | Default | Notes |
| --- | --- | --- | --- | --- | --- |
| 1 | `VCC` | Power | 3.3V or 5V | On | Logic power |
| 2 | `GND` | Power | 0V | Ground | Shared ground |
| 3 | `TX` | Output | Logic level | UART idle | Board transmit |
| 4 | `RX` | Input | Logic level | UART idle | Board receive |
| 5 | `OUT1` | Output | Logic level | Inactive | General output |
| 6 | `IN1` | Input | Logic level | Pulled defined | General input |

Update this format as real connector designs are created.

---

## Research Links

The `Research-Links.md` file should collect useful research references.

Possible links:

- PS2 hardware research
- Modchip documentation
- BlueRetro documentation
- SD2PSX documentation
- Mechacon / EE UART notes
- PS2 repair references
- HDMI mod references
- Power management references
- Touchscreen module documentation
- Community discussion threads
- GitHub repositories
- Forum posts

Research links should include short notes explaining why the link matters.

---

## Related Projects

The `Related-Projects.md` file should document projects that Button Butler may interact with or be inspired by.

Possible related projects:

- ChipSlayer
- PICfix / PICfix2 concepts
- BlueRetro
- SD2PSX
- MemCard Pro 2 / MMCE-related work
- RetroGEM
- ElectronAnalog
- LazyrSavre
- A5-V11 internal router work
- PowerOR
- PS2BBL
- OPL
- UDPBD / UDPFS
- MechaPwn research
- PMAP / optical drive service tools

Related project notes should explain the relationship to Button Butler.

---

## Source Links

The `Source-Links.md` file should collect original source links for important references.

Possible source link categories:

- Official datasheets
- Official GitHub repositories
- Official product pages
- Manufacturer documentation
- Community forum threads
- Discord discussion references if publicly shareable
- YouTube research videos
- Vendor product pages
- PS2 hardware notes

Source links help track where information came from.

---

## Link Documentation Format

When adding a link, use a simple format:

```markdown
## Reference Name

- Link:
- Type:
- Related area:
- Notes:
- Status:
```

Example:

```markdown
## Example Part Datasheet

- Link:
- Type: Datasheet
- Related area: Button Interposer hardware
- Notes: Used for output driver selection.
- Status: Candidate part
```

This keeps links useful instead of becoming a random bookmark list.

---

## Reference Status Labels

Use simple status labels for references.

| Status | Meaning |
| --- | --- |
| Unreviewed | Saved but not reviewed yet |
| Reviewed | Read and appears useful |
| Candidate | May apply to selected hardware |
| Selected | Applies to a chosen part or design |
| Tested | Confirmed through bench or console testing |
| Deprecated | No longer preferred |
| Rejected | Reviewed and not useful or incorrect |
| Historical | Useful for background but not current design |

Status labels help separate random research from proven design references.

---

## Verified Versus Unverified Information

References should clearly separate verified and unverified information.

Verified information may include:

- Official datasheet limits
- Direct measurements
- Tested pinouts
- Confirmed board behavior
- Bench test data
- Console test results

Unverified information may include:

- Forum claims
- Discord comments
- Vendor descriptions
- AI-generated notes
- Assumptions
- Untested diagrams
- Early guesses

Unverified information can still be useful, but it should be labeled clearly.

---

## Measurement Notes

Real measurements should be saved and linked from reference docs.

Useful measurements:

- Button line voltage
- Lid switch voltage
- Standby rail voltage
- Switched rail voltage
- Accessory current draw
- UART voltage level
- Output default state
- Power-good behavior
- Backfeeding voltage
- Heat readings

Measurements should include the console model and board revision.

---

## Image References

Reference images may include:

- PS2 motherboard photos
- Solder point photos
- Pinout diagrams
- Board revision photos
- Connector orientation photos
- Touchscreen mounting photos
- Power measurement setup photos
- Logic analyzer captures
- Oscilloscope captures

Suggested location:

```text
References/Images/
```

or a project-specific image folder if the image belongs with hardware or test data.

---

## Safety-Critical References

Some references are safety-critical and should be treated carefully.

Safety-critical areas:

- PS2 power/reset behavior
- PS2 reset line behavior
- Modchip enable/disable behavior
- Accessory power switching
- Video board power control
- SD2PSX or memory-card-related behavior
- UART voltage compatibility
- Backfeeding prevention
- Load switch current limits
- Regulator thermal limits

Do not rely on unverified references for safety-critical decisions.

---

## Component Selection References

When selecting components, save references for:

- Electrical ratings
- Package size
- Pinout
- Logic thresholds
- Current rating
- Voltage rating
- Thermal behavior
- Startup behavior
- Power-off behavior
- Availability
- Substitute parts

Component references should connect back to the BOM and schematic.

---

## Firmware References

Firmware-related references may include:

- MCU datasheets
- UART protocol references
- Compiler documentation
- Programming tool documentation
- Bootloader notes
- Display driver documentation
- Touch controller documentation
- GPIO expander documentation
- EEPROM or flash storage notes

Firmware references should be placed in this folder if they support the project generally.

Detailed firmware notes should live in `Firmware/`.

---

## Hardware References

Hardware-related references may include:

- Logic IC datasheets
- MOSFET datasheets
- Regulator datasheets
- Load switch datasheets
- Connector datasheets
- PCB manufacturer rules
- PS2 board notes
- Power measurement notes
- Backfeeding protection notes
- ESD protection notes

Detailed board design files should live in `Hardware/`.

---

## Manufacturing References

Manufacturing-related references may include:

- PCB manufacturer capabilities
- Assembly guidelines
- Soldering notes
- Connector crimping notes
- Test jig notes
- QA process notes
- Packaging supplier notes
- Label supplier notes

Detailed manufacturing files should live in `Manufacturing/`.

---

## Documentation Rules

Reference documentation should be:

- Clear
- Source-linked where possible
- Labeled as tested or untested
- Organized by topic
- Updated when parts change
- Not mixed with unrelated notes
- Not treated as final unless verified

The goal is to make future design decisions easier.

---

## Do Not Store

Avoid storing the following in this folder:

- Large unrelated files
- Random screenshots without notes
- Duplicate datasheets with unclear names
- Unsupported claims without labels
- Private customer information
- Files that belong in `Hardware/`, `Firmware/`, `Manufacturing/`, or `Test-Data/`
- Copyrighted material that should not be redistributed

Keep the folder useful and clean.

---

## Suggested Reference Workflow

A good workflow:

1. Find a useful source.
2. Save the link or datasheet.
3. Add a short note explaining why it matters.
4. Mark the status as unreviewed, reviewed, candidate, selected, or tested.
5. Link it to the related board, firmware, or test file.
6. Update the status after testing.
7. Remove or mark deprecated references when they are no longer useful.

This keeps the reference folder from becoming a junk drawer.

---

## Open Questions

Open reference questions:

- Which PS2 models should be measured first?
- Which button board signals need model-specific notes?
- Which MCU datasheets belong in the first reference set?
- Which connector family should be documented first?
- Should datasheets be stored directly or linked only?
- Should vendor product pages be included?
- Should Discord/community findings be summarized separately?
- Should every pinout get its own file?
- Should reference images live here or under each hardware folder?
- What references are required before the first PCB prototype?

These questions should be answered as the repo develops.

---

## Recommended First References To Add

Recommended first items:

- Selected PIC or MCU datasheet
- Button Interposer signal measurements
- PS2 Slim button/lid switch notes
- UART logic-level notes
- Programming header / ICSP notes
- Output driver candidate datasheets
- Load switch candidate datasheets
- Touchscreen module documentation
- Connector datasheets
- PCB manufacturer design rules
- ChipSlayer relationship notes
- BlueRetro and SD2PSX links if used in early planning

Start with references that support the first prototype.

---

## Summary

The `References/` folder is the supporting research library for the FBD PS2 Button Butler project.

It should collect datasheets, pinouts, PS2 board notes, research links, related projects, and source links.

References should be organized, labeled, and updated as the project moves from concept to tested hardware.

The most important rule is simple:

**Do not treat a reference as proven until it is verified through official documentation, direct measurement, or real hardware testing.**
