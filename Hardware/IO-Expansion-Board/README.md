# I/O Expansion Board Hardware

## Overview

This folder contains hardware planning notes, schematic ideas, PCB notes, and future design files for the **I/O Expansion Board** used in the **FBD PS2 Button Butler** project.

The I/O Expansion Board is an optional hardware block that gives Button Butler more raw inputs and outputs for controlling internal PlayStation 2 mods.

The main Button Interposer Board should stay focused on safe power/reset button behavior.

The I/O Expansion Board allows advanced builds to add extra control lines without making the base interposer board too large or too complicated.

---

## Purpose

The purpose of the I/O Expansion Board is to provide additional control points for internal PS2 mods and accessories.

Possible uses include:

- Control LEDs
- Control RGB lighting
- Enable or disable ChipSlayer
- Enable or disable a modchip control circuit
- Trigger Bluetooth audio pairing
- Enable or disable Bluetooth audio
- Reset BlueRetro
- Enable or disable BlueRetro channels
- Control accessory power enable lines
- Control chime or buzzer output
- Read extra switches or jumpers
- Read power-good or status signals
- Provide extra service/test points

This board is intended to make complex installs cleaner and more organized.

---

## Project Status

| Area | Status |
| --- | --- |
| Hardware concept | In planning |
| Schematic | Not started |
| PCB layout | Not started |
| MCU or I/O method | Not finalized |
| Communication method | Not finalized |
| Output driver style | Not finalized |
| Input protection | Not finalized |
| Bench testing | Not started |
| Console testing | Not started |
| Customer-ready status | Not ready |

This hardware should be treated as experimental until it is designed, assembled, and tested.

---

## Relationship To Button Butler

The Button Butler system may include multiple boards.

| Board | Purpose |
| --- | --- |
| Button Interposer Board | Handles PS2 power/reset button behavior and safety logic |
| I/O Expansion Board | Adds extra raw inputs and outputs |
| Touchscreen Hub | User interface and status display |
| Power Management Board | Accessory power switching and monitoring |
| Service Adapter | Debugging, testing, and firmware development |

The I/O Expansion Board should not replace the Button Interposer Board.

It should extend it.

---

## Main Design Goal

The main design goal is:

**Add useful control outputs and inputs without making the base Button Butler install messy.**

The I/O Expansion Board should provide a clean way to connect internal mods while keeping wiring organized and serviceable.

---

## Design Philosophy

The I/O Expansion Board should follow the same design philosophy as the rest of Button Butler:

1. Safe default states
2. Clear labeling
3. Simple wiring
4. Easy testing
5. Modular installation
6. Minimal shell modification
7. Serviceable layout
8. Low risk to the PS2 console
9. Expandable without becoming confusing
10. Useful for real FBD builds

The board should solve wiring and control problems, not create new ones.

---

## Why Use A Separate I/O Board?

The base Button Interposer Board should stay small and focused.

If every possible output is placed on the interposer, the board may become:

- Too large
- Too hard to install
- Too difficult to route
- Too complicated to test
- Too dependent on features not every build needs

A separate I/O Expansion Board allows advanced builds to add more features only when needed.

---

## Possible I/O Expansion Methods

The I/O Expansion Board could be designed in several ways.

| Method | Description | Notes |
| --- | --- | --- |
| PIC-based I/O board | Small MCU controls inputs and outputs | Flexible and FBD-friendly |
| I2C GPIO expander | GPIO expander controlled by another MCU | Simple but needs I2C bus |
| Shift register board | Adds many outputs using a few pins | Good for LEDs or simple outputs |
| Direct GPIO breakout | Breaks out spare pins from another controller | Simple but less flexible |
| MOSFET driver board | Focused on switching loads | Good for lights or power enables |
| Passive distribution board | Organizes wiring only | Simple but no intelligence |

The first version may be simplest as either a PIC-based board or a direct output driver board.

---

## PIC-Based I/O Expansion Option

A PIC-based I/O Expansion Board could act as a small local controller.

Possible advantages:

- Flexible firmware
- Can store local output states
- Can handle timing and pulses
- Can report input/output state
- Can run simple behavior even if touchscreen is asleep
- Can use UART or I2C for commands
- Can provide watchdog or safe mode behavior

Possible disadvantages:

- Needs firmware
- Needs programming pads
- Adds complexity
- Needs communication protocol
- More parts than a passive board

