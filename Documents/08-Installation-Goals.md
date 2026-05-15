# 08 - Installation Goals

## Overview

The **FBD PS2 Button Butler** project is being designed with installation in mind from the beginning.

The goal is not only to add more features to a PlayStation 2. The goal is to make advanced internal mods cleaner, easier to repeat, easier to service, and less destructive to the console shell.

Button Butler should help reduce the number of external switches, buttons, LEDs, and holes needed for complex PS2 builds.

A successful install should look intentional, clean, and serviceable.

---

## Main Installation Goal

The main installation goal is simple:

**Add more control without making the console look hacked up.**

Button Butler should help avoid:

- Extra drilled holes
- Extra toggle switches
- Random external buttons
- Messy wiring
- Hard-to-reach internal controls
- Unlabeled wires
- Unserviceable board placement
- Confusing customer operation

The system should make a complex PS2 build feel more finished.

---

## Design Philosophy

Button Butler should be designed around real installation conditions.

Inside a PS2 shell, space is limited. Wire routing matters. Board placement matters. Heat matters. Service access matters.

The project should follow these design ideas:

1. Keep the base install simple.
2. Make advanced features optional.
3. Avoid unnecessary shell cutting.
4. Use clear labels.
5. Use practical connector locations.
6. Keep wiring short when possible.
7. Make the system testable before final assembly.
8. Make the system serviceable after installation.
9. Avoid making one failed accessory disable the whole console.
10. Keep customer operation simple.

The hardware should be designed for repeat installs, not just one successful prototype.

---

## Minimal Shell Modification

One of the biggest goals of Button Butler is to reduce shell modification.

Many PS2 mods require extra switches, extra buttons, extra LEDs, or external access points. Over time, the console shell can become cluttered.

Button Butler should reduce the need for these modifications by reusing:

- The original power/reset button
- The lid switch as a hidden modifier
- The touchscreen as a single control interface
- Internal I/O outputs for hidden controls
- Software profiles instead of physical switches where possible

The cleaner the shell, the more premium the final build feels.

---

## Shell Cutting Rules

Shell cutting should be avoided unless it adds real value.

Recommended shell cutting rules:

- Do not cut the shell for a feature that can be handled internally.
- Do not add a separate button if the power/reset button can safely handle the function.
- Do not add a separate switch if the touchscreen can control it.
- Do not add an LED hole unless the display, existing light pipe, or internal indicator cannot handle the status.
- Do not cut for service access unless the feature needs to be reached by the user.
- Keep any required cuts clean, symmetrical, and intentional.

If a cut is required, it should look factory-inspired or build-themed.

---

## Button Interposer Install Goals

The Button Interposer Board is the core of the project.

Its install should be as simple as possible.

Goals:

- Small board footprint
- Clear input and output labels
- Minimal solder points
- Easy access to power/reset button signals
- Easy access to lid switch signal if used
- Safe default behavior
- Test pads for troubleshooting
- Programming/debug pads
- No required touchscreen
- No required shell cutting

The base board should be useful even by itself.

---

## Touchscreen Install Goals

The touchscreen version is for advanced and premium builds.

The touchscreen should look like it belongs on the console.

Goals:

- Clean mounting location
- Secure physical mounting
- Good viewing angle
- Minimal shell cutting
- No exposed messy wiring
- Easy access to service if needed
- Display does not interfere with cooling
- Display does not block internal board placement
- Display shape matches the build style
- Screen can be removed or serviced if possible

The touchscreen should feel like a designed feature, not an afterthought.

---

## I/O Expansion Board Install Goals

The I/O Expansion Board should make wiring cleaner, not worse.

Goals:

- Provide extra outputs without overcrowding the main interposer
- Keep mod-control wiring organized
- Use clear output labels
- Provide known default states
- Keep high-current loads away from sensitive logic
- Allow easy testing of each output
- Support optional install only when needed
- Mount in a repeatable location where possible

The I/O Expansion Board should help make advanced builds easier to manage.

---

