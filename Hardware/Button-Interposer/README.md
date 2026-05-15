# Button Interposer Hardware

## Overview

This folder contains hardware planning notes, schematic ideas, PCB notes, and future design files for the **Button Interposer Board** used in the **FBD PS2 Button Butler** project.

The Button Interposer Board is the core hardware block of Button Butler.

Its job is to sit between the original PlayStation 2 power/reset button circuit and the console, allowing the button behavior to be monitored, passed through, blocked, modified, or used to trigger other internal mod-control functions.

The Button Interposer Board should work as a useful standalone board, even without the touchscreen or I/O expansion board installed.

---

## Purpose

The purpose of the Button Interposer Board is to make the original PS2 power/reset button smarter without adding extra external switches or drilling unnecessary holes in the console shell.

The board should allow:

- Normal power/reset button behavior
- Button press monitoring
- Short press detection
- Long press detection
- Lid-switch based alternate behavior
- Safe button pass-through
- Safe button blocking when needed
- Digital outputs for mod control
- UART debug and future touchscreen communication
- Optional service or test features

This board is the foundation of the Button Butler system.

---

## Project Status

| Area | Status |
| --- | --- |
| Hardware concept | In planning |
| Schematic | Not started or early planning |
| PCB layout | Not started |
| MCU selection | Not finalized |
| Button pass-through method | Not finalized |
| Output circuit style | Not finalized |
| UART connector | Planned |
| Lid switch input | Planned |
| Bench testing | Not started |
| Console testing | Not started |
| Customer-ready status | Not ready |

This hardware should be treated as experimental until it is built and tested.

---

## Main Design Goal

The main design goal is:

**Safely intercept the PS2 power/reset button while preserving normal console behavior.**

The board should not make the console harder to use.

If no special action is being performed, the PS2 should behave normally.

---

## Core Functions

The Button Interposer Board may provide:

- Physical button input
- Console-side button output
- MCU-based button monitoring
- Button debounce support through firmware
- Lid switch input
- UART TX/RX
- One or more digital outputs
- Service input or jumper
- Programming pads
- Status LED or test LED
- Expansion connector for future features

The first revision should stay simple and focus on proving the button behavior.

---

## Hardware Philosophy

The board should be designed around practical PS2 installation.

Important design priorities:

1. Small board size
2. Clear silkscreen labels
3. Safe default states
4. Minimal solder points
5. Easy bench testing
6. Easy console testing
7. Easy debug access
8. No unnecessary shell cutting
9. No fragile wiring
10. Expandable without making the first board too complicated

This board should solve an installation problem, not create a new one.

---

## Relationship To Other Button Butler Boards

The Button Interposer Board is the core board.

Other boards may connect to it later.

| Board | Relationship |
| --- | --- |
| Button Interposer Board | Core controller for power/reset button behavior |
| Touchscreen Hub | Sends command requests and displays status |
| I/O Expansion Board | Provides additional inputs and outputs |
| Power Management Board | Controls accessory power if used |
| Service Adapter | Allows debug or UART access during testing |

The interposer should be able to operate without the optional boards.

---

## Recommended First Hardware Revision

The first hardware revision should be as simple as possible.

Recommended first revision features:

- MCU or PIC
- Button input
- Console-side button output
- Lid switch input
- UART TX/RX pads
- One general-purpose output
- One spare output or status LED
- Programming pads
- Power and ground pads
- Test points

Avoid adding too many advanced features to the first board.

---

## Minimum Viable Hardware

The minimum useful board should include:

| Signal | Purpose |
| --- | --- |
| `BTN_IN` | Reads the physical PS2 power/reset button |
| `BTN_OUT` | Sends or blocks the button action to the console |
| `LID_IN` | Reads lid switch state if used |
| `OUT1` | General mod-control output |
| `UART_TX` | Sends debug/status data |
| `UART_RX` | Receives debug/touchscreen commands |
| `VCC` | Logic power |
| `GND` | Ground |
| `MCLR / RESET` | MCU reset or programming |
| `PGD / PGC` | Programming pins if using PIC ICSP |