A PIC-based board may be a good fit if the I/O board is expected to do more than simple on/off outputs.

---

## I2C GPIO Expander Option

An I2C GPIO expander could provide more inputs and outputs with fewer MCU pins.

Possible advantages:

- Simple hardware
- Many GPIO pins available
- Easy to control from a main MCU
- Good for local board expansion

Possible disadvantages:

- I2C wiring can be sensitive to long runs
- Requires pull-up resistors
- Devices need unique addresses
- Less autonomous than a PIC-based board
- May not be ideal across longer internal harnesses

I2C may be useful for short board-to-board connections inside the same internal module stack.

---

## Shift Register Option

Shift registers can provide many outputs using a few control lines.

Possible advantages:

- Simple output expansion
- Good for LEDs
- Low cost
- Easy to cascade

Possible disadvantages:

- Mostly output-focused
- No intelligence
- No easy readback unless extra hardware is added
- Needs careful startup default handling
- Less ideal for safety-critical outputs

Shift registers may be better for LEDs than for mod-control outputs.

---

## Direct GPIO Breakout Option

A direct GPIO breakout simply routes spare MCU pins to connectors or pads.

Possible advantages:

- Simple
- Low cost
- No extra firmware
- Easy to understand

Possible disadvantages:

- Depends on the main controller having enough pins
- Wiring may become messy
- Less protection unless added
- No local intelligence
- Less modular

This may be useful for early prototypes but may not be ideal for a final product.

---

## MOSFET Driver Board Option

A MOSFET driver board focuses on switching loads.

Possible uses:

- RGB lighting
- LEDs
- Chime or buzzer
- Accessory enable lines
- Small relay coils
- Load switch enable lines

Possible advantages:

- Good for loads that need more current than an MCU pin can supply
- Keeps high-current switching away from the main logic board
- Easy to test with dummy loads

Possible disadvantages:

- Not a complete I/O system by itself
- Needs control signals from another board
- Needs proper default states and pull resistors

A MOSFET driver section may be part of the I/O Expansion Board.

---

## Possible Board Signals

The I/O Expansion Board may include several signal types.

| Signal Type | Example Signals |
| --- | --- |
| Logic power | `VCC`, `3V3`, `5V` |
| Ground | `GND` |
| Communication | `UART_TX`, `UART_RX`, `SDA`, `SCL` |
| Digital outputs | `OUT1`, `OUT2`, `OUT3`, `OUT4` |
| Digital inputs | `IN1`, `IN2`, `IN3`, `IN4` |
| Power control | `PWR_EN1`, `PWR_EN2` |
| Status feedback | `PGOOD1`, `PGOOD2`, `STATE1`, `STATE2` |
| Service | `SERVICE`, `RESET`, programming pins |
| Expansion | Spare pins or future connector |

The exact signals depend on the final architecture.

---

## Minimum Useful I/O Board

The minimum useful I/O Expansion Board could include:

| Signal | Purpose |
| --- | --- |
| `VCC` | Logic power |
| `GND` | Ground |
| `UART_TX` or `SDA` | Communication output |
| `UART_RX` or `SCL` | Communication input |
| `OUT1` | General mod-control output |
| `OUT2` | General mod-control output |
| `OUT3` | Lighting or accessory output |
| `OUT4` | Spare output |
| `IN1` | Status or service input |
| `IN2` | Status or service input |
| `RESET / MCLR` | Programming or reset |
| Programming pins | Firmware flashing if MCU-based |

This would be enough for early testing.

---

## Possible Full I/O Board

A more advanced board could include:

- 4 to 8 digital outputs
- 2 to 4 digital inputs
- UART or I2C communication
- Open-drain output options
- MOSFET output stages
- LED output support
- Chime or buzzer output
- Power enable outputs
- Status feedback inputs
- Service jumper
- Programming pads
- Test points
- Expansion connector

The board should not become larger than necessary.

---

## Output Use Cases

Possible output use cases:

| Output Target | Possible Function |
| --- | --- |
| ChipSlayer | Enable, disable, or toggle |
| Modchip control | Enable or disable modchip circuit |
| Bluetooth audio | Enable audio board |
| Bluetooth pairing | Trigger pairing mode |
| BlueRetro | Enable, disable, or reset |
| BlueRetro channels | Enable or disable specific channels |
| RGB lighting | On/off, brightness, or mode control |
| Status LED | Show mode or error state |
| Chime or buzzer | Play confirmation or warning tone |
| Video board power | Enable RetroGEM or ElectronAnalog power control circuit |
| Router power | Enable or reset internal router |
| Accessory rail | Enable a regulator or load switch |

