# Manufacturing

## Overview

This folder contains manufacturing, assembly, QA, kit planning, and release-preparation documentation for the **FBD PS2 Button Butler** project.

Button Butler is currently a work-in-progress PS2 mod platform. The goal of this folder is to keep the production-side notes organized as the project moves from concept, to prototype, to tested hardware, and eventually to possible FBD product versions.

This folder should only contain manufacturing and release-related material.

Design theory, firmware planning, hardware architecture, and general project explanations should live in their own folders.

---

## Purpose Of This Folder

The `Manufacturing/` folder is used to document how Button Butler hardware should eventually be:

- Assembled
- Inspected
- Programmed
- Tested
- Packaged
- Labeled
- Installed
- Supported
- Prepared for possible kit or product release

This folder helps turn the project from a one-off prototype into something repeatable.

---

## Current Status

| Area | Status |
| --- | --- |
| Manufacturing process | Concept planning |
| Board assembly process | Not finalized |
| Firmware flashing process | Not finalized |
| QA checklist | Draft created |
| Kit planning | Future concept |
| Packaging | Future concept |
| Labels | Future concept |
| Release process | Future concept |
| Customer documentation | Future concept |
| Product-ready status | Not ready |

Button Butler is not customer-ready yet.

Manufacturing documentation should be updated as real prototypes are built and tested.

---

## Manufacturing Philosophy

Button Butler should be designed and documented like a real FBD product.

The manufacturing process should focus on:

1. Repeatability
2. Safety
3. Clear labeling
4. Clean assembly
5. Correct firmware matching
6. Reliable testing
7. Useful QA records
8. Serviceability
9. Customer-safe behavior
10. Honest documentation

The goal is not just to make one board work.

The goal is to create a process that can be repeated without relying on memory or guesswork.

---

## Folder Index

| File / Folder | Purpose |
| --- | --- |
| `README.md` | Overview of the manufacturing folder |
| `QA-Checklist.md` | Main quality assurance checklist for boards, firmware, installs, and customer builds |
| `Assembly-Instructions/` | Future board assembly and wiring instructions |
| `Kit-Packaging/` | Future kit contents, packaging notes, and packing checklists |
| `Labels/` | Future board labels, product labels, warning labels, and revision labels |
| `Etsy-eBay-Website-Copy/` | Future customer-facing product copy and listing text |
| `Release-Notes/` | Future release notes for hardware and firmware versions |
| `Test-Jigs/` | Future test fixture notes and test jig documentation |

Some folders may remain empty until the hardware becomes real.

---

## Suggested Folder Structure

Suggested manufacturing folder structure:

```text
Manufacturing/
├── README.md
├── QA-Checklist.md
├── Assembly-Instructions/
│   └── README.md
├── Kit-Packaging/
│   └── README.md
├── Labels/
│   └── README.md
├── Etsy-eBay-Website-Copy/
│   └── README.md
├── Release-Notes/
│   └── README.md
└── Test-Jigs/
    └── README.md
```

This structure can be adjusted as the project develops.

---

## Manufacturing Goals

The long-term manufacturing goals are:

- Build Button Butler boards consistently
- Match each board to the correct firmware
- Verify every board before console installation
- Verify every installed console before customer use
- Keep revision history clear
- Keep test results organized
- Make installation repeatable
- Make troubleshooting easier
- Prepare possible kit versions
- Protect the FBD brand by avoiding rushed or untested releases

A board should not be treated as product-ready just because it works once.

---

## Board Types

Button Butler may eventually include several board types.

| Board | Manufacturing Notes |
| --- | --- |
| Button Interposer Board | Core board, highest safety priority |
| I/O Expansion Board | Optional expansion board for extra inputs and outputs |
| Touchscreen Hub | Optional premium user interface hardware |
| Power Management Board | Optional accessory power control board |
| Touchscreen Adapter Board | Possible future board for clean touchscreen wiring |
| Service Adapter | Possible future board for testing and programming |
| Test Jig | Possible future board or fixture for QA testing |

