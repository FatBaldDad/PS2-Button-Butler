# Power Management Hardware

## Overview

This folder contains hardware planning notes, schematic ideas, PCB notes, and future design files for the **Power Management** section of the **FBD PS2 Button Butler** project.

The Power Management hardware is an optional expansion area for Button Butler.

Its purpose is to help control internal accessory power inside advanced PlayStation 2 builds.

The goal is not to replace the PS2 power system.

The goal is to safely manage added internal mods and accessories so the console can run cleaner, cooler, and more predictably.

---

## Purpose

The purpose of the Power Management hardware is to provide controlled power enable, disable, sequencing, and monitoring for internal PS2 accessories.

Possible controlled devices include:

- RetroGEM
- ElectronAnalog
- BlueRetro
- Bluetooth audio board
- SD2PSX support hardware
- RGB lighting
- Touchscreen backlight
- Internal router or network module
- Chime or buzzer circuit
- I/O Expansion Board
- Future LazyrSavre-style diagnostic hardware
- Other internal FBD accessory boards

Power Management should help reduce heat, reduce unnecessary current draw, and make complex builds easier to control.

---

## Project Status

| Area | Status |
| --- | --- |
| Hardware concept | In planning |
| Schematic | Not started |
| PCB layout | Not started |
| Load switch selection | Not finalized |
| Regulator selection | Not finalized |
| Power domains | Not finalized |
| Accessory targets | Concept planning |
| Bench testing | Not started |
| Console testing | Not started |
| Customer-ready status | Not ready |

This hardware should be treated as experimental until it is designed, tested, and proven in real PS2 builds.

---

## Main Design Goal

The main design goal is:

**Control internal accessory power safely without interfering with normal PS2 operation.**

Power Management should not make the console harder to use.

It should not create random shutdowns, video loss, memory card problems, or unstable startup behavior.

---

## Relationship To Button Butler

The Power Management hardware is part of the larger Button Butler system.

| Board | Purpose |
| --- | --- |
| Button Interposer Board | Handles PS2 power/reset button behavior and safety logic |
| I/O Expansion Board | Provides extra inputs and outputs |
| Touchscreen Hub | Provides user interface and sends power-control requests |
| Power Management Board | Controls accessory power rails or enable signals |
| Service Adapter | Allows testing, UART access, and debugging |

The Button Interposer Board should remain responsible for critical PS2 button behavior.

The Power Management hardware should only control accessory power unless a future design specifically proves safe control of larger power paths.

---

## Important Design Rule

Do not directly modify or interrupt the main PS2 power system until the circuit is fully understood and tested.

Early Power Management should focus on:

- Accessory enable lines
- Accessory regulators
- Small internal modules
- Video board power control
- RGB lighting power
- Bluetooth audio power
- Internal router power
- Touchscreen/backlight power

The main PS2 motherboard power path should be treated carefully.

---

## Power Management Philosophy

Power Management should follow these rules:

1. Keep the PS2 console safe.
2. Use safe default states.
3. Avoid backfeeding.
4. Avoid floating enable lines.
5. Avoid unnecessary automatic behavior early on.
6. Measure current before switching loads.
7. Use proper load switches or regulators.
8. Keep high-current paths away from logic when possible.
9. Keep the system serviceable.
10. Test every power-controlled device individually.

Power Management should solve heat and control problems, not create new reliability problems.

---

## Why Power Management Matters

Advanced PS2 builds can contain several internal mods.

Each added board can increase:

- Current draw
- Heat
- Wiring complexity
- Startup timing issues
- Shutdown timing issues
- Backfeeding risk
- Noise on power rails
- Customer confusion

Power Management can help by allowing accessories to be powered only when needed.

---

## Possible Power Management Levels

Power Management can be added in levels.

| Level | Description |
| --- | --- |
| Level 1 | No power switching, only documentation and measurement |
| Level 2 | Simple enable outputs to other power circuits |
| Level 3 | MOSFET or load-switch controlled accessory power |
| Level 4 | Touchscreen-controlled accessory power |
| Level 5 | Power-good monitoring and fault reporting |
| Level 6 | Automatic sequencing, idle behavior, and diagnostics |

The first hardware should start with simple enable outputs and measurements.

---

## Possible Power Targets

Button Butler may eventually manage power for several internal devices.

| Target | Possible Power Management Use |
| --- | --- |
| RetroGEM | Enable, disable, or reduce idle heat |
| ElectronAnalog | Enable or disable internal HDMI board power |
| BlueRetro | Enable, disable, reset, or keep always powered depending on build |
| Bluetooth audio | Enable, disable, reset, or trigger pairing support |
| RGB lighting | Turn lighting power on/off |
| Touchscreen | Sleep, wake, dim, or control backlight power |
| Internal router | Enable, disable, reset, or wait for ready state |
| SD2PSX support | Monitor only at first, avoid risky power switching |
| Chime circuit | Enable or disable sound feedback |
| I/O Expansion | Power or enable extra output hardware |
| Future diagnostic boards | Enable only during service or diagnostics |