Each output should have a safe default state.

---

## Input Use Cases

Possible input use cases:

| Input Source | Possible Function |
| --- | --- |
| Service jumper | Enter service mode or safe mode |
| Power-good signal | Confirm accessory power is stable |
| ChipSlayer state | Confirm active state |
| Modchip state | Confirm enabled or disabled state |
| Bluetooth audio status | Connected or pairing state if available |
| Router ready | Indicates internal router is ready |
| Video power-good | Confirms video board power is stable |
| Extra button | Installer or hidden control input |
| Lid switch copy | Alternate or redundant lid state |
| Temperature alert | Future thermal warning input |

Inputs should be buffered or protected if needed.

---

## Output Electrical Styles

Different outputs may need different electrical behavior.

| Output Style | Best Use |
| --- | --- |
| Push-pull GPIO | Same-voltage logic signals |
| Open-drain output | Pull-low control lines and safer shared signals |
| NPN transistor pull-down | Simple low-side pull or trigger |
| N-channel MOSFET | Low-side switching for LEDs or small loads |
| P-channel MOSFET | High-side switching with proper drive |
| Load switch enable | Accessory power control |
| Logic buffer | Clean signal output or isolation |
| Relay driver | Larger isolated loads if ever needed |

Open-drain style outputs may be the safest default for many mod-control lines.

---

## Open-Drain Outputs

Open-drain outputs only pull a line low.

They do not actively drive the line high.

Possible advantages:

- Safer for unknown pull-up voltage
- Useful for active-low inputs
- Good for simulating button presses
- Helps avoid drive conflict
- Reduces risk when sharing lines

Possible disadvantages:

- Requires a pull-up somewhere
- Slower rising edges depending on pull-up value
- Not suitable for every signal
- Active-high devices may need inversion

Open-drain behavior should be documented per output.

---

## Push-Pull Outputs

Push-pull outputs actively drive high and low.

Possible advantages:

- Clear logic levels
- Fast switching
- Simple for same-voltage devices

Possible disadvantages:

- Can fight another device
- Can damage 3.3V inputs if driven by 5V
- Can backfeed unpowered boards
- Less forgiving for unknown signals

Push-pull outputs should only be used when the voltage domain and target input are known.

---

## MOSFET Outputs

MOSFET outputs are useful for loads that need more current than an MCU pin can provide.

Possible MOSFET output targets:

- LEDs
- RGB lighting
- Active buzzer
- Relay coil
- Accessory enable circuit
- Small switched loads

MOSFET outputs should include:

- Gate resistor if needed
- Gate pull-down or pull-up
- Flyback diode for inductive loads
- Current rating margin
- Thermal consideration
- Clear active state

Do not drive loads directly from MCU pins unless the current is very small and verified.

---

## Load Switch Control

For accessory power, a load switch IC may be better than a simple transistor circuit.

Possible uses:

- Video board power
- Bluetooth audio power
- Internal router power
- Touchscreen backlight power
- RGB power rail

Possible advantages:

- Cleaner power switching
- Controlled turn-on
- Some parts include current limiting
- Some parts include thermal protection
- Smaller and more predictable than improvised circuits

The I/O Expansion Board may only provide the enable signal, while a Power Management Board handles the actual load switching.

---

## Input Protection

Inputs should be protected and defined.

Recommended input practices:

- Use pull-ups or pull-downs
- Avoid floating inputs
- Add series resistors where useful
- Use level shifting when needed
- Avoid feeding 5V into 3.3V-only inputs
- Consider filtering for noisy signals
- Use Schmitt-trigger buffers if needed
- Protect external or semi-external inputs from ESD if practical

Inputs should not affect the circuit they are monitoring.

---

## Input Pull Resistors

Each input should have a known default state.

Possible input defaults:

| Input | Suggested Default |
| --- | --- |
| Service jumper | Inactive |
| Status input | Defined inactive state |
| Power-good input | Low until valid |
| Router ready input | Low until ready |
| Bluetooth status | Unknown or inactive |
| Feedback input | Defined by target circuit |
| Spare input | Pull-up or pull-down installed |

The pull direction depends on the circuit being monitored.