Each board should have its own assembly notes, BOM, firmware target, and QA procedure.

---

## Prototype Versus Product

Button Butler should clearly separate prototype hardware from product-ready hardware.

| Stage | Meaning |
| --- | --- |
| Concept | Idea only, not built |
| Prototype | Built for testing and development |
| Bench Tested | Tested outside a console |
| Console Tested | Tested in a real PS2 |
| Installer Tested | Installed and tested repeatedly |
| Product Candidate | Possible final version after repeated testing |
| Released | Approved for customer or kit use |

Do not mix these stages.

Prototype hardware should be labeled clearly.

---

## Revision Tracking

Every board revision should be tracked.

Record:

- Board name
- Board revision
- Date ordered
- Date received
- PCB manufacturer
- Board thickness
- Copper weight if important
- Assembly method
- Firmware target
- Known issues
- Test status
- Supported console models
- Unsupported console models

Revision tracking prevents old mistakes from coming back later.

---

## Suggested Revision Labels

Suggested revision labels:

| Revision | Meaning |
| --- | --- |
| `v0.0` | Concept only |
| `v0.1` | First schematic prototype |
| `v0.2` | First PCB prototype |
| `v0.3` | Bench-tested revision |
| `v0.4` | Console-tested revision |
| `v0.5` | Improved prototype |
| `v1.0` | First stable release candidate |

Do not mark a revision as stable until it has been tested in real hardware.

---

## Assembly Documentation

Assembly documentation should explain how each board is built.

Assembly notes should include:

- Board revision
- BOM version
- Required tools
- Required parts
- Component placement notes
- Component orientation notes
- Soldering notes
- Inspection notes
- Programming notes
- Known assembly issues
- Rework notes
- Photos or diagrams

The goal is to make assembly repeatable.

---

## BOM Requirements

Each board should have a clear BOM.

A good BOM should include:

- Reference designator
- Quantity
- Component value
- Component package
- Manufacturer part number
- Supplier part number
- Voltage rating
- Current rating if relevant
- Power rating if relevant
- Tolerance
- Notes
- Substitute part options

The BOM should avoid vague entries where possible.

---

## Firmware Matching

Each programmed board must match the correct firmware.

Record:

- Board name
- Board revision
- MCU part number
- Firmware file name
- Firmware version
- Firmware build date
- Active profile
- UART settings
- Known limitations
- Programmer used
- Verification result

Wrong firmware on the wrong board revision can cause unsafe behavior.

---

## Firmware File Naming

Suggested firmware naming format:

```text
button-butler-[board-name]-[version]-[purpose].[ext]
```

Examples:

```text
button-butler-pic-interposer-v0.1.0-button-test.hex
button-butler-pic-interposer-v0.3.0-console-test.hex
button-butler-touchscreen-v0.2.0-uart-test.uf2
button-butler-io-expansion-v0.1.0-output-test.hex
```

Firmware names should make the board target and purpose obvious.

---

## QA Process

Every board and every installed build should go through QA.

The main QA document is:

```text
Manufacturing/QA-Checklist.md
```

QA should include:

- Visual inspection
- Continuity checks
- Power checks
- Firmware verification
- UART testing
- Button behavior testing
- Output default testing
- Backfeeding checks
- Console testing
- Heat testing if needed
- Final assembly inspection
- Customer documentation check

Safety-critical failures must be fixed before a board is used in a real console.

---

## Required QA Records

Each tested board or install should record:

- Build ID
- Board revision
- Firmware version
- Date tested
- Tester
- Test status
- Failed items
- Corrective action
- Retest result
- Console model tested if applicable
- Final approval status

QA records are important if Button Butler becomes a product.

---

## Test Data Links

Test results should be saved in the `Test-Data/` folder.

Suggested locations:

| Test Data Folder | Purpose |
| --- | --- |
| `Test-Data/UART-Logs/` | UART boot logs, command logs, and error logs |
| `Test-Data/Button-Tests/` | Button timing and behavior tests |
| `Test-Data/Power-Measurements/` | Voltage, current, and power data |
| `Test-Data/Heat-Testing/` | Temperature and runtime data |
| `Test-Data/Console-Tests/` | Real-console testing notes |
| `Test-Data/Failure-Reports/` | Problems, failures, and fixes |