Not every build will use every target.

---

## Accessory Enable Versus Power Switching

There are two main ways to control a device.

| Method | Description |
| --- | --- |
| Enable control | Button Butler controls an enable pin on another circuit |
| Power switching | Button Butler or a power board physically switches power to the device |

Enable control is usually safer and easier.

Power switching is more powerful but requires more careful hardware design.

---

## Accessory Enable Control

An accessory enable signal does not carry the full load current.

It only tells another circuit to turn on or off.

Possible enable targets:

- Regulator enable pin
- Load switch enable pin
- Power board enable pin
- Bluetooth audio enable pin
- RGB controller enable pin
- Router power enable pin
- Video board power enable circuit

This is a good starting point for early prototypes.

---

## Direct Power Switching

Direct power switching means the board actually controls power going to the accessory.

Possible switching methods:

- Dedicated load switch IC
- P-channel MOSFET high-side switch
- N-channel MOSFET low-side switch
- Regulator enable pin
- Relay
- Solid-state relay
- Power mux or ideal diode circuit

Direct switching should only be used after the load current, startup behavior, and backfeeding risks are understood.

---

## Recommended Early Approach

The recommended early approach is:

1. Measure the accessory current.
2. Identify whether the accessory has an enable pin.
3. Use the enable pin if available.
4. If no enable pin exists, test a load switch or MOSFET circuit on the bench.
5. Check for backfeeding through signal lines.
6. Test startup and shutdown behavior.
7. Only then install in a console.

Do not start by switching several accessories at once.

---

## Power Domains

Button Butler may interact with several power domains.

| Power Domain | Possible Use |
| --- | --- |
| PS2 standby power | Always-on logic, button monitoring, wake behavior |
| PS2 switched power | Console-on accessory logic |
| 3.3V logic | MCU, UART, low-power control signals |
| 5V accessory rail | Bluetooth audio, RGB, small modules |
| 8.5V / 9V input | PS2 Slim input power area |
| USB-C PD rail | Optional external or alternate power source |
| Video board rail | RetroGEM or ElectronAnalog power |
| Router rail | Internal router or network module power |
| Display rail | Touchscreen and backlight power |

Every power domain should be labeled clearly.

---

## Standby Power

Some parts of Button Butler may need standby power.

Possible standby-powered sections:

- Button Interposer Board
- Wake control logic
- Low-power MCU
- Touchscreen wake circuit
- BlueRetro power-on support if used
- Power state monitor

Standby current should be kept low.

The board should not draw unnecessary current when the console is off.

---

## Switched Power

Switched power is available only when the console is on or active.

Possible switched-power devices:

- RGB lighting
- Video board
- Bluetooth audio
- Chime circuit
- Touchscreen backlight
- Internal router depending on build
- Noncritical accessory circuits

Switched power is useful for reducing heat and power draw when the console is off.

---

## 3.3V Logic Rail

The 3.3V rail may be used for:

- MCU logic
- UART communication
- Control outputs
- Status inputs
- Touchscreen controller logic
- I/O expansion logic

Important notes:

- Confirm every connected device is 3.3V compatible.
- Do not feed 5V signals into 3.3V-only inputs.
- Use level shifting when needed.
- Add decoupling capacitors near ICs.
- Avoid backfeeding unpowered boards through signal lines.

---

## 5V Accessory Rail

The 5V rail may be used for:

- Bluetooth audio boards
- Some touchscreen modules
- RGB lighting
- Small support modules
- Chime or buzzer modules
- USB-related accessories

Important notes:

- Measure current draw.
- Use proper regulators.
- Avoid overheating linear regulators.
- Add filtering for noisy accessories.
- Keep high-current wiring short and properly sized.

---

## PS2 8.5V / 9V Input Area

PS2 Slim consoles commonly use an external DC input around this range.

Possible uses:

- Feeding accessory buck regulators
- Feeding a PowerOR-style power system
- Feeding a dedicated internal accessory supply

Important notes:

- Do not modify the main PS2 input path casually.
- Confirm the actual voltage under load.
- Use efficient converters for lower-voltage accessories.
- Add protection and filtering where needed.
- Measure heat and current draw.

Button Butler should not directly switch the main PS2 input in early versions.

---

## USB-C PD Power Concepts

Some FBD builds may include USB-C PD power or PowerOR-style power hardware.