This is enough to prove the concept.

---

## Possible Board Signals

Possible full signal list:

| Signal | Direction | Description |
| --- | --- | --- |
| `BTN_IN` | Input | Physical power/reset button input |
| `BTN_OUT` | Output | Console-side button output or control signal |
| `LID_IN` | Input | Lid switch input |
| `OUT1` | Output | General mod-control output |
| `OUT2` | Output | Spare output, LED, chime, or second control output |
| `IN1` | Input | Spare input or service jumper |
| `UART_TX` | Output | UART transmit |
| `UART_RX` | Input | UART receive |
| `VCC` | Power | Logic supply |
| `GND` | Power | Ground |
| `MCLR` | Input | MCU reset/programming pin |
| `PGD` | Bidirectional | PIC programming data if used |
| `PGC` | Input | PIC programming clock if used |
| `STATUS_LED` | Output | Optional status LED |

The exact list depends on the selected MCU and board size.

---

## MCU Selection

The first board may use a PIC microcontroller.

Useful MCU traits:

- Enough GPIO pins
- Hardware UART if possible
- Internal oscillator
- Timer support
- Watchdog timer
- Low current draw
- Small package
- Easy programming
- Available from common suppliers
- Enough memory for simple command handling
- Stable behavior at the selected voltage

The selected MCU should not be so small that it makes the first firmware painful.

---

## Possible MCU Requirements

Minimum MCU requirements:

| Requirement | Reason |
| --- | --- |
| 1 button input | Read physical button |
| 1 button output | Control console-side behavior |
| 1 lid input | Hidden modifier behavior |
| 1 UART TX | Debug and status |
| 1 UART RX | Commands and future touchscreen |
| 1 or 2 outputs | Mod control or status |
| Timer | Button debounce and press timing |
| Watchdog | Optional reliability feature |
| Programming pins | Firmware flashing |

More GPIO gives more flexibility during development.

---

## Logic Voltage

The logic voltage must be chosen carefully.

Possible logic voltages:

- 3.3V
- 5V

Important rules:

- Match the selected MCU voltage.
- Match or level-shift UART signals.
- Do not feed 5V into 3.3V-only devices.
- Avoid backfeeding unpowered boards through signal lines.
- Confirm voltage compatibility with the PS2 signals being monitored.
- Confirm voltage compatibility with the touchscreen controller.

The final voltage should be based on real PS2 signal measurements and selected hardware.

---

## Power Input

The board needs a stable logic supply.

Possible power sources:

- PS2 standby power rail
- PS2 switched power rail
- Local regulator from a higher voltage rail
- Shared accessory logic rail
- Development bench supply during testing

The first board should include clearly labeled power pads.

---

## Standby Power Consideration

If the board needs to detect button activity while the console is in standby, it may need standby power.

Possible standby-powered features:

- Button monitoring
- Wake or power request handling
- UART readiness
- Touchscreen wake signal
- Safe output defaults

Standby current should be kept low.

The board should not draw excessive current when the console is off.

---

## Switched Power Consideration

If the board only needs to operate when the console is on, switched power may be enough.

However, switched-only power may limit:

- Power-on behavior
- Touchscreen power requests
- Standby monitoring
- Wake features
- Advanced power management

The correct power source depends on the final behavior goals.

---

## Button Intercept Methods

The safest method for intercepting the PS2 power/reset button must be determined through testing.

Possible methods:

| Method | Description |
| --- | --- |
| Passive monitor only | MCU watches button but does not interrupt it |
| Controlled pass-through | MCU controls whether button reaches console |
| Analog switch | Electronic switch passes or blocks the button signal |
| Open-drain simulation | MCU or transistor simulates button press |
| Relay or solid-state switch | Isolated switching method |
| Cut-and-insert interposer | Board sits directly in series with button line |