---

## Communication Options

The I/O Expansion Board needs a way to communicate with the rest of Button Butler.

Possible communication options:

| Method | Notes |
| --- | --- |
| UART | Simple, good for board-to-board command/status |
| I2C | Good for local GPIO expansion and short wiring |
| SPI | Fast but may use more pins |
| Direct GPIO | Simple but not scalable |
| One-wire style control | Possible for simple commands but less flexible |
| Parallel control lines | Simple for a few outputs but wiring grows quickly |

UART is likely the easiest to debug.

I2C may be useful if the board is close to the touchscreen or main controller.

---

## UART Option

UART communication could allow the I/O Expansion Board to receive commands such as:

- Turn output on
- Turn output off
- Toggle output
- Pulse output
- Read input
- Report status
- Enter service mode
- Restore safe outputs

UART is useful because it can be tested with a USB-to-serial adapter.

Important UART notes:

- TX and RX must be crossed.
- Ground must be shared.
- Logic voltage must match.
- Multiple TX lines should not be tied together directly.
- Backfeeding through UART lines must be considered.

---

## I2C Option

I2C may be useful if the I/O Expansion Board is close to the main controller.

Possible advantages:

- Two wires for multiple devices
- Common GPIO expander support
- Easy addressing
- Good for short internal board connections

Possible concerns:

- Requires pull-up resistors
- Address conflicts
- Less ideal for long wires
- Noise sensitivity
- Voltage-level compatibility
- More difficult to debug than UART for some cases

I2C should be kept short and clean inside the console.

---

## Direct GPIO Option

Direct GPIO control may be simplest for early prototypes.

Possible advantages:

- No command parser
- Easy to test
- Low firmware complexity
- Good for one or two outputs

Possible disadvantages:

- Many wires for many signals
- Not very scalable
- No easy status reporting
- Depends on main controller pin count

Direct GPIO may be useful for the first bench tests.

---

## Suggested Communication Direction

A practical development path:

1. Start with direct GPIO or UART-controlled outputs.
2. Test output defaults and switching.
3. Add input reading.
4. Add simple status reporting.
5. Add touchscreen support later.
6. Consider I2C only if the board is close and the bus stays short.

Do not overcomplicate the first revision.

---

## Output Default States

Every output must have a default state.

Example default table:

| Output | Suggested Default | Notes |
| --- | --- | --- |
| `OUT1` | Inactive | General mod-control output |
| `OUT2` | Inactive | General mod-control output |
| `OUT3` | Inactive | Lighting or accessory output |
| `OUT4` | Inactive | Spare output |
| `BT_PAIR` | Inactive | Should never trigger at startup |
| `BLUERETRO_RESET` | Inactive | Should never pulse at startup |
| `RGB_EN` | Off | Avoid extra current draw |
| `VIDEO_EN` | Build-profile dependent | Avoid blanking display unexpectedly |
| `BUZZER` | Off | Avoid unwanted sound |

Defaults should be enforced by hardware and firmware where possible.

---

## Startup Behavior

The I/O Expansion Board should start safely.

Recommended startup behavior:

1. Power applied
2. Outputs held inactive by hardware defaults
3. MCU initializes if used
4. GPIO directions are configured
5. Communication starts
6. Board reports ready state if applicable
7. Outputs remain inactive until commanded
8. Optional saved profile loads only after safe defaults are active

No output should pulse or glitch during startup.

---

## Shutdown Behavior

Shutdown behavior should also be safe.

Recommended shutdown behavior:

- Outputs return to inactive states where possible
- No new commands are accepted during unstable power
- Communication lines do not backfeed unpowered boards
- Accessory enable lines do not float
- Power-good states are no longer trusted once power drops
- Stored settings are not written during unstable power

Hardware defaults matter during shutdown.

---

## Safe Mode

The I/O Expansion Board should support a safe condition.

Safe mode behavior may include:

- Turn noncritical outputs off
- Disable RGB lighting
- Disable buzzer/chime
- Block modchip state changes
- Keep ChipSlayer in a defined state
- Keep accessory enables in safe states
- Allow status reads
- Allow service commands only if unlocked

Safe mode should be useful for recovery and testing.

---

## Service Mode

Service mode may allow manual testing.

Possible service functions:

- Toggle each output
- Pulse each output
- Read each input
- Show firmware version
- Show hardware revision
- Test LEDs
- Test chime output
- Restore defaults
- Force all outputs off
- Report active profile