Possible uses:

- Alternate PS2 power input
- Accessory power source
- Bench testing source
- Compact build power source

Important notes:

- Confirm PD trigger voltage.
- Confirm charger current capability.
- Confirm buck converter output.
- Do not mix power sources without proper OR-ing.
- Avoid backfeeding the OEM PSU path.
- Keep Button Butler control logic separate from unsafe power paths.

Button Butler may request or monitor power states, but the actual power OR-ing should be handled by a dedicated power design.

---

## High-Side Switching

High-side switching controls the positive supply to a load.

Possible advantages:

- Device ground remains connected
- Better for modules with signal connections
- Avoids floating the device ground
- Often cleaner for video, audio, and logic modules

Possible disadvantages:

- Circuit can be more complex
- P-channel MOSFET gate drive needs care
- Load switch IC may be needed
- Inrush current should be considered

High-side switching is usually preferred for accessory modules.

---

## Low-Side Switching

Low-side switching controls the ground return path.

Possible advantages:

- Simple with N-channel MOSFETs
- Easy to drive from logic
- Good for LEDs, buzzers, and simple loads

Possible disadvantages:

- Device ground floats when off
- Signal lines may backfeed the device
- Not ideal for UART-connected modules
- Not ideal for audio or video boards
- Can cause strange behavior in connected modules

Low-side switching should be used mainly for simple loads.

---

## Load Switch ICs

A load switch IC is often the cleanest option for accessory power control.

Possible advantages:

- Simple enable input
- Controlled turn-on behavior
- Some parts include current limiting
- Some parts include thermal protection
- Some parts include reverse current blocking
- Small package
- Predictable behavior

Possible uses:

- Video board power
- Bluetooth audio power
- Router power
- Touchscreen backlight power
- RGB power rail
- Accessory support boards

Load switch ICs should be considered for final hardware.

---

## Regulator Enable Control

If an accessory has its own regulator, the regulator enable pin may be the best control point.

Possible advantages:

- Low-current control signal
- Does not require switching full load current on Button Butler
- Cleaner layout
- Lower heat on the control board
- Predictable startup behavior if the regulator is designed well

Possible disadvantages:

- Not every module exposes an enable pin
- Signal lines can still backfeed the module
- Startup timing must be tested
- Some regulators may have slow turn-on behavior

Regulator enable control is preferred when available.

---

## MOSFET Switching

MOSFETs may be used for accessory control.

Possible uses:

- RGB lighting
- Small LED loads
- Buzzer or chime output
- Simple accessory enable circuits
- Power switching with proper design

MOSFET switching should include:

- Gate resistor if needed
- Gate pull-up or pull-down
- Correct voltage rating
- Correct current rating
- Thermal margin
- Flyback protection for inductive loads
- Safe startup default state

Do not switch unknown loads without testing.

---

## Relay Control

Relays are usually not the first choice for compact Button Butler power control.

Possible advantages:

- Electrical isolation
- Simple contact behavior
- Easy to understand

Possible disadvantages:

- Larger size
- Coil current
- Mechanical wear
- Audible click
- Flyback protection required
- Not ideal for tiny internal PS2 installs

Relays may be useful in special cases but should not be the default approach.

---

## Output Enable Signals

Power Management may use enable signals from the Button Interposer Board or I/O Expansion Board.

Possible enable signals:

| Signal | Purpose |
| --- | --- |
| `VID_EN` | Enable video board power |
| `BT_EN` | Enable Bluetooth audio power |
| `RGB_EN` | Enable lighting power |
| `ROUTER_EN` | Enable internal router power |
| `AUX_EN1` | Spare accessory power enable |
| `AUX_EN2` | Spare accessory power enable |
| `TS_BL_EN` | Touchscreen backlight enable |
| `CHIME_EN` | Enable chime or buzzer circuit |

Each enable signal needs a defined default state.

---

## Power-Good Signals

Power-good signals can confirm that a rail is stable.

Possible power-good inputs:

| Signal | Purpose |
| --- | --- |
| `VID_PG` | Video board power-good |
| `BT_PG` | Bluetooth audio power-good |
| `ROUTER_PG` | Router power rail valid |
| `AUX_PG1` | Accessory rail 1 valid |
| `AUX_PG2` | Accessory rail 2 valid |
| `3V3_PG` | Logic rail valid |
| `5V_PG` | Accessory 5V rail valid |

Power-good signals are useful but should not be trusted until tested.

---

## Current Monitoring

Future versions may include current monitoring.

Possible uses:

- Detect accessory overcurrent
- Track power draw
- Detect failed boards
- Report power usage on touchscreen
- Warn about excessive current
- Help with heat testing