The first hardware should be tested carefully before cutting or interrupting important console signals.

---

## Passive Monitor Mode

Passive monitor mode is the safest starting point.

In this mode:

- The original button still connects normally.
- The MCU only observes the button state.
- The console behavior is not changed.
- UART can report button events.
- Firmware can prove debounce and timing.

This is a good first bench and console test mode.

---

## Controlled Pass-Through Mode

Controlled pass-through mode allows the MCU to decide whether the button reaches the console.

Possible uses:

- Normal pass-through
- Block button when lid is open
- Redirect button press to an alternate action
- Allow touchscreen-requested virtual button press
- Enter service mode

This mode is more powerful but also more risky.

It should be tested after passive monitoring is proven.

---

## Analog Switch Method

An analog switch may be useful for cleanly passing or blocking the button signal.

Possible advantages:

- Solid-state switching
- Small package
- Low control current
- Can behave like a signal switch
- Avoids mechanical relays

Possible concerns:

- On resistance
- Off leakage
- Voltage range
- Power-off behavior
- Signal direction
- Startup default state
- Whether the switch affects the PS2 button circuit

The analog switch part must be selected based on the actual signal requirements.

---

## Open-Drain Simulation Method

Some PS2 button behavior may be simulated by pulling a signal low.

An open-drain or open-collector style output may be useful.

Possible advantages:

- Avoids actively driving a signal high
- Reduces contention risk
- Useful for simulating button presses
- Useful for shared lines with pull-ups

Possible concerns:

- Requires known pull-up behavior
- Requires correct active-low logic
- May not work if the button circuit is not simple
- Needs safe default high-impedance state

This method should be tested with real hardware.

---

## Hardware Pass-Through Default

A safe hardware default is important.

Possible default behavior goals:

- Console can still power on if MCU is reset
- Console can still power on if touchscreen is missing
- Button line is not held active
- Reset is not held unintentionally
- Outputs default inactive
- Mod-control lines default to a known safe state

Hardware should not rely only on firmware for safe startup.

---

## Fail-Safe Considerations

The board should be designed so failures are as safe as practical.

Possible fail-safe goals:

- If MCU firmware crashes, console button still works if possible.
- If touchscreen is disconnected, console button still works.
- If UART is noisy, outputs do not change randomly.
- If power is unstable, outputs stay inactive.
- If board is unpowered, it does not backfeed or block the console if possible.
- If a command is unsafe, the hardware cannot accidentally force it.

True fail-safe behavior depends on circuit design.

---

## Lid Switch Input

The lid switch input is useful as a hidden modifier.

Possible uses:

- Alternate button behavior
- Service mode entry
- ChipSlayer toggle
- Bluetooth pairing trigger
- Modchip state change
- Installer-only functions

The lid switch input should have a defined default state.

Floating lid inputs should be avoided.

---

## Lid Switch Electrical Notes

The lid switch signal must be measured and understood before connecting.

Important questions:

- Is the lid switch active-high or active-low?
- What voltage is present?
- Is it pulled up internally?
- Can it be safely monitored without affecting console behavior?
- Does it differ between PS2 models?
- Does it need a series resistor or buffer?
- Does it share ground with the Button Butler board?

The first version should treat lid switch input as optional until tested.

---

## Digital Outputs

The board may include one or more digital outputs.

Possible output uses:

- ChipSlayer enable
- Modchip enable or disable control
- Bluetooth audio pairing trigger
- Bluetooth audio enable
- BlueRetro reset
- RGB lighting enable
- Status LED
- Chime or buzzer
- Accessory power enable

Each output should have a documented default state and active state.

---

## Output Electrical Styles

Possible output styles:

| Output Style | Best Use |
| --- | --- |
| Push-pull GPIO | Same-voltage local logic |
| Open-drain output | Pull-low control lines |
| NPN transistor | Low-side small load or pull-down |
| N-channel MOSFET | Low-side switching |
| P-channel MOSFET | High-side switching with proper drive |
| Load switch enable | Accessory power control |
| Logic buffer | Clean signal drive or isolation |
| Analog switch | Signal pass-through control |