Service mode should be protected if used in customer builds.

---

## Status LED

A local status LED may be useful.

Possible LED meanings:

| LED Pattern | Meaning |
| --- | --- |
| Off | No power or inactive |
| Solid on | Board powered |
| Slow blink | Firmware running |
| Fast blink | Service mode |
| Double flash | Command received |
| Triple flash | Error |
| Breathing | Standby or idle |

LED behavior should be optional.

---

## Chime Or Buzzer Output

The I/O Expansion Board may include or drive a chime/buzzer output.

Possible uses:

- Button accepted tone
- Button rejected tone
- Pairing mode tone
- Service mode tone
- Startup chime
- Shutdown chime
- Error warning

Possible buzzer types:

- Active buzzer
- Passive buzzer
- Small audio trigger module
- Output to another audio circuit

Chime output should be optional and disabled by default until tested.

---

## RGB Lighting Support

The I/O Expansion Board may control lighting.

Possible lighting support:

- Simple on/off output
- PWM brightness control
- RGB enable line
- Addressable LED output in future versions
- Status lighting output
- Service mode lighting

Lighting should not interfere with console operation.

RGB current draw should be measured.

---

## ChipSlayer Support

ChipSlayer is a strong early use case.

Possible ChipSlayer control:

- Enable output
- Disable output
- Toggle output
- State feedback input if available
- Status LED
- Touchscreen command support
- Lid-open button shortcut support through interposer

ChipSlayer output should have a clearly defined safe default.

---

## Modchip Support

Modchip control may be supported later.

Possible modchip control:

- Enable output
- Disable output
- Next-boot state control
- Service-mode override
- State feedback input if available
- Touchscreen status display

Modchip control should be protected because it affects boot behavior.

---

## Bluetooth Audio Support

Bluetooth audio control is a practical use case.

Possible Bluetooth audio control:

- Audio enable output
- Pairing pulse output
- Reset pulse output
- Status input if available
- Touchscreen audio page support

Pairing and reset should usually be pulse outputs.

They should not trigger on startup.

---

## BlueRetro Support

BlueRetro support may be simple at first.

Possible BlueRetro control:

- Enable output
- Reset pulse output
- Channel enable outputs if supported
- Status input or UART later
- Touchscreen controller page support

BlueRetro control should be tested with both wired and wireless controllers.

---

## Video Board Support

The I/O Expansion Board may provide enable signals for video power control.

Possible targets:

- RetroGEM
- ElectronAnalog
- Other internal HDMI boards
- Video accessory regulator
- Load switch enable

Video power control should be protected because it can blank the display or affect video stability.

---

## Internal Router Support

The I/O Expansion Board may control an internal router or network module.

Possible router control:

- Power enable output
- Reset output
- Ready input
- Power-good input
- Service UART handoff in future versions

Router power sequencing may matter if the PS2 depends on network loading.

---

## Power Domains

The I/O Expansion Board may connect to several power domains.

Possible power domains:

| Domain | Possible Use |
| --- | --- |
| 3.3V logic | MCU, GPIO, UART |
| 5V accessory | LEDs, Bluetooth audio, small modules |
| PS2 standby power | Always-on control logic |
| PS2 switched power | Console-on accessories |
| Accessory rail | Power controlled by another board |
| External programmer power | Bench testing or firmware flashing |

Power domains should be labeled clearly.

---

## Backfeeding Risks

Backfeeding is a major risk.

Possible backfeeding paths:

- UART TX/RX
- I2C pull-ups
- Output lines
- Input pull-ups
- Status lines
- LED paths
- Programming adapter
- Touchscreen connection
- Accessory enable lines

Backfeeding can partially power boards, create strange behavior, or damage parts.

---

## Backfeeding Prevention

Ways to reduce backfeeding:

- Use series resistors
- Use open-drain outputs
- Use high-impedance defaults
- Use level shifters
- Avoid pull-ups to unpowered rails
- Power communicating devices from compatible rails
- Use MOSFET isolation where needed
- Use load switches with reverse-current blocking if needed
- Test for voltage on unpowered rails

Backfeeding should be checked during bench testing.

---

## Logic Level Considerations

The board may need to interact with both 3.3V and 5V logic.

Important rules:

- Confirm voltage level before connecting signals.
- Do not feed 5V into 3.3V-only inputs.
- Use level shifting if needed.
- Use open-drain outputs when target pull-up voltage is unknown.
- Keep UART voltage compatible.
- Label voltage domains on the PCB.

Logic-level mistakes can damage hardware.

---

## Current Handling

The board should not route high current unless designed for it.

Important current rules:

- MCU pins should not directly drive loads.
- LEDs need current limiting.
- RGB lighting may need MOSFET drivers.
- Buzzer or relay outputs may need drivers.
- Accessory power should use proper traces or separate power board.
- High-current outputs need wider traces.
- Heat must be considered.

If the board only provides enable signals, current handling is much easier.

---

## Grounding

The I/O Expansion Board needs a good ground reference.

Grounding notes:

- UART and I/O signals need shared ground.
- High-current load grounds should be routed carefully.
- Noisy loads should not disturb button or UART signals.
- Use enough ground connections.
- Label ground pads clearly.
- Avoid using random shield contact as the only ground.

Bad grounding can cause random behavior.

---

## Decoupling

If the board has active ICs, decoupling is required.

Typical decoupling:

- 0.1 uF capacitor near each IC power pin
- Bulk capacitor if switching loads or LEDs
- Extra filtering for noisy accessory power if needed

Final values depend on the selected components.

---

## Test Points

Useful test points:

- `VCC`
- `GND`
- `OUT1`
- `OUT2`
- `OUT3`
- `OUT4`
- `IN1`
- `IN2`
- `UART_TX`
- `UART_RX`
- `SDA`
- `SCL`
- `RESET`
- `PGD`
- `PGC`
- `SERVICE`

Test points make debugging much easier.

---

## Programming Pads

If the board uses a PIC or MCU, programming access is needed.

Possible programming signals:

| Signal | Purpose |
| --- | --- |
| `VPP / MCLR` | Programming voltage or reset |
| `VDD` | Target voltage |
| `VSS` | Ground |
| `PGD` | Programming data |
| `PGC` | Programming clock |

Programming pads should be accessible during development.

Final product versions may use smaller pads.

---

## Connector Planning

The board may use connectors for cleaner installation.

Possible connectors:

| Connector | Signals |
| --- | --- |
| Main control connector | Power, ground, UART or I2C |
| Output connector | `OUT1` through `OUT4` |
| Input connector | `IN1` through `IN4` |
| Lighting connector | RGB or LED control |
| Power enable connector | Accessory enable outputs |
| Service connector | UART, reset, programming, service input |
| Expansion connector | Future signals |

Connector pinouts should be documented early.

---

## Silkscreen Goals

The silkscreen should make installation easier.

Important labels:

- Board name
- Revision
- `VCC`
- `GND`
- `OUT1`
- `OUT2`
- `OUT3`
- `OUT4`
- `IN1`
- `IN2`
- `TX`
- `RX`
- `SDA`
- `SCL`
- `SERVICE`
- `RESET`
- Pin 1 markers
- Voltage warnings

Do not remove useful labels just for artwork.

---

## Board Size Goals

The board should be small but still usable.

Board size goals:

- Fit inside PS2 Slim builds
- Fit inside Ultra Slim builds
- Avoid RF shield conflicts
- Leave room for wire strain relief
- Provide hand-solderable pads
- Provide readable labels
- Provide test points
- Avoid making connectors impossible to access

A board that is too small can become hard to install.

---

## Board Thickness

Possible board thicknesses:

| Thickness | Notes |
| --- | --- |
| 1.6 mm | Standard and strong |
| 1.0 mm | Thinner and still manageable |
| 0.8 mm | Useful for tight PS2 installs |
| Flex PCB | Future option for harness/interposer use |

For early prototypes, 0.8 mm or 1.0 mm may be useful if space is tight.

---

## Mounting Goals

The board should be easy to mount.

Possible mounting methods:

- Double-sided tape for prototypes
- Adhesive standoffs
- Printed bracket
- RF shield mounting area
- Custom carrier plate
- Screw and standoff if space allows
- Flex cable attachment in future versions

Final builds should avoid weak mounting if wires may pull on the board.

---

## Insulation

The board must be insulated from metal shields and other boards.

Recommended insulation methods:

- Kapton tape
- Fish paper
- Printed bracket
- Standoffs
- Solder mask coverage
- Careful pad placement
- Avoid exposed pads facing metal

The board should not short to the RF shield.

---