Manufacturing notes should link to test data when useful.

---

## Test Jig Planning

A test jig may be useful if Button Butler becomes repeatable.

A test jig could help verify:

- Power input
- Current draw
- Button input
- Lid switch input
- UART communication
- Output states
- Output toggling
- Service mode
- Safe mode
- Firmware version
- Programming connection
- Touchscreen communication

A test jig can reduce mistakes and speed up QA.

---

## Possible Test Jig Features

Possible test jig features:

- Simulated button press
- Simulated lid open / closed
- LEDs for each output
- UART connection to PC
- Programming connector
- Current measurement points
- Power switch
- Safe dummy loads
- Touchscreen connector
- Pass / fail markings
- Printed test steps

A test jig should be simple and reliable.

---

## Kit Planning

Button Butler may eventually become a kit or set of kit options.

Possible kit versions:

| Kit | Possible Contents |
| --- | --- |
| Basic Interposer Kit | Button Interposer Board, wires, basic instructions |
| ChipSlayer Control Kit | Interposer, ChipSlayer wiring, install notes |
| I/O Expansion Kit | I/O Expansion Board, harness, output notes |
| Touchscreen Kit | Touchscreen, adapter, harness, mounting parts |
| Premium Kit | Interposer, touchscreen, I/O board, power control, harnesses |
| Service Kit | Test adapter, programming access, service harness |

Kit planning should wait until the hardware is stable.

---

## Kit Contents Checklist

Every kit should eventually have a contents checklist.

Possible kit checklist items:

- Correct board revision
- Correct firmware flashed
- Wiring harness included
- Connectors included
- Mounting hardware included
- Insulation material included
- Labels included
- Install guide included
- QR code or documentation link included
- Warning notes included
- QA status recorded

A kit should not ship with unclear or unlabeled parts.

---

## Packaging Goals

Packaging should protect the parts and keep the kit organized.

Packaging goals:

- Prevent ESD damage
- Prevent bent pins
- Prevent scratched displays
- Keep small parts contained
- Label each bag or section
- Include revision information
- Include basic warning notes
- Include documentation link
- Look clean and intentional

The packaging should feel like an FBD product, not a loose parts bag.

---

## Label Planning

Labels may be useful for boards, bags, and kits.

Possible labels:

- Board name
- Board revision
- Firmware version
- Build profile
- QA pass label
- Warning label
- Kit contents label
- Customer support label
- QR code label
- Install step label
- Connector label

Labels should be clear and useful.

---

## Suggested Label Information

A product or board label may include:

| Label Field | Example |
| --- | --- |
| Product | FBD PS2 Button Butler |
| Board | Button Interposer |
| Revision | v0.3 |
| Firmware | v0.2.0 |
| Profile | ChipSlayer |
| QA | Pass |
| Date | YYYY-MM-DD |
| Build ID | FBD### |
| Notes | Prototype / Not for sale |

Prototype labels should clearly say prototype.

---

## Customer-Facing Copy

The `Etsy-eBay-Website-Copy/` folder can hold future product descriptions.

Customer-facing copy may include:

- Product overview
- Feature list
- Compatibility notes
- What is included
- What is not included
- Installation difficulty
- Required skill level
- Warning notes
- Support notes
- FAQ
- Listing-safe wording

Customer copy should be honest and not oversell untested features.

---

## Customer Copy Rules

Customer-facing Button Butler descriptions should:

- Be clear
- Avoid overpromising
- List only tested features
- Separate prototype ideas from supported features
- Explain compatibility limits
- Explain install skill level
- Mention that console models may vary
- Avoid confusing technical overload
- Avoid claims that are not proven

The FBD tone can be enthusiastic, but the product copy should stay accurate.

---

## Installation Documentation

Manufacturing should link to install documentation when available.

Install docs should include:

- Supported console models
- Required tools
- Required solder points
- Wire colors
- Board placement
- Connector pinouts
- Insulation notes
- Test steps
- Final assembly checks
- Troubleshooting notes

Installation documentation should be created only after real installs are tested.

---

## Customer Documentation

Customer documentation should explain only what the user needs to know.

Customer docs may include:

- What Button Butler does
- Basic button behavior
- Touchscreen overview if installed
- Hidden shortcuts if customer-facing
- What not to press
- What service mode means
- How to recover from safe mode if supported
- Feature limitations
- Support contact or documentation link

The customer should not need to understand the entire engineering project.

---

## Safety Notes

Button Butler can affect power/reset behavior and internal mod control.

Safety-critical areas include:

- Console power/reset button behavior
- Console reset line
- Modchip enable/disable behavior
- ChipSlayer state
- Accessory power control
- Video board power
- SD2PSX or memory-card-related behavior
- Touchscreen command handling
- Backfeeding between boards

Anything safety-critical must be tested before customer use.

---

## Do Not Release Until

A Button Butler board or kit should not be released until:

- Board revision is clearly marked
- Firmware version is clearly marked
- Correct firmware is flashed
- QA checklist passes
- Default states are safe
- No output glitches at startup
- No backfeeding issue is present
- Button behavior is reliable
- Console testing has passed
- Documentation exists
- Known limitations are listed
- Customer-facing features are tested

A working prototype is not the same thing as a releasable product.

---

## Release Notes

Future release notes should document:

- Hardware revision
- Firmware version
- Supported features
- Supported PS2 models
- Known issues
- Changes from previous version
- Required install changes
- Testing completed
- Warnings
- Upgrade notes

Release notes should be added for every serious hardware or firmware release.

---

## Product Candidate Requirements

A product-candidate Button Butler version should have:

- Stable schematic
- Stable PCB layout
- Stable BOM
- Stable firmware
- Known supported console models
- Known unsupported or untested models
- Complete QA checklist
- Repeatable install process
- Customer-safe default behavior
- Clear troubleshooting notes
- Clear customer documentation
- Clean packaging plan

This is the stage before calling anything a real product.

---

## Manufacturing Risks

Possible manufacturing risks:

- Wrong component installed
- Wrong firmware flashed
- Wrong board revision used
- Unsafe default output state
- Output glitches at startup
- Poor solder joint
- Connector installed backwards
- Mislabeling
- Backfeeding not detected
- QA skipped
- Prototype accidentally treated as product
- Customer receives unsupported feature
- Documentation does not match hardware
- Firmware does not match board revision

The manufacturing process should reduce these risks.

---

## Open Questions

Open manufacturing questions:

- Which board revision will become the first real prototype?
- Which MCU will be used for the first Button Interposer?
- Should boards be assembled by hand or ordered assembled later?
- What test jig is needed first?
- What labels should be used on prototype boards?
- What connector style is best for repeatable kits?
- Should kits be sold or only used in FBD builds?
- Should touchscreen kits be separate from basic interposer kits?
- What warranty or support limits should apply?
- What documentation should be included with customer builds?
- What features are safe enough to advertise first?

These questions should be answered after the first real prototypes are tested.

---

## Recommended Development Order

Recommended manufacturing development order:

1. Build first Button Interposer prototype
2. Create assembly notes
3. Create firmware flashing notes
4. Use QA checklist on the prototype
5. Record test results
6. Revise board if needed
7. Create repeatable test procedure
8. Add I/O Expansion or Touchscreen hardware later
9. Create kit packaging notes only after stable hardware
10. Create customer-facing copy only for tested features
11. Prepare release notes for stable revisions

Manufacturing documentation should follow real tested hardware, not just ideas.

---

## Summary

The `Manufacturing/` folder is where Button Butler becomes repeatable.

This folder should track assembly, QA, kit planning, labels, packaging, release notes, and customer-facing product preparation.

The most important manufacturing rule is simple:

**Do not treat Button Butler as product-ready until the hardware, firmware, install process, QA checklist, and documentation are all proven.**