The selected style depends on what the output controls.

---

## Open-Drain Output Preference

Open-drain outputs are often safer for mod-control signals.

Possible uses:

- Pulling enable lines low
- Simulating button presses
- Triggering reset lines
- Avoiding drive conflict
- Allowing external pull-ups to define voltage

Where the target signal is not fully understood, open-drain style control may be safer than push-pull.

---

## Output Default States

Every output needs a defined default state.

Example defaults:

| Output | Suggested Default |
| --- | --- |
| `OUT1` | Inactive |
| `OUT2` | Inactive |
| ChipSlayer control | Build-profile dependent but defined |
| Modchip control | Safe known state |
| Bluetooth pairing | Inactive |
| BlueRetro reset | Inactive |
| RGB enable | Off |
| Buzzer | Off |
| Accessory enable | Off unless required |

No output should float.

---

## UART Hardware

The board should include UART access.

Possible UART uses:

- Firmware debug output
- Button event logs
- Touchscreen communication
- Service commands
- I/O expansion communication
- Firmware development

The UART should be easy to access during bench testing.

---

## UART Connector Or Pads

Possible UART connection options:

| Option | Notes |
| --- | --- |
| Through-hole header | Easy for development, larger |
| SMD pads | Smaller, good for final board |
| JST connector | Cleaner harness option |
| Tag-Connect style pads | Good for production or programming |
| Castellated edge pads | Useful for interposer or daughterboard designs |

The first prototype may benefit from larger debug pads or a header footprint.

---

## UART Pin Labels

UART pins should be clearly labeled.

Recommended labels:

- `TX`
- `RX`
- `GND`
- `VCC`
- `DBG_TX`
- `DBG_RX`

Remember:

- Board `TX` connects to adapter `RX`.
- Board `RX` connects to adapter `TX`.
- Ground must be shared.

---

## Programming Pads

The board should include programming access.

If using PIC ICSP, possible pins include:

| Signal | Purpose |
| --- | --- |
| `VPP / MCLR` | Programming voltage or reset |
| `VDD` | Target power reference |
| `VSS` | Ground |
| `PGD` | Programming data |
| `PGC` | Programming clock |

The exact pins depend on the selected PIC.

Programming pads should be accessible after assembly if possible.

---

## Test Points

Test points are important for development.

Useful test points:

- `VCC`
- `GND`
- `BTN_IN`
- `BTN_OUT`
- `LID_IN`
- `OUT1`
- `OUT2`
- `UART_TX`
- `UART_RX`
- `MCLR`
- `PGD`
- `PGC`

Even small test pads can make troubleshooting easier.

---

## Status LED

A status LED may be useful.

Possible status LED uses:

- MCU boot indicator
- Firmware heartbeat
- Service mode indicator
- Output activity indicator
- Error indicator
- UART activity indicator

The LED should be optional if board space is tight.

A solder jumper or firmware option could disable it for low-power builds.

---

## Service Jumper

A service jumper may be useful for development and recovery.

Possible service jumper uses:

- Force service mode
- Force safe mode
- Disable advanced behavior
- Enable UART commands
- Allow output testing
- Restore default settings

The service jumper should have a defined pull-up or pull-down.

It should not float.

---

## Connector Planning

Connectors should be considered early.

Possible connector groups:

| Connector | Signals |
| --- | --- |
| Button connector | `BTN_IN`, `BTN_OUT`, `GND` if needed |
| Lid connector | `LID_IN`, `GND`, reference if needed |
| UART connector | `TX`, `RX`, `GND`, `VCC` reference |
| Output connector | `OUT1`, `OUT2`, `GND`, optional power |
| Programming connector | `MCLR`, `VDD`, `GND`, `PGD`, `PGC` |
| Expansion connector | UART, power, ground, spare I/O |