Current monitoring is useful but not required for the first version.

It may add cost, complexity, and board space.

---

## Temperature Monitoring

Future versions may include temperature monitoring.

Possible targets:

- Video board area
- Internal router area
- Power regulator area
- Touchscreen area
- General internal shell temperature

Possible uses:

- Display temperature on touchscreen
- Warn about overheating
- Disable noncritical accessories if too hot
- Log heat test data

Temperature monitoring should be added only after basic power control is stable.

---

## RetroGEM Power Management

RetroGEM may be a future power management target in premium builds.

Possible goals:

- Power RetroGEM only when needed
- Reduce heat
- Add manual touchscreen control
- Add auto power-saving mode
- Show power state
- Show fault or power-good state if available

Important concerns:

- HDMI handshake behavior
- Video loss if powered off
- Startup timing
- Backfeeding through video or control lines
- User confusion if the display goes blank
- Proper shutdown and startup behavior

RetroGEM power management should be tested carefully before customer use.

---

## ElectronAnalog Power Management

ElectronAnalog is another possible power management target.

Possible goals:

- Enable or disable internal HDMI board power
- Reduce idle heat
- Save power
- Add touchscreen control
- Add service-mode testing
- Show power state

Important concerns:

- Video dropouts
- HDMI display behavior
- Startup timing
- Power rail noise
- Backfeeding
- Heat reduction benefit
- User-facing clarity

ElectronAnalog power control should be tested on real hardware before being considered stable.

---

## BlueRetro Power Management

BlueRetro power behavior depends on the build.

Possible strategies:

| Strategy | Use Case |
| --- | --- |
| Always powered | Needed if controller wake or standby pairing is desired |
| Console-on only | Saves power but may limit wake behavior |
| Enable controlled | Allows touchscreen or Button Butler to disable it |
| Reset controlled | Allows recovery without opening the console |
| Channel controlled | Allows specific controller channels to be disabled |

BlueRetro should be tested with both wired and wireless controllers.

---

## Bluetooth Audio Power Management

Bluetooth audio is a good power management target.

Possible controls:

- Audio board enable
- Audio board disable
- Pairing trigger
- Reset pulse
- Power state display
- Auto-disable when unused

Important concerns:

- Audio pops
- Reconnect delay
- Pairing state
- Ground noise
- Power rail noise
- Backfeeding through audio or control lines

Bluetooth audio power should be tested with actual audio output.

---

## SD2PSX Power Management

SD2PSX should be handled conservatively.

Recommended early approach:

- Monitor status only
- Avoid power switching
- Avoid reset control unless proven safe
- Avoid mode switching during gameplay
- Avoid anything that could interrupt saves

Important concerns:

- Save corruption
- OPL or game compatibility
- Memory card stability
- Power loss during writes
- User confusion

Power management for SD2PSX should not be added until behavior is fully understood.

---

## RGB Lighting Power Management

RGB lighting is a practical power management target.

Possible controls:

- Lighting on
- Lighting off
- Brightness control
- Mode control
- Auto-off during gameplay
- Service mode lighting
- Error state lighting

Important concerns:

- Current draw
- Heat
- Noise
- Wire routing
- Regulator capacity
- LED current limiting

RGB current can add up quickly.

Measure it.

---

## Touchscreen Power Management

The touchscreen may need its own power behavior.

Possible controls:

- Backlight enable
- Display sleep
- Display wake
- Brightness control
- Full power off if supported
- Wake on button or touch
- Wake on error

Important concerns:

- Standby current
- Startup time
- Display wear
- Heat
- Touch wake reliability
- UART backfeeding

The interposer should still work if the touchscreen is asleep or disconnected.

---

## Internal Router Power Management

An internal router or network module can draw significant current and create heat.

Possible controls:

- Router power enable
- Router reset
- Router ready input
- Router power-good input
- Startup delay
- Manual touchscreen restart
- Service mode reset

Important concerns:

- Boot time
- Wi-Fi connection delay
- USB storage mount delay
- Heat
- Current draw
- Game loading dependency
- Network ready state

If the PS2 depends on the router for game loading, sequencing may be important.

---

## Power Sequencing

Some devices may need to turn on in a certain order.

Possible startup sequence:

1. Button Interposer Board powers up
2. Outputs default to safe states
3. Power Management Board initializes
4. Required accessory rails are enabled
5. Power-good states are checked
6. Internal router starts if required
7. Touchscreen starts or wakes
8. Console power request is allowed
9. Optional accessories turn on by profile

The exact sequence depends on the build.

---

## Shutdown Sequencing

Shutdown should also be controlled.

Possible shutdown sequence:

1. User requests power off or console becomes idle
2. Risky actions are confirmed
3. Noncritical accessories are turned off
4. RGB lighting is disabled or dimmed
5. Bluetooth audio is disabled if configured
6. Video board power is handled according to profile
7. Touchscreen dims or sleeps
8. Interposer remains ready if standby power is available

Shutdown should not corrupt saves or interrupt important hardware unexpectedly.

---

## Idle Behavior

Idle behavior can reduce heat and power draw.

Possible idle actions:

- Dim touchscreen
- Turn off RGB lighting
- Disable Bluetooth audio
- Turn off video board if safe
- Sleep display backlight
- Disable internal router if not needed
- Keep critical devices powered

Idle behavior should be conservative.

It is better to leave a device powered than to shut it off at the wrong time.

---

## Manual Override

Power-managed accessories should have a manual override.

Possible override methods:

- Touchscreen control
- Service mode command
- Physical jumper
- UART command
- Firmware profile setting

Manual override is important during testing and troubleshooting.

---

## Default Power States

Every controlled power output needs a default state.

Example defaults:

| Controlled Output | Suggested Default |
| --- | --- |
| `VID_EN` | On when console is on, profile dependent |
| `BT_EN` | Off or profile dependent |
| `RGB_EN` | Off |
| `ROUTER_EN` | Profile dependent |
| `TS_BL_EN` | On briefly, then dim or sleep |
| `CHIME_EN` | Off |
| `AUX_EN1` | Off |
| `AUX_EN2` | Off |

Defaults should be selected based on the build profile.

---

## Safe State

The Power Management Board should support a safe state.

Possible safe-state behavior:

- Disable RGB lighting
- Disable chime output
- Disable noncritical accessory power
- Keep required logic powered
- Avoid interrupting SD2PSX or memory card behavior
- Avoid blanking video unless intentional
- Keep Button Interposer Board functional
- Report safe mode to touchscreen if available

Safe state should not make the console impossible to use.

---

## Backfeeding Risks

Backfeeding is one of the most important risks.

Possible backfeeding paths:

- UART TX/RX
- I2C pull-ups
- Enable lines
- Power-good lines
- HDMI or video-related signals
- Audio signals
- LED paths
- Debug adapter
- Touchscreen connection
- Pull-up resistors to switched rails

Backfeeding can partially power a device, cause weird behavior, or damage hardware.

---

## Backfeeding Prevention

Possible prevention methods:

- Series resistors on signal lines
- Open-drain outputs where appropriate
- Level shifters with proper power-domain behavior
- MOSFET isolation
- Load switches with reverse-current blocking
- Avoid pull-ups to rails that may be off
- Ensure unpowered devices are not driven by powered outputs
- Use high-impedance defaults
- Check unpowered rail voltage with a meter

Backfeeding should be tested during bench and console testing.

---

## Inrush Current

Some accessories may draw a surge current when powered on.

Possible inrush sources:

- Large capacitors
- Video boards
- Routers
- USB devices
- RGB lighting
- Display backlights

Possible mitigation methods:

- Load switch with slew-rate control
- Soft-start regulator
- Current-limited switch
- Staggered power sequencing
- Proper bulk capacitance
- Avoid turning several loads on at once

Inrush behavior should be measured.

---

## Overcurrent Protection

Future versions may include overcurrent protection.

Possible methods:

- Fuse
- Resettable polyfuse
- Load switch with current limit
- Current sense resistor
- Current monitor IC
- Regulator current limit

Overcurrent protection is especially useful for accessory rails.

---

## Thermal Protection

Power circuits can generate heat.

Thermal considerations:

- Regulator efficiency
- MOSFET RDS(on)
- Load switch package
- Current draw
- PCB copper area
- Airflow
- Nearby heat sources
- Enclosed shell temperature

Thermal testing should be performed inside the console shell.

---

## Filtering And Noise

Power Management hardware can introduce noise.

Possible noise sources:

- Buck converters
- Switching loads
- RGB lighting
- Wireless modules
- Router power spikes
- Display backlight
- Poor grounding

Noise reduction methods:

- Decoupling capacitors
- Bulk capacitance
- Ferrite beads
- LC filters where needed
- Good layout
- Short power wiring
- Grounding strategy
- Separate noisy loads from sensitive signals

Power Management should not create video, audio, or UART problems.

---

## Grounding

Grounding must be planned carefully.

Grounding notes:

- Logic signals need shared ground.
- Accessory loads need proper return paths.
- High-current ground should not disturb sensitive inputs.
- Audio boards may be sensitive to ground noise.
- Video boards may be sensitive to grounding and noise.
- Avoid thin ground traces for loads.
- Label ground pads clearly.

Bad grounding can cause random resets, noise, or unstable communication.

---