## Wire Routing

Wire routing should be planned.

Wire routing goals:

- Short wires
- Clear wire colors
- Avoid pinch points
- Avoid screw posts
- Avoid hot areas
- Avoid noisy power wiring
- Keep high-current wires separate from signals
- Add strain relief
- Keep shell closure easy
- Keep service access possible

The I/O board should make wiring cleaner, not harder.

---

## Suggested Wire Colors

Suggested wire colors:

| Color | Suggested Use |
| --- | --- |
| Black | Ground |
| Red | Main power |
| Orange | 3.3V |
| Yellow | 5V or accessory power |
| White | UART TX or signal |
| Green | UART RX or signal |
| Blue | Output control |
| Purple | Service or special function |
| Gray | Status input |
| Brown | Lid or button-related signal |

The actual colors used should be documented per build.

---

## Noise Considerations

The I/O Expansion Board may switch loads or connect to noisy devices.

Possible noise sources:

- RGB lighting
- Buzzers
- Relays
- Switching regulators
- Internal routers
- Video boards
- Long wires
- Wireless modules

Noise reduction methods:

- Decoupling capacitors
- Good grounding
- Series resistors
- Short wires
- Pull-ups or pull-downs
- Avoid routing signals near high-current wires
- Use MOSFET drivers for loads
- Separate noisy loads from UART lines

Noise should be considered before final layout.

---

## Bench Testing Plan

Before installing in a console, bench test the I/O Expansion Board.

Bench tests:

1. Visual inspection
2. Continuity check
3. Power rail check
4. Current draw check
5. MCU programming check if used
6. Communication test
7. Output default state test
8. Output toggle test
9. Output pulse test
10. Input state test
11. Service mode test
12. Startup glitch test
13. Shutdown behavior test
14. Backfeeding check

Do not connect to important PS2 signals until basic tests pass.

---

## Console Testing Plan

After bench testing, test in a real console.

Console tests:

1. Confirm console works before install
2. Connect only power and ground first
3. Confirm no abnormal current draw
4. Connect communication lines
5. Confirm UART or I2C works
6. Connect one output to a safe target
7. Test output state
8. Test output default after power cycle
9. Connect one input if needed
10. Test shell closure
11. Test noise or interference
12. Test during gameplay if relevant
13. Test repeated startup and shutdown

Add one function at a time.

---

## Test Data To Save

Save useful test data in:

`Test-Data/`

Useful test data:

- Output voltage states
- Input voltage states
- Current draw
- UART logs
- I2C logs if used
- Startup behavior notes
- Shutdown behavior notes
- Backfeeding measurements
- Heat measurements
- Console model tested
- Board revision
- Firmware version

Good test data helps future board revisions.

---

## Suggested Folder Structure

| Path | Purpose |
| --- | --- |
| `KiCad/` | KiCad project files |
| `Schematics/` | Exported schematic PDFs or images |
| `Images/` | Board renders, layout screenshots, install photos |
| `Gerbers/` | Prototype Gerber exports if shared |
| `BOM/` | I/O Expansion Board BOM files |
| `README.md` | Hardware overview and planning notes |

Additional folders can be added later.

---

## File Naming

Suggested file naming:

| File | Purpose |
| --- | --- |
| `FBD-PS2-Button-Butler-IO-Expansion.kicad_pro` | KiCad project |
| `FBD-PS2-Button-Butler-IO-Expansion-v0.1.pdf` | Schematic export |
| `FBD-PS2-Button-Butler-IO-Expansion-v0.1-Gerbers.zip` | Prototype Gerbers |
| `FBD-PS2-Button-Butler-IO-Expansion-v0.1-BOM.csv` | BOM |
| `FBD-PS2-Button-Butler-IO-Expansion-v0.1-Top.png` | Top render |
| `FBD-PS2-Button-Butler-IO-Expansion-v0.1-Bottom.png` | Bottom render |

Use consistent names to make revisions easier to track.

---

## Revision Naming

Suggested revision naming:

| Revision | Meaning |
| --- | --- |
| `v0.0` | Concept only |
| `v0.1` | First schematic prototype |
| `v0.2` | First PCB prototype |
| `v0.3` | Bench-tested revision |
| `v0.4` | Console-tested revision |
| `v0.5` | Improved prototype |
| `v1.0` | First stable release candidate |