The first board may use pads instead of connectors to save space.

---

## Board Size Goals

The board should be small enough for Slim and Ultra Slim installs.

Board size goals:

- Fit in tight PS2 Slim spaces
- Avoid RF shield conflicts
- Avoid optical drive conflicts if the drive is installed
- Allow wire routing without shell pressure
- Provide enough pad size for hand soldering
- Leave room for labels
- Leave room for test points

Do not make the board so small that it becomes difficult to solder or debug.

---

## Board Thickness

Possible board thicknesses:

| Thickness | Notes |
| --- | --- |
| 1.6 mm | Standard, strong, common |
| 1.0 mm | Thinner, useful in tight spaces |
| 0.8 mm | Very useful for compact PS2 mod boards |
| Flex PCB | Future option for interposer-style installs |

For early prototypes, 0.8 mm or 1.0 mm may be useful if fitment is tight.

---

## PCB Manufacturing Notes

Possible PCB manufacturers:

- OSH Park
- JLCPCB
- PCBWay
- Other prototype PCB services

Prototype boards should include:

- Clear revision marking
- Board name
- Pin labels
- Test pads
- Mounting information if possible
- FBD or Button Butler marking if space allows

Do not remove useful labels just for artwork.

---

## Silkscreen Goals

The silkscreen should be practical.

Useful silkscreen labels:

- Board name
- Revision
- `BTN_IN`
- `BTN_OUT`
- `LID`
- `OUT1`
- `OUT2`
- `TX`
- `RX`
- `VCC`
- `GND`
- Pin 1 markers
- Programming pin labels
- Warning marks for voltage

Branding is good, but labels are more important.

---

## Mounting Goals

The board should be mountable in a repeatable way.

Possible mounting methods:

- Double-sided tape for prototypes
- Adhesive standoffs
- Small printed bracket
- RF shield mounting point
- Custom carrier board
- Flex interposer
- Screw/standoff if space allows

Final product versions should avoid relying only on weak adhesive.

---

## Insulation

The board must be insulated from metal shields and other boards.

Insulation methods:

- Kapton tape
- Fish paper
- Printed bracket
- Standoffs
- Solder mask clearance awareness
- No exposed pads facing metal
- Proper strain relief

Do not allow exposed pads to touch the RF shield.

---

## Wire Routing

Wire routing should be planned.

Wire routing goals:

- Keep wires short
- Avoid pinch points
- Avoid screw posts
- Avoid sharp shell edges
- Avoid hot parts
- Avoid moving optical drive parts
- Keep UART wires away from noisy power wires if possible
- Anchor wires for strain relief
- Keep shell closure easy

A clean board still fails if the wiring is messy.

---

## Suggested Wire Colors

Suggested wire colors for prototypes:

| Color | Signal |
| --- | --- |
| Black | Ground |
| Red | Power |
| Orange | 3.3V |
| Yellow | 5V or accessory power |
| White | UART TX |
| Green | UART RX |
| Blue | Button or control output |
| Purple | Service or special function |
| Gray | Status input |
| Brown | Lid switch |

These are suggestions only.

Document the colors used in each build.

---

## Backfeeding Prevention

Backfeeding must be considered.

Possible backfeeding paths:

- UART TX/RX
- Button input/output line
- Lid switch input
- Mod-control output
- Pull-up resistors
- Programming adapter
- Touchscreen connection
- Accessory enable line

Prevention methods:

- Series resistors
- Open-drain outputs
- High-impedance defaults
- Level shifting
- Power-domain-aware pull-ups
- MOSFET isolation if needed
- Avoid driving unpowered devices
- Test for voltage on unpowered rails

Backfeeding should be tested with a meter.

---

## Noise Considerations

The board may be installed near noisy electronics.

Possible noise sources:

- Optical drive motors
- Switching regulators
- Video boards
- Wireless modules
- RGB lighting
- Internal routers
- Long wires

Noise reduction methods:

- Decoupling capacitors near MCU
- Short signal wires
- Series resistors where useful
- Good ground reference
- Avoid routing UART next to power switching wires
- Pull-ups or pull-downs on inputs
- Debounce in firmware
- Filtering if needed

Button input reliability is important.

---

## Decoupling

The MCU should have proper decoupling.

Typical decoupling:

- 0.1 uF ceramic capacitor close to MCU power pins
- Additional bulk capacitance if needed
- Separate filtering if powered from a noisy rail

Final values depend on the selected MCU and power source.

---

## Pull-Ups And Pull-Downs

Inputs should have defined states.

Possible inputs needing pull resistors:

- `BTN_IN`
- `LID_IN`
- `SERVICE_IN`
- Feedback inputs
- Optional mode select pins

Pull direction depends on circuit behavior.

Do not assume internal pull-ups are enough until tested.

---

## Series Resistors

Series resistors can help protect signals.

Possible uses:

- UART TX/RX
- Button sense line
- Lid switch sense line
- MCU outputs to external boards
- Touchscreen communication lines

Series resistors can reduce current in fault or contention cases.

Values should be selected based on the signal and speed.

---

## ESD And Protection

The board may connect to points that users indirectly touch.

Possible protection considerations:

- ESD protection for external or semi-external controls
- Series resistors on signal lines
- Input clamping awareness
- Connector protection
- Avoid long antenna-like wires
- Avoid exposed test pads in final builds

ESD protection may become more important for product versions.

---

## Supported PS2 Models

Supported models are not finalized.

Possible target models:

- PS2 Slim SCPH-700xx
- PS2 Slim SCPH-750xx
- PS2 Slim SCPH-770xx
- PS2 Slim SCPH-790xx
- PS2 Slim SCPH-900xx
- PS2 Fat models
- FBD Ultra Slim builds
- Future FBD custom shell builds

Each model may require different wiring and fitment notes.

---

## Model-Specific Concerns

Different PS2 models may have different button circuits or board layouts.

Things to document by model:

- Button board location
- Button signal behavior
- Standby voltage availability
- Lid switch behavior
- Ground points
- Fitment space
- RF shield clearance
- Power rail differences
- Shell closure concerns

Do not assume one wiring method fits every model.

---

## PS2 Slim Notes

PS2 Slim models are likely the first target.

Slim concerns:

- Tight internal space
- Low shell height
- RF shield clearance
- Lid switch access
- Power/reset button board location
- Optical drive or no optical drive
- Internal BlueRetro placement
- SD2PSX placement
- HDMI board placement
- Heat management

Small board size and clean wiring are important.

---

## PS2 Fat Notes

PS2 Fat support may come later.

Fat concerns:

- Different front panel button layout
- Different ribbon cable routing
- More internal room but more structures
- HDD bay and network adapter area
- Different standby behavior
- Different install goals

Fat support should get its own tested wiring notes later.

---

## Ultra Slim Notes

FBD Ultra Slim builds are a major use case.

Ultra Slim goals:

- Minimal external controls
- Clean shell appearance
- Internal BlueRetro support
- Internal SD2PSX support
- HDMI board support
- Possible touchscreen integration
- Reduced wiring clutter
- Hidden mod control
- Serviceable install

Button Butler fits this style well because it reduces the need for extra switches.

---

## Bench Testing Plan

Before installing in a console, bench test the board.

Bench testing should include:

1. Visual inspection
2. Continuity check
3. Power rail check
4. Current draw check
5. MCU programming check
6. UART output check
7. Button input simulation
8. Lid input simulation
9. Output default state check
10. Output toggle test
11. Startup glitch test
12. Reset glitch test
13. Service jumper test if installed

Bench testing should be documented.

---

## Console Testing Plan

After bench testing, test in a real console.

Console testing should include:

1. Confirm console works before install
2. Connect passive monitor mode first if possible
3. Confirm button still works normally
4. Confirm UART reports button state
5. Confirm lid switch state if connected
6. Confirm no random resets
7. Confirm no power-on issues
8. Test pass-through behavior
9. Test blocking behavior only after pass-through is stable
10. Test one output with a safe target
11. Test power cycling
12. Test shell closure

Do not start with the most complex behavior.

---

## Test Data To Save

Save useful hardware test data.

Suggested folder:

`Test-Data/Button-Tests/`

Useful data:

- Button voltage measurements
- Lid switch voltage measurements
- Power rail measurements
- Current draw
- UART logs
- Startup behavior notes
- Shutdown behavior notes
- Output voltage tests
- Console model tested
- Motherboard revision if known
- Firmware version
- Board revision

Good test notes make future revisions better.

---

## Schematic Documentation

The schematic should clearly document:

- MCU part number
- Power source
- Logic voltage
- Button input circuit
- Button output circuit
- Lid input circuit
- Output drivers
- UART connector
- Programming header
- Pull-ups and pull-downs
- Test points
- Default states
- Safety notes

Every signal should have a clear purpose.

---

## PCB Documentation

PCB documentation should include:

- Board dimensions
- Board thickness
- Layer count
- Copper weight if important
- Manufacturer settings
- Revision number
- Render images
- Top and bottom screenshots
- Assembly notes
- Known fitment issues
- Connector orientation
- Test point locations

Board revisions should be tracked carefully.

---

## Suggested Folder Structure

| Path | Purpose |
| --- | --- |
| `KiCad/` | KiCad project files |
| `Schematics/` | Exported schematic PDFs or images |
| `Images/` | Board renders, layout screenshots, install photos |
| `Gerbers/` | Prototype Gerber exports if shared |
| `BOM/` | Button Interposer BOM files |
| `README.md` | Hardware overview and planning notes |

Additional folders can be added later.

---

## File Naming

Suggested file naming:

| File | Purpose |
| --- | --- |
| `FBD-PS2-Button-Butler-Interposer.kicad_pro` | KiCad project |
| `FBD-PS2-Button-Butler-Interposer-v0.1.pdf` | Schematic export |
| `FBD-PS2-Button-Butler-Interposer-v0.1-Gerbers.zip` | Prototype Gerbers |
| `FBD-PS2-Button-Butler-Interposer-v0.1-BOM.csv` | BOM |
| `FBD-PS2-Button-Butler-Interposer-v0.1-Top.png` | Top render |
| `FBD-PS2-Button-Butler-Interposer-v0.1-Bottom.png` | Bottom render |

Consistent file names make revisions easier to track.

---

## Revision Naming

Suggested board revision naming:

| Revision | Meaning |
| --- | --- |
| `v0.0` | Concept only |
| `v0.1` | First schematic prototype |
| `v0.2` | First PCB prototype |
| `v0.3` | Bench-tested revision |
| `v0.4` | Console-tested revision |
| `v0.5` | Improved prototype |
| `v1.0` | First stable release candidate |

Do not call a revision stable until it has been tested in real hardware.

---

## BOM Notes

The BOM should include:

- Reference designator
- Quantity
- Value
- Package
- Manufacturer part number
- Supplier part number
- Voltage rating
- Tolerance
- Notes
- Substitute parts if available

For small SMD parts, common footprints should be used where practical.

---

## Component Selection Goals

Component selection should prioritize:

- Availability
- Hand-solderability for prototypes
- Small size
- Reliable packages
- Common values
- Known suppliers
- Stable supply chain
- Correct voltage ratings
- Clear datasheets

Do not choose hard-to-source parts unless they solve a real problem.

---

## Prototype Assembly Goals

Prototype boards should be easy to assemble and inspect.

Assembly goals:

- Use practical SMD sizes
- Avoid overly tiny parts unless required
- Leave enough spacing around programming pads
- Keep polarity markings clear
- Label connectors clearly
- Use test pads
- Avoid parts under connectors if possible
- Keep MCU orientation obvious