## Trace Width

Power traces must be sized for the current they carry.

Important factors:

- Load current
- Copper weight
- Trace length
- Temperature rise
- Board thickness
- Available copper area
- Connector current rating

Do not route accessory power through thin logic traces.

If only enable signals are used, high-current trace requirements are reduced.

---

## Connector Planning

Power connectors should be selected carefully.

Possible connector groups:

| Connector | Signals |
| --- | --- |
| Logic power connector | `VCC`, `GND` |
| Accessory input connector | Accessory supply input and ground |
| Switched output connector | Switched accessory power and ground |
| Enable connector | Enable signals from interposer or I/O board |
| Power-good connector | Status feedback signals |
| Touchscreen power connector | Display power and backlight enable |
| Service connector | Debug and test signals |

Connector current rating must match the load.

---

## Connector Labeling

Power connectors must be clearly labeled.

Recommended labels:

- `VIN`
- `VOUT`
- `3V3`
- `5V`
- `9V`
- `GND`
- `EN`
- `PG`
- `VID_EN`
- `BT_EN`
- `RGB_EN`
- `ROUTER_EN`
- `AUX1`
- `AUX2`

Power labels should be unambiguous.

---

## Reverse Polarity Protection

Reverse polarity protection may be useful depending on the connector and power source.

Possible methods:

- Series diode
- Ideal diode circuit
- P-channel MOSFET protection
- Keyed connectors
- Clear labeling

For internal fixed wiring, keyed connectors and clear labels may be enough.

For external or service connectors, additional protection may be useful.

---

## ESD Protection

Power management boards may connect to external or semi-external devices.

Possible ESD-sensitive areas:

- USB-C power paths
- Debug connectors
- Touchscreen harness
- User-accessible controls
- External accessory connectors

ESD protection should be considered for final product versions.

---

## Test Points

Power Management boards should include useful test points.

Suggested test points:

- `VIN`
- `GND`
- `3V3`
- `5V`
- `VID_EN`
- `VID_OUT`
- `VID_PG`
- `BT_EN`
- `BT_OUT`
- `RGB_EN`
- `RGB_OUT`
- `ROUTER_EN`
- `ROUTER_OUT`
- `AUX1_OUT`
- `AUX2_OUT`

Test points make troubleshooting much easier.

---

## Status LEDs

Status LEDs may be useful during development.

Possible LEDs:

| LED | Meaning |
| --- | --- |
| `PWR` | Board has power |
| `3V3` | 3.3V rail active |
| `5V` | 5V rail active |
| `VID` | Video power active |
| `BT` | Bluetooth audio power active |
| `RGB` | Lighting power active |
| `ROUTER` | Router power active |
| `FAULT` | Fault or overcurrent state |

Final builds may omit some LEDs to reduce current draw or light bleed.

---

## Board Size Goals

Power Management hardware should stay compact but not unsafe.

Board size goals:

- Fit inside PS2 Slim and Ultra Slim builds
- Leave room for proper power traces
- Provide enough copper for heat spreading
- Leave room for connectors
- Leave room for test points
- Avoid placing hot parts near plastic or sensitive boards
- Avoid RF shield shorts
- Allow clean wire routing

Do not make the board too small if it compromises current handling or heat.

---

## Board Thickness

Possible board thicknesses:

| Thickness | Notes |
| --- | --- |
| 1.6 mm | Strong and common |
| 1.0 mm | Thinner and useful in tight installs |
| 0.8 mm | Good for compact PS2 builds if current allows |
| 2.0 mm | Stronger but usually unnecessary |
| Flex PCB | Future option for low-current enable wiring only |

Power boards may benefit from standard 1.6 mm thickness if current and thermal handling matter.

---

## Mounting Goals

Power Management hardware should be mounted securely.

Possible mounting methods:

- Screws and standoffs
- Adhesive standoffs
- Printed bracket
- RF shield mounting plate
- Custom carrier plate
- Double-sided tape for prototypes only

Final builds should avoid weak mounting for boards with power wiring.

---

## Insulation

Power boards must be insulated from metal shields.

Insulation methods:

- Kapton tape
- Fish paper
- Standoffs
- Printed bracket
- Solder mask clearance
- No exposed pads facing RF shield
- Heat-resistant insulation where needed

Power boards should not short to the RF shield or shell.

---

## Wire Routing

Power wiring should be routed carefully.

Wire routing goals:

- Use proper wire gauge
- Keep power wires short
- Avoid pinch points
- Avoid hot areas
- Avoid running high-current wires over sensitive signal lines
- Use strain relief
- Keep connectors accessible
- Keep shell closure easy
- Avoid RF shield cutting unless necessary