## Power Management Install Goals

Power management hardware should be installed carefully.

Goals:

- Keep accessory power wiring short and clean
- Avoid running high-current loads through tiny logic traces
- Use proper regulators, MOSFETs, or load switches
- Avoid backfeeding between boards
- Clearly separate power domains
- Label voltage rails clearly
- Add test points for voltage checks
- Keep heat-generating parts away from sensitive areas
- Avoid blocking airflow
- Make fuse or protection locations accessible if used

Power management should improve reliability, not create new failure points.

---

## Wiring Goals

Wiring should be clean, labeled, and serviceable.

Good wiring practices:

- Use consistent wire colors
- Keep wires as short as practical
- Avoid tight strain on solder pads
- Avoid routing wires over hot parts
- Avoid routing wires through screw posts or pinch points
- Keep signal wires away from noisy power wiring when possible
- Anchor wires where needed
- Leave enough slack for service access
- Avoid large wire bundles that prevent shell closure
- Document wire colors and destinations

A clean wiring plan makes the build easier to troubleshoot later.

---

## Suggested Wire Color Conventions

Wire colors should be consistent across builds when possible.

Example color conventions:

| Color | Suggested Use |
| --- | --- |
| Black | Ground |
| Red | Main positive power |
| Orange | 3.3V |
| Yellow | 5V or accessory power |
| White | UART TX or signal line |
| Green | UART RX or signal line |
| Blue | Control output |
| Purple | Service or special function |
| Gray | Status input |
| Brown | Lid switch or button-related signal |

These colors are only suggestions.

Whatever convention is used should be documented per build.

---

## Connector Goals

Connectors can make installs cleaner and more serviceable.

Connector goals:

- Use connectors where repeated service is expected
- Avoid connectors where direct soldering is more reliable
- Use keyed connectors when possible
- Label pin 1 clearly
- Keep connector pinouts consistent
- Avoid fragile connectors in high-stress locations
- Use locking connectors where vibration or movement is possible
- Provide strain relief where needed

Not every wire needs a connector, but serviceable sections should be planned.

---

## Possible Connector Groups

Possible connector groups:

| Connector | Purpose |
| --- | --- |
| Button connector | Physical power/reset button input and console-side output |
| Lid connector | Lid switch input |
| Touchscreen connector | Touchscreen power and UART |
| I/O connector | General-purpose outputs and inputs |
| Power connector | Accessory power input or switched output |
| Debug connector | UART, programming, and testing |
| Service connector | Installer-only test or configuration access |

Connector groups should be documented in the hardware folder.

---

## Board Placement Goals

Board placement should be planned early.

Good board placement should:

- Avoid heat sources
- Avoid moving parts
- Avoid screw posts
- Avoid RF shield conflicts
- Avoid controller port interference
- Avoid optical drive interference if present
- Avoid memory card slot interference
- Allow shell closure without pressure
- Allow wires to route cleanly
- Allow access to debug pads if possible

A board that works on the bench but does not fit cleanly in the shell is not finished.

---

## Board Mounting Options

Possible mounting methods:

| Method | Notes |
| --- | --- |
| Double-sided tape | Easy for prototypes but not ideal for final builds |
| Kapton tape isolation | Useful for insulation, not a mounting solution by itself |
| 3D printed bracket | Good for repeatable mounting if space allows |
| Screws and standoffs | Strong but may require shell planning |
| RF shield mounting | Useful if the shield is stable and grounded correctly |
| Adhesive standoffs | Easy but must be tested for long-term reliability |
| Custom carrier plate | Good for premium or repeatable builds |
| Flex PCB | Useful for tight areas and interposer-style installs |

Final products should avoid relying only on weak adhesive if the board may be serviced or pulled by wires.

---

## Service Access Goals

The install should be serviceable.

Service goals:

- Debug pads should not be buried permanently
- Firmware programming pads should be reachable if practical
- Connectors should be removable without destroying the install
- Test points should be labeled
- Important wires should be traceable
- The system should be testable before final shell closure
- A failed accessory should be replaceable if possible