The first board should be friendly for debugging.

---

## Manufacturing Goals

Future manufacturing goals:

- Clean BOM
- Repeatable assembly
- Clear QA checklist
- Stable firmware flashing process
- Clear labeling
- Test jig support
- Known supported PS2 models
- Installation guide
- Customer-safe behavior

Manufacturing should only be planned after prototypes are stable.

---

## QA Checklist Ideas

Future QA checklist items:

- Visual inspection
- Check for shorts
- Confirm correct voltage
- Confirm current draw
- Flash firmware
- Confirm UART output
- Confirm button input
- Confirm lid input
- Confirm output defaults
- Toggle outputs
- Confirm safe mode
- Confirm pass-through behavior
- Confirm service mode
- Label firmware version
- Record board revision

A detailed QA checklist should eventually live in:

`Manufacturing/QA-Checklist.md`

---

## Known Issues

Current known issues:

- Final MCU is not selected.
- Button intercept method is not finalized.
- Board size is not finalized.
- Logic voltage is not finalized.
- Power source is not finalized.
- Output driver style is not finalized.
- Lid switch behavior is not tested.
- UART connector style is not finalized.
- No prototype has been bench tested yet.
- No prototype has been console tested yet.

This list should be updated as the design develops.

---

## Open Questions

Open hardware questions:

- Which PIC or MCU should be used for the first board?
- Should the board be powered from standby power?
- Should the button line default to hardware pass-through if the MCU is off?
- What is the safest button intercept circuit?
- Should `BTN_OUT` use an analog switch, transistor, open-drain output, or another method?
- Is the PS2 button signal active-high or active-low on each target model?
- How should lid switch input be connected safely?
- How many outputs should the first board include?
- Should outputs be open-drain by default?
- Should UART use pads or a connector?
- Should ICSP use pads or a connector?
- Should there be a service jumper?
- What is the best board size for Slim installs?
- Should the first prototype be 0.8 mm or 1.6 mm thick?
- Should the board be designed as a rigid board, flex board, or both?

These questions should be answered through measurements and prototypes.

---

## Risks

Possible hardware risks:

- Console button no longer works
- Console stuck in reset
- Console will not power on
- MCU output glitches on startup
- Button line is loaded too much
- Lid switch input affects console behavior
- UART backfeeds another board
- Wrong logic voltage damages a device
- Output line activates unexpectedly
- Shell will not close
- Board shorts to RF shield
- Wire routing creates intermittent faults
- Different PS2 models behave differently

The safest path is to start with passive monitoring and expand carefully.

---

## Recommended Development Order

Recommended hardware development order:

1. Measure PS2 button signal behavior
2. Measure lid switch signal behavior
3. Choose a simple MCU for first prototype
4. Build passive monitor test circuit
5. Add UART debug
6. Add one output with safe default
7. Test on bench
8. Test in console without interrupting button behavior
9. Add controlled pass-through circuit
10. Test pass-through and block behavior
11. Add lid-switch alternate behavior
12. Add connector improvements
13. Add second output or expansion connector
14. Create first proper PCB prototype
15. Document results

Do not jump directly to the full touchscreen system.

---

## Minimum First Prototype

A good first prototype could include:

- PIC or MCU
- Power input
- Ground
- Button input monitor
- UART TX/RX
- Lid switch input
- One output
- Status LED
- Programming pads
- Test pads

This board can prove the firmware and signal behavior before the final pass-through circuit is chosen.

---

## Summary

The Button Interposer Board is the foundation of the FBD PS2 Button Butler project.

Its main job is to safely monitor and control the original PS2 power/reset button behavior.

The first hardware revision should stay simple, testable, and install-friendly.

After the base board can reliably read the button, report events, detect lid switch state, and control one safe output, the project can expand into controlled pass-through behavior, ChipSlayer control, touchscreen communication, I/O expansion, and advanced mod management.

The board should always prioritize safe PS2 operation over extra features.