Power wiring should look intentional and serviceable.

---

## Suggested Wire Colors

Suggested power wire colors:

| Color | Suggested Use |
| --- | --- |
| Black | Ground |
| Red | Main positive power |
| Orange | 3.3V |
| Yellow | 5V |
| Blue | Enable signal |
| White | Power-good or status signal |
| Green | UART or control signal |
| Purple | Service or special function |
| Gray | Accessory status |
| Brown | Secondary power or lid/button-related signal |

Actual wire colors should be documented per build.

---

## Schematic Documentation

The schematic should clearly document:

- Power input source
- Power output target
- Voltage rails
- Current limits
- Load switch or MOSFET part numbers
- Enable logic
- Power-good signals
- Pull-ups and pull-downs
- Backfeeding protection
- Test points
- Connector pinouts
- Default states
- Safety notes

Power schematics should be very clear.

---

## PCB Documentation

PCB documentation should include:

- Board dimensions
- Board thickness
- Copper weight
- Layer count
- Trace width notes
- Current path notes
- Thermal notes
- Connector orientation
- Board renders
- Schematic exports
- Gerber exports
- BOM
- Revision notes
- Known fitment issues

Power-related revisions should be tracked carefully.

---

## Suggested Folder Structure

| Path | Purpose |
| --- | --- |
| `KiCad/` | KiCad project files |
| `Schematics/` | Exported schematic PDFs or images |
| `Images/` | Board renders, layout screenshots, install photos |
| `Gerbers/` | Prototype Gerber exports if shared |
| `BOM/` | Power Management BOM files |
| `Test-Notes/` | Bench and console power test notes |
| `README.md` | Hardware overview and planning notes |

Additional folders can be added later.

---

## File Naming

Suggested file naming:

| File | Purpose |
| --- | --- |
| `FBD-PS2-Button-Butler-Power-Management.kicad_pro` | KiCad project |
| `FBD-PS2-Button-Butler-Power-Management-v0.1.pdf` | Schematic export |
| `FBD-PS2-Button-Butler-Power-Management-v0.1-Gerbers.zip` | Prototype Gerbers |
| `FBD-PS2-Button-Butler-Power-Management-v0.1-BOM.csv` | BOM |
| `FBD-PS2-Button-Butler-Power-Management-v0.1-Top.png` | Top render |
| `FBD-PS2-Button-Butler-Power-Management-v0.1-Bottom.png` | Bottom render |

Consistent file names make revision tracking easier.

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

Do not call a power board stable until it has been load tested.

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
- Current rating
- Power rating
- Tolerance
- Thermal notes
- Substitute parts if available

Power components must be rated properly.

---

## Component Selection Goals

Component selection should prioritize:

- Correct voltage rating
- Correct current rating
- Low heat
- Availability
- Clear datasheets
- Practical package size
- Hand-solderability for prototypes
- Built-in protection where useful
- Stable supplier availability
- Known behavior

Do not choose under-rated power components just to save space.

---

## Bench Testing Plan

Before installing in a console, bench test Power Management hardware.

Bench tests:

1. Visual inspection
2. Continuity check
3. Power input check
4. No-load output voltage check
5. Enable input test
6. Load switch test
7. Dummy load test
8. Current draw measurement
9. Temperature check
10. Power-good signal test if used
11. Startup timing test
12. Shutdown timing test
13. Backfeeding test
14. Repeated power-cycle test
15. Fault or overload behavior test if supported

Do not connect expensive accessories until dummy load tests pass.

---

## Dummy Load Testing

Dummy loads should be used before real accessories.

Possible dummy loads:

- Resistor load
- LED with resistor
- Electronic load
- Small test module
- Known safe load board

Dummy load testing helps confirm switching behavior without risking expensive hardware.

---

## Console Testing Plan

After bench testing, test in a real console.

Console tests:

1. Confirm console works before install
2. Install power board without accessory connected
3. Confirm no abnormal current draw
4. Confirm console still powers normally
5. Confirm power board startup behavior
6. Connect one accessory only
7. Test accessory enable and disable
8. Check for backfeeding
9. Check for video/audio/noise issues
10. Check heat after extended runtime
11. Test shutdown behavior
12. Test shell closure
13. Repeat power cycling
14. Test during gameplay if relevant

Add one accessory at a time.

---

## Suggested Measurements

Useful measurements:

| Measurement | Purpose |
| --- | --- |
| Input voltage | Confirm source voltage |
| Output voltage | Confirm switched rail |
| No-load current | Check board behavior |
| Load current | Confirm accessory draw |
| Standby current | Check always-on load |
| Inrush current | Check startup stress |
| MOSFET temperature | Check switching heat |
| Regulator temperature | Check power conversion heat |
| Load switch temperature | Check part margin |
| Backfeed voltage | Detect unwanted power paths |
| Power-good timing | Confirm stable startup |