A clean install should not become impossible to repair.

---

## Testing Before Final Assembly

Button Butler should be tested before the console is fully reassembled.

Recommended pre-assembly tests:

1. Confirm power and ground.
2. Confirm the interposer powers correctly.
3. Confirm the console still powers on.
4. Confirm the console still resets or behaves normally.
5. Confirm button input is detected.
6. Confirm lid switch input is detected if used.
7. Confirm UART debug output works.
8. Confirm outputs default to safe states.
9. Confirm each output changes only when requested.
10. Confirm no accessory is backfeeding the system.
11. Confirm touchscreen communication if installed.
12. Confirm shell closes without pinching wires.

The system should not be tested for the first time after everything is fully assembled.

---

## Final Assembly Checks

Before final closure, check:

- No wires are pinched
- No wires are under screw posts
- No exposed pads can short to the RF shield
- No board touches metal without insulation
- No connectors are loose
- No solder joints are under mechanical stress
- No mod board blocks airflow
- No accessory gets too hot
- No ribbon cables are strained
- No touchscreen wiring is twisted or pulled
- No buttons are stuck
- No outputs are active unexpectedly

A careful final check can prevent many problems.

---

## Install Documentation Goals

Every install should be documented well enough to repeat or troubleshoot.

Documentation should include:

- Console model
- Motherboard revision if known
- Button Butler board revision
- Firmware version
- Installed feature level
- Connected mods
- Wire colors
- Solder points
- Connector pinouts
- Default states
- Button behavior profile
- Test results
- Photos of install
- Notes about fitment

This information should be saved with the build records.

---

## Customer-Facing Simplicity

The finished build should be easy for the customer to understand.

Customer-facing goals:

- Normal power/reset behavior should be obvious.
- Hidden functions should be documented if the customer needs them.
- Dangerous or service-only functions should be hidden or protected.
- Touchscreen pages should use clear labels.
- The console should not require a complicated manual for basic use.
- Any unusual behavior should be explained in the build documentation.

Advanced features are good only if the user can understand them.

---

## Installer-Facing Controls

Some controls may be intended only for the installer.

Installer-facing controls may include:

- Service mode
- Output testing
- UART logs
- Button timing tests
- Lid switch tests
- Power management tests
- Modchip state override
- ChipSlayer testing
- Factory reset of Button Butler settings

These should not be easily triggered by accident during normal use.

---

## Build Profile Install Goals

Different builds may use different install profiles.

Possible install profiles:

| Profile | Install Focus |
| --- | --- |
| Basic | Power/reset button behavior only |
| ChipSlayer | Button shortcut and ChipSlayer output |
| Modchip | Modchip enable/disable behavior |
| BlueRetro | BlueRetro enable/reset/channel control |
| Bluetooth Audio | Audio enable and pairing trigger |
| Premium Touchscreen | Full touchscreen control and status |
| Service/Test | Debugging, output tests, and diagnostics |

Each profile should have its own wiring notes and behavior notes.

---

## Basic Install Profile

The basic install should be the easiest version.

Possible features:

- Button Interposer Board
- Normal pass-through behavior
- Short press and long press detection
- Lid switch input optional
- One output optional
- UART debug optional
- No touchscreen required
- No shell modification required

This profile proves the core hardware.

---

## ChipSlayer Install Profile

The ChipSlayer install profile focuses on mod control.

Possible features:

- Button Interposer Board
- ChipSlayer control output
- Lid switch modifier
- Hidden button shortcut
- Optional status LED
- Optional touchscreen control later

Example behavior:

- Lid closed equals normal button behavior
- Lid open plus button press toggles ChipSlayer

This avoids adding a separate ChipSlayer switch to the shell.

---

## Bluetooth Audio Install Profile

The Bluetooth audio profile focuses on pairing and enable control.

Possible features:

- Bluetooth audio enable output
- Bluetooth pairing trigger
- Optional reset output
- Optional touchscreen audio page
- Optional status LED