Do not call a revision stable until it has been tested.

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
- Current rating if relevant
- Tolerance
- Notes
- Substitute parts if available

For prototypes, parts should be easy to solder and source.

---

## Component Selection Goals

Component selection should prioritize:

- Availability
- Practical packages
- Clear datasheets
- Common values
- Correct voltage ratings
- Correct current ratings
- Low heat where possible
- Easy soldering for prototypes
- Reliable suppliers
- Reasonable cost

Avoid hard-to-source parts unless they solve an important problem.

---

## Manufacturing Goals

Future manufacturing goals:

- Stable PCB revision
- Clean BOM
- Repeatable assembly
- Clear connector labels
- Programming method if MCU-based
- Test jig support
- QA checklist
- Known safe defaults
- Installation notes
- Supported build profiles

Manufacturing should only be planned after prototypes are stable.

---

## QA Checklist Ideas

Possible QA checklist items:

- Visual inspection
- Check for shorts
- Confirm correct voltage
- Confirm current draw
- Confirm firmware flashing if MCU-based
- Confirm communication
- Confirm all outputs default inactive
- Confirm each output works
- Confirm each input reads correctly
- Confirm service mode
- Confirm safe mode
- Confirm no startup glitches
- Confirm no backfeeding
- Record firmware version
- Record board revision

A detailed QA checklist should eventually live in:

`Manufacturing/QA-Checklist.md`

---

## Known Issues

Current known issues:

- Final I/O expansion method is not selected.
- MCU or GPIO expander choice is not finalized.
- Communication method is not finalized.
- Output count is not finalized.
- Input count is not finalized.
- Output driver style is not finalized.
- Logic voltage is not finalized.
- Connector style is not finalized.
- No prototype has been bench tested yet.
- No prototype has been console tested yet.

This section should be updated as the design develops.

---

## Open Questions

Open hardware questions:

- Should the first I/O Expansion Board be PIC-based?
- Should it use UART, I2C, or direct GPIO?
- How many outputs are actually needed for the first prototype?
- How many inputs are useful without making the board too large?
- Should outputs be mostly open-drain?
- Should MOSFET drivers be included on the board?
- Should lighting control be included or left to a separate board?
- Should accessory power switching be included or left to a Power Management Board?
- Should the board have a service jumper?
- Should it store output states?
- Should it report input/output states to the touchscreen?
- What connector style is best for repeated installs?
- What board size fits best in Slim and Ultra Slim builds?
- Should the first version be a simple driver board instead of a smart MCU board?

These questions should be answered through prototype testing.

---

## Risks

Possible risks:

- Too many outputs make the board too large
- Too many wires make installs messy
- Outputs glitch during startup
- Outputs float into unsafe states
- Wrong voltage level damages another board
- Backfeeding through I/O lines
- RGB or accessory loads draw too much current
- UART or I2C communication becomes unreliable
- Service mode is left unlocked
- Modchip control changes boot behavior unexpectedly
- Board placement causes shell fitment problems
- Board shorts to RF shield
- Feature creep delays the core Button Butler project

The safest path is to start with a small number of outputs and expand later.

---

## Recommended Development Order

Recommended development order:

1. Decide if first board is smart or passive
2. Define minimum output count
3. Define minimum input count
4. Choose communication method
5. Choose logic voltage
6. Design one safe output circuit
7. Design one safe input circuit
8. Add power and ground
9. Add programming/debug access if MCU-based
10. Build bench prototype
11. Test with LEDs or dummy loads
12. Test communication
13. Test default states
14. Add one real mod-control target
15. Document results
16. Revise board

Do not connect several important mods at once during first testing.

---

## Minimum First Prototype

A good first prototype could include:

- Power input
- Ground
- Communication input
- Two safe outputs
- One or two inputs
- Status LED
- Test points
- Optional programming pads if MCU-based

This is enough to test the expansion concept without overbuilding.

---

## Summary

The I/O Expansion Board is an optional expansion board for the FBD PS2 Button Butler project.

Its job is to provide extra inputs and outputs for advanced PS2 mods while keeping the main Button Interposer Board focused on safe power/reset button behavior.

The board may eventually control ChipSlayer, Bluetooth audio, BlueRetro, RGB lighting, modchip control circuits, accessory power enables, chimes, status LEDs, and other internal features.

The first revision should stay simple, safe, and testable.

Advanced features can be added after the core Button Butler interposer and basic I/O behavior are proven.