Measurements should be saved in `Test-Data/Power-Measurements/`.

---

## Heat Testing

Heat testing should be performed inside the console shell.

Heat testing should include:

- Console idle
- Gameplay
- Accessory on
- Accessory off
- Touchscreen on
- Touchscreen dimmed
- Router active if installed
- Video board active
- RGB lighting active
- Long-duration runtime

Power management is partly about reducing heat, so heat data matters.

---

## Test Data To Save

Save useful test data.

Suggested folder:

`Test-Data/Power-Measurements/`

Useful data:

- Voltage measurements
- Current measurements
- Temperature measurements
- Startup timing
- Shutdown timing
- Power-good timing
- Backfeeding readings
- Accessory behavior
- Console model
- Board revision
- Firmware version
- Test date
- Notes

Good data helps justify design changes.

---

## QA Checklist Ideas

Future QA checklist items:

- Visual inspection
- Check for shorts
- Confirm input voltage
- Confirm output voltage
- Confirm enable behavior
- Confirm output default state
- Confirm current draw
- Confirm no-load behavior
- Confirm dummy-load behavior
- Confirm accessory-load behavior
- Confirm no backfeeding
- Confirm temperature is acceptable
- Confirm power-good behavior if used
- Confirm connector labels
- Confirm board revision
- Record test results

A detailed QA checklist should eventually live in:

`Manufacturing/QA-Checklist.md`

---

## Known Issues

Current known issues:

- Power Management architecture is not finalized.
- Load switch parts are not selected.
- Regulator parts are not selected.
- Power domains are not finalized.
- Accessory targets are not finalized.
- Board size is not finalized.
- Connector style is not finalized.
- Default states are not finalized.
- Current limits are not finalized.
- No prototype has been bench tested yet.
- No prototype has been console tested yet.

This section should be updated as the design develops.

---

## Open Questions

Open hardware questions:

- Which accessories actually need power control?
- Should video board power be controlled first?
- Should ElectronAnalog power control be manual only at first?
- Should RetroGEM power control be delayed until more testing?
- Should BlueRetro remain always powered for controller wake behavior?
- Should Bluetooth audio default to off or profile-dependent?
- Should SD2PSX be monitored only and not power-switched?
- Should the first board only provide enable signals?
- Should load switches be on a separate board?
- Should current monitoring be included?
- Should temperature monitoring be included?
- What standby current is acceptable?
- Which power rail should feed the Power Management Board?
- Should the board use 0.8 mm, 1.0 mm, or 1.6 mm PCB thickness?
- What connector style is safe for accessory current?

These questions should be answered through measurements and prototypes.

---

## Risks

Possible hardware risks:

- Backfeeding between boards
- Accessory power rail overloaded
- Regulator overheating
- MOSFET overheating
- Load switch current limit too low
- Video board instability
- HDMI handshake issues
- Bluetooth reconnect issues
- Audio pops or noise
- SD2PSX save risk if power is interrupted
- Internal router not ready before console boot
- Touchscreen sending unsafe power commands
- User confusion if power-managed devices turn off unexpectedly
- Shell fitment problems
- Power wiring shorts to RF shield

Power features should be added slowly and tested carefully.

---

## Recommended Development Order

Recommended development order:

1. Define accessory targets
2. Measure current draw for each target
3. Identify always-on and switched devices
4. Decide which devices only need enable control
5. Build one simple enable-control test
6. Test with a dummy load
7. Test with one real accessory
8. Check for backfeeding
9. Check heat
10. Add power-good input if useful
11. Add touchscreen status display
12. Add manual touchscreen control
13. Add automatic behavior only after manual control is stable
14. Document results
15. Revise the hardware

Do not begin with automatic power sequencing.

---

## Minimum First Prototype

A good first Power Management prototype could include:

- Logic power input
- Ground
- One enable input
- One switched accessory output
- One load switch or MOSFET circuit
- One power-good or status output if available
- Test points
- Clear labels
- Dummy load test pads

This proves the power-control concept without overbuilding.

---

## Summary

The Power Management hardware is an optional expansion area for the FBD PS2 Button Butler project.

Its job is to safely manage accessory power inside advanced PS2 builds.

The first goal should be simple accessory enable or one safely switched rail.

Advanced features such as video board power control, Bluetooth audio power, RGB power, internal router sequencing, current monitoring, temperature monitoring, and touchscreen-controlled power profiles can be added after the basic system is tested.

Power Management should always prioritize safe PS2 operation, low heat, clean wiring, and predictable behavior over flashy automation.