Example behavior:

- Lid open plus long press starts pairing
- Touchscreen Pair button starts pairing

This avoids adding a separate pairing button.

---

## BlueRetro Install Profile

The BlueRetro profile focuses on internal controller control.

Possible features:

- BlueRetro enable output
- BlueRetro reset output
- Optional channel enable outputs
- Optional touchscreen controller page
- Optional status input or UART support

Example behavior:

- Touchscreen can disable BlueRetro
- Service page can reset BlueRetro
- Lid open shortcut can toggle BlueRetro state

---

## Premium Touchscreen Install Profile

The premium touchscreen profile is for advanced FBD builds.

Possible features:

- Button Interposer Board
- Touchscreen Hub
- I/O Expansion Board
- Modchip control
- ChipSlayer control
- Bluetooth audio control
- BlueRetro control
- RGB lighting control
- Video power management
- SD2PSX display support
- Service and diagnostic pages

This profile should feel like a complete integrated system.

---

## Minimal Install Version

The minimal install version should be useful by itself.

Minimum install targets:

- Intercept or monitor power/reset button
- Preserve normal console behavior
- Provide one configurable output
- Provide debug UART or pads
- Require little or no shell modification

This version is important because it gives the project a practical starting point.

---

## Maximum Install Version

The maximum install version may include many modules.

Possible maximum install:

- Button Interposer Board
- Touchscreen Controller
- I/O Expansion Board
- Power management board
- BlueRetro control
- Bluetooth audio control
- ChipSlayer control
- Modchip control
- RGB lighting control
- Video board power control
- SD2PSX status display
- Router status or control
- Future laser feedback display

This version should be built only after the lower levels are proven.

---

## Fitment Considerations

Fitment depends on the PS2 model.

Important fitment factors:

- Slim vs Fat console
- Motherboard revision
- RF shield shape
- Optical drive installed or removed
- Internal HDMI board placement
- BlueRetro placement
- SD2PSX placement
- Memory card slot access
- Controller port wiring
- Fan and airflow path
- Touchscreen mounting location
- Existing shell mods

Each build type may need its own placement notes.

---

## PS2 Slim Install Notes

PS2 Slim builds are likely a main target.

Slim install concerns:

- Limited internal space
- Tight shell height
- RF shield clearance
- Ribbon cable routing
- Controller port area crowding
- Optical drive or no optical drive
- Heat from internal video boards
- Placement of BlueRetro and SD2PSX
- Touchscreen mounting on shell or custom top plate

Slim installs need compact boards and clean wire routing.

---

## PS2 Fat Install Notes

PS2 Fat builds may have more internal room, but they also have different constraints.

Fat install concerns:

- Larger shell but more internal structures
- Different power/reset board arrangement
- Front panel and ribbon cable differences
- HDD bay and network adapter area
- Cooling path
- RF shield layout
- Possible internal storage mods
- Different aesthetic goals

Fat support may need separate wiring diagrams and install notes.

---

## Ultra Slim Install Notes

FBD Ultra Slim-style builds are a major use case for Button Butler.

Ultra Slim install goals:

- Keep the build compact
- Avoid extra external controls
- Use Button Butler to replace multiple switches
- Integrate with BlueRetro
- Integrate with SD2PSX
- Manage HDMI board power if needed
- Support touchscreen control in premium builds
- Keep shell appearance clean and intentional

Button Butler fits especially well in compact builds because it reduces extra hardware clutter.

---

## Heat And Airflow Goals

Installation should not make heat problems worse.

Heat and airflow goals:

- Do not block the fan
- Do not block vents
- Do not place heat-sensitive boards near hot regulators
- Do not trap video boards without airflow
- Avoid large wire bundles over hot areas
- Use power management to reduce unnecessary heat
- Measure temperature during testing
- Leave airflow paths open where possible

Power management and clean board placement can help reduce heat.

---

## RF Shield And Insulation

The RF shield can be both useful and risky.

RF shield considerations:

- It can provide mechanical support
- It can act as a heat spreader in some builds
- It can short exposed pads if insulation is poor
- It can pinch wires if routing is bad
- It may need clearance checks
- It should not be used as a random ground unless planned

Use Kapton tape, insulating sheets, or proper standoffs where needed.

---

## Strain Relief

Wires should not pull directly on fragile pads.

Strain relief methods:

- Small adhesive anchors
- Kapton tape
- Wire routing loops
- Cable lacing or small ties
- Connector retention
- Hot glue only where appropriate
- Mechanical brackets

Strain relief is especially important for customer builds that may be opened later.

---

## Avoiding Install Mistakes

Button Butler should be designed to reduce common install mistakes.

Design choices that help:

- Clear silkscreen labels
- Different connector sizes for different functions
- Pin 1 markings
- Test pads
- Color-coded wiring guides
- Default safe states
- Install checklists
- Board revision markings
- Firmware version markings
- Documentation photos

A good board design should make the correct install obvious.

---

## QA Checklist Goal

Every completed install should have a QA checklist.

Possible checklist sections:

- Visual inspection
- Continuity checks
- Power checks
- Button behavior checks
- Lid switch checks
- UART checks
- Output checks
- Touchscreen checks
- Mod control checks
- Heat checks
- Gameplay checks
- Final customer operation check

The QA checklist should eventually live in:

`Manufacturing/QA-Checklist.md`

---

## Test Data Goal

Testing should generate useful records.

Useful test data:

- Button timing values
- Output voltage states
- Startup behavior
- Shutdown behavior
- Touchscreen communication logs
- UART logs
- Power measurements
- Heat measurements
- Mod control test results
- Model-specific install notes

Test data should be saved in:

`Test-Data/`

---

## Release Install Goal

A release-ready install should include:

- Stable hardware revision
- Stable firmware version
- Clear wiring diagram
- Known supported PS2 models
- Known unsupported PS2 models
- Clear feature list
- Install checklist
- QA checklist
- Customer operation notes
- Troubleshooting notes

A product version should not rely on memory or guesswork.

---

## Open Questions

Open installation questions:

- What is the best mounting location for the base interposer board?
- Should the base board use connectors or solder pads?
- What connector style is best for small repeat installs?
- How many wires should the Level 1 install require?
- Should the lid switch input be optional?
- What is the cleanest touchscreen mounting method?
- Should the touchscreen be mounted to the shell, RF shield, or custom bracket?
- What is the safest board placement for Slim builds?
- What is the safest board placement for Fat builds?
- How should service access be handled after final assembly?
- Should different PS2 models get different harnesses?
- Should premium builds use a custom internal carrier plate?

These questions should be answered through prototype installs.

---

## Risks

Possible installation risks:

- Too many wires
- Shell not closing cleanly
- Board shorting to RF shield
- Button behavior not working after final assembly
- Touchscreen fitment issues
- Wire pinch points
- Poor strain relief
- Heat buildup
- Difficult service access
- Connector confusion
- Customer confusion
- Different PS2 models requiring different layouts

The project should solve install problems, not create new ones.

---

## Recommended Installation Development Order

Recommended order:

1. Define the Level 1 wiring
2. Build a simple interposer prototype
3. Test the board outside the console
4. Install in one test console
5. Confirm normal button behavior
6. Add lid switch input
7. Add one output
8. Document all solder points
9. Photograph the install
10. Test shell closure
11. Add I/O expansion only after the base board fits
12. Add touchscreen mounting only after the base system works
13. Create model-specific install notes
14. Create QA checklist

Start with fitment and reliability before adding premium features.

---

## Summary

Button Butler should be install-friendly from the beginning.

The project should reduce shell modification, reduce external switches, improve wiring organization, and make advanced PS2 builds easier to service.

The base install should be simple and reliable.

The advanced touchscreen install should look like a premium feature.

The final goal is a clean, repeatable, FBD-style installation system that makes complex PS2 mods easier to build, easier to use, and easier to support.
