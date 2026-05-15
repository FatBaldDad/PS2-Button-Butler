# 06 - Power Management

## Overview

The **FBD PS2 Button Butler** project may eventually manage more than button behavior and mod control.

In advanced builds, Button Butler can also help manage power for internal accessories such as video boards, Bluetooth audio, BlueRetro, SD2PSX, RGB lighting, internal routers, and other future FBD boards.

The goal is not to replace the PS2 power system.

The goal is to give advanced PS2 builds better control over internal accessory power so the console can run cleaner, cooler, and more reliably.

---

## Purpose Of Power Management

Power management is important because modern custom PS2 builds often contain several extra internal devices.

Possible internal devices include:

- RetroGEM
- ElectronAnalog
- BlueRetro
- Bluetooth audio board
- SD2PSX
- RGB lighting
- Internal router or network hardware
- Internal USB storage
- Chime or buzzer circuit
- Touchscreen controller
- I/O expansion board
- Future LazyrSavre-style feedback hardware

Each added board uses power, creates heat, and adds wiring complexity.

Button Butler can help by turning devices on only when needed, disabling devices when idle, and coordinating accessory power with the console state.

---

## Main Goals

The main goals of Button Butler power management are:

- Reduce heat inside the console
- Save power when accessories are not needed
- Prevent unnecessary accessory runtime
- Coordinate power state between mods
- Give the touchscreen control over accessory power
- Keep startup behavior predictable
- Keep shutdown behavior predictable
- Avoid backfeeding between boards
- Avoid unstable power sequencing
- Keep the PS2 console itself safe

Power management should make the build more reliable, not more complicated.

---

## Important Design Rule

Button Butler should not directly interfere with the main PS2 power system unless that circuit is fully understood and tested.

For early versions, Button Butler should focus on controlling accessory power only.

Examples:

- Enable or disable an internal video board
- Enable or disable Bluetooth audio
- Enable or disable RGB lighting
- Enable or disable an internal router
- Enable or disable accessory regulators
- Control standby-powered support boards

The PS2 motherboard power path should be treated carefully.

---

## Power Management Levels

Power management can be added in levels.

| Level | Description |
| --- | --- |
| Level 1 | No power management, only button behavior |
| Level 2 | Simple enable outputs for accessories |
| Level 3 | MOSFET-switched accessory power |
| Level 4 | Touchscreen-controlled accessory power |
| Level 5 | Automatic power sequencing and idle behavior |
| Level 6 | Advanced monitoring and diagnostics |

The first prototype should not try to do everything.

Start with simple enable signals before switching actual power rails.

---

## Possible Power Targets

Button Butler may eventually control several internal power targets.

| Target | Possible Purpose |
| --- | --- |
| RetroGEM | Reduce heat and power when console is idle |
| ElectronAnalog | Power down when video output is not needed |
| BlueRetro | Disable controller system if needed |
| Bluetooth audio | Turn off audio board or trigger pairing system |
| RGB lighting | Disable decorative lighting |
| Internal router | Control network hardware power |
| SD2PSX support circuit | Control display or support electronics |
| Touchscreen | Sleep, wake, or dim display |
| I/O expansion board | Control auxiliary outputs |
| Chime circuit | Enable or disable sound feedback |

Not every build will use every target.

---

## RetroGEM And ElectronAnalog Power Control

RetroGEM and ElectronAnalog power management are important because video boards can add heat inside a compact PS2 build.

Possible power management goals:

- Disable video hardware when the console is idle
- Disable video hardware when the console is off
- Reduce heat inside the shell
- Save power
- Allow manual control from the touchscreen
- Add automatic timeout behavior
- Add service override behavior

This should be tested carefully.

Video hardware should not be power-cycled in a way that creates instability, bad HDMI behavior, or damage.

---

## Video Board Control Ideas

Possible control methods for video boards:

| Method | Description |
| --- | --- |
| Enable pin control | Use an enable line if the board provides one |
| Regulator enable | Control the regulator powering the board |
| High-side MOSFET | Switch the positive supply to the board |
| Load switch IC | Use a dedicated power switch IC |
| Manual override | Allow touchscreen or service mode override |
| Auto timeout | Disable board after idle condition |

A proper load switch or regulator enable input is usually cleaner than switching power with a random transistor circuit.

---

## Accessory Power Enable Outputs

The simplest form of power management is an enable output.

An enable output does not directly supply power.

It only tells another circuit to turn on or off.

Possible uses:

- Enable a regulator
- Enable a load switch
- Enable a MOSFET power circuit
- Enable Bluetooth audio
- Enable RGB lighting
- Enable video board power
- Enable internal router power

This is a good first step because it keeps the Button Butler board from carrying heavy current.

---

## Direct Power Switching

Direct power switching means Button Butler or a related board actually switches power to a device.

Possible methods:

- P-channel MOSFET high-side switch
- N-channel MOSFET low-side switch
- Dedicated load switch IC
- Relay
- Regulator enable pin
- Power mux or ideal diode circuit

For most internal PS2 accessories, a dedicated load switch IC or regulator enable pin may be the cleanest option.

---

## High-Side Switching

High-side switching controls the positive supply going to a device.

Possible advantages:

- Device ground remains connected
- Usually cleaner for accessory modules
- Avoids lifting the ground reference
- Better for devices with signal connections to the console

Possible disadvantages:

- Circuit can be more complex
- P-channel MOSFETs need proper gate drive
- Load switch ICs may add cost
- Must consider inrush current

High-side switching is usually preferred when switching power to modules that also have signal lines.

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
- Can create weird behavior with communication lines
- Not ideal for UART-connected modules
- Not ideal for video or audio modules

Low-side switching is useful for simple loads, but it should be used carefully with complex boards.

---

## Load Switch ICs

A load switch IC is a dedicated chip designed to switch power to a load.

Possible advantages:

- Clean enable input
- Controlled turn-on behavior
- Current limiting on some parts
- Thermal protection on some parts
- Small size
- Better behavior than a basic MOSFET circuit

Possible uses:

- Video board power
- Bluetooth audio power
- Touchscreen power
- RGB power
- Internal router power
- Accessory rail control

Load switch ICs should be considered for final hardware if space and cost allow.

---

## Regulator Enable Control

If an accessory has its own regulator, the cleanest control point may be the regulator enable pin.

Possible advantages:

- Low current control signal
- No need to switch the full load current on Button Butler
- Cleaner layout
- Lower heat on the control board
- Predictable behavior if the regulator is designed well

Possible disadvantages:

- Not all modules expose enable pins
- Some modules may still be partially powered through signal lines
- Startup timing must be tested

Regulator enable control is a good option when available.

---

## Standby Power Considerations

Some parts of Button Butler may need standby power.

Possible always-on devices:

- Button Interposer Board
- Touchscreen wake circuit
- BlueRetro power-on controller
- Small I/O controller
- Power management logic

The always-on section should use as little current as practical.

Always-on power should be designed carefully because it may remain powered whenever the console has external power connected.

---

## Switched Power Considerations

Other devices should only be powered when needed.

Possible switched devices:

- RGB lighting
- Bluetooth audio board
- Video board
- Internal router
- Touchscreen backlight
- Chime circuit
- Nonessential accessory boards

Switched power can reduce heat and power draw.

---

## Touchscreen Power Behavior

The touchscreen may have its own power management behavior.

Possible touchscreen states:

- Fully on
- Dimmed
- Display off but controller awake
- Fully asleep
- Wake on touch
- Wake on button press
- Wake on console state change
- Wake on error or service event

The touchscreen should not need to run at full brightness all the time.

---

## Touchscreen Sleep Goals

Touchscreen sleep behavior should:

- Reduce heat
- Reduce current draw
- Prevent unnecessary screen wear
- Keep the console looking clean when idle
- Wake quickly when needed
- Not interfere with normal button behavior

The interposer should still work even if the touchscreen is asleep or disconnected.

---

## Console State Detection

Power management depends on knowing the console state.

Possible states:

- External power present
- Console standby
- Console on
- Console off
- Console resetting
- Lid open
- Lid closed
- Accessory power active
- Accessory power disabled
- Idle timeout active

Not all of these states will be available in early hardware.

Only reliable state information should be used for automatic decisions.

---

## Possible Console State Signals

Possible signals that may help detect console state:

- PS2 standby power rail
- PS2 switched power rail
- Power/reset button state
- Lid switch state
- Accessory regulator output
- Video board power-good signal
- Touchscreen heartbeat
- Interposer firmware state
- BlueRetro or SD2PSX status if available

Each signal should be tested on real hardware before relying on it.

---

## Idle Detection

Idle detection could allow Button Butler to power down accessories when not needed.

Possible idle conditions:

- Console appears off
- No button activity for a set time
- Touchscreen inactive
- Accessory not in use
- Video board not needed
- Service mode not active
- No controller activity if available
- No error condition active

Idle detection should be conservative.

It is better to leave a device on than to shut it off at the wrong time.

---

## Power Sequencing

Some devices may need to turn on or off in a specific order.

Possible startup sequence:

1. Button Interposer powers up
2. Outputs default to safe states
3. Accessory regulators remain off
4. Console state is checked
5. Touchscreen starts
6. Required accessories are enabled
7. Status is reported
8. User interface becomes active

Possible shutdown sequence:

1. User requests power off or idle timeout occurs
2. Risky actions are confirmed
3. Nonessential accessories are disabled
4. Touchscreen dims or sleeps
5. Outputs return to safe states
6. Interposer remains ready for button input

The exact sequence depends on hardware.

---

## Startup Safety

Startup safety is very important.

During startup:

- Outputs should default inactive
- Reset control should not pulse accidentally
- Power enable lines should not glitch on
- Mod control states should be defined
- Touchscreen should wait for interposer status
- Interposer should initialize before accepting commands
- Accessory power should not turn on randomly
- Backfeeding should be avoided

The console should not behave unpredictably during power-up.

---

## Shutdown Safety

During shutdown:

- Accessory boards should not backfeed the console
- UART lines should not power unpowered devices
- Outputs should return to safe states
- Touchscreen should not keep sending commands
- Power rails should decay safely
- Mod control lines should not float
- Reset should not be held by mistake

Shutdown behavior should be tested as carefully as startup behavior.

---

## Backfeeding Risks

Backfeeding is one of the biggest risks in a multi-board power system.

Backfeeding can happen when a powered board sends voltage into an unpowered board through a signal line.

Possible backfeeding paths:

- UART TX and RX lines
- Enable lines
- Pull-up resistors
- LED lines
- Status outputs
- I/O pins
- HDMI or video-related signals
- Audio signals
- Shared connectors
- Debug adapters

Backfeeding can cause strange behavior, partial power-up, latch-up, or damage.

---

## Backfeeding Prevention

Possible ways to reduce backfeeding:

- Use series resistors on signal lines
- Use open-drain style outputs where possible
- Use proper level shifting
- Use load switches with reverse current blocking when needed
- Use MOSFET isolation
- Avoid pull-ups to a rail that may be off
- Power communicating devices from the same rail where practical
- Ensure unpowered devices do not receive driven logic signals
- Use high-impedance defaults
- Document every power domain

Backfeeding prevention should be considered early, not after the board is already built.

---

## Power Domains

Button Butler may involve several power domains.

Possible power domains:

| Power Domain | Possible Use |
| --- | --- |
| PS2 standby power | Always-on logic, button monitoring |
| PS2 switched power | Console-on accessory logic |
| 3.3V logic | MCU, UART, low-power control |
| 5V accessory power | Bluetooth audio, RGB, small modules |
| 8.5V / 9V input | PS2 Slim input power area |
| USB-C PD rail | Optional alternate power source |
| Video board rail | RetroGEM or ElectronAnalog power |
| Router rail | Internal A5-V11 or similar hardware |
| Display rail | Touchscreen and backlight |

Each domain should be labeled clearly in schematics and board layouts.

---

## 3.3V Logic Rail

The 3.3V rail may be used for:

- PIC or MCU logic if selected part is 3.3V
- UART communication
- Touchscreen logic
- Some control outputs
- Status inputs
- I/O expansion logic

Important notes:

- Confirm all connected devices are 3.3V compatible.
- Avoid feeding 5V logic into 3.3V pins.
- Use level shifting when needed.
- Add decoupling near every IC.

---

## 5V Accessory Rail

The 5V rail may be used for:

- Bluetooth audio boards
- Some RGB lighting
- Small support modules
- USB-related accessories
- Some touchscreen modules
- Chime or buzzer modules

Important notes:

- Confirm current draw.
- Confirm regulator heat.
- Do not overload the PS2 supply.
- Add filtering if the accessory is noisy.
- Use switching regulators where linear regulators would waste too much power.

---

## 8.5V / 9V PS2 Input Area

PS2 Slim consoles commonly use an external DC input around this range.

Important notes:

- This is closer to the main console power input.
- It may be used to feed accessory buck regulators.
- It should not be modified casually.
- Any added converter should be efficient and well-filtered.
- Current draw should be measured under real use.
- Protection should be considered.

Button Butler should not directly switch the main PS2 input power in early prototypes.

---

## USB-C PD Power Concepts

Some builds may use USB-C PD power through a trigger board or a custom PowerOR-style system.

Possible uses:

- Alternate power input
- Accessory power source
- Compact PS2 build power
- Bench testing power source

Important notes:

- Confirm requested PD voltage.
- Confirm current capability.
- Confirm buck converter output.
- Do not mix power sources without proper OR-ing.
- Avoid backfeeding the OEM PSU path.
- Test startup and load behavior.

USB-C PD power management should be handled by a dedicated power design, not improvised on the Button Butler logic board.

---

## Accessory Current Budgeting

Every internal accessory adds current draw.

The build should have a current budget.

Example current budget items:

| Device | Current Notes |
| --- | --- |
| Touchscreen | Depends on display and backlight brightness |
| BlueRetro | Depends on ESP32 module and Bluetooth activity |
| Bluetooth audio | Depends on module and audio output state |
| RGB lighting | Can become high current quickly |
| Video board | Can add heat and current draw |
| Internal router | Can draw significant current for a small build |
| SD2PSX support | Usually modest, but should still be measured |
| Chime circuit | Short bursts, depends on design |

Measure real current whenever possible.

Do not rely only on guesses.

---

## Heat Management

Power management is also heat management.

Possible heat sources:

- Video boards
- ESP32 modules
- Internal routers
- Linear regulators
- RGB LEDs
- Buck converters
- Bluetooth audio modules
- Tight shell spaces
- Poor airflow

Powering down unused accessories can help keep the build cooler.

This is especially important for Ultra Slim and compact custom builds.

---

## Regulator Selection

Any added regulator should be selected carefully.

Important regulator factors:

- Input voltage range
- Output voltage
- Current rating
- Efficiency
- Thermal behavior
- Package size
- Layout requirements
- Switching noise
- Availability
- Protection features

Linear regulators are simple but can get hot when dropping a lot of voltage.

Switching regulators are more efficient but need better layout and filtering.

---

## Filtering And Noise

Internal accessories can introduce electrical noise.

Possible noise sources:

- Buck converters
- RGB lighting
- Wireless modules
- Internal routers
- Video boards
- Motor activity from optical drive
- Long wires

Possible noise control methods:

- Decoupling capacitors
- Bulk capacitors where needed
- Ferrite beads
- LC filters
- Proper grounding
- Short power wiring
- Separate noisy loads from sensitive signals
- Good PCB layout

Power management should not create new noise problems.

---

## Grounding

All controlled accessories need proper grounding.

Grounding notes:

- Logic signals need a shared ground reference.
- High-current accessory grounds should be routed carefully.
- Avoid skinny ground paths for high-current loads.
- Keep noisy loads away from sensitive button and UART wiring.
- Use clear ground labels.
- Avoid random ground loops where possible.

Bad grounding can cause resets, UART errors, audio noise, or strange button behavior.

---

## Video Board Power Notes

Video boards should be handled carefully.

Possible concerns:

- HDMI handshake behavior
- Startup timing
- Heat generation
- Power rail noise
- Signal backfeeding
- Ground reference
- Firmware behavior after power cycle
- User confusion if video disappears

If a video board is powered down, the user should know why.

The touchscreen should show video power state if this feature is used.

---

## Bluetooth Audio Power Notes

Bluetooth audio power control can be useful.

Possible features:

- Enable audio board
- Disable audio board
- Pairing mode trigger
- Reset audio board
- Auto-disable after idle
- Touchscreen control
- Button shortcut control

Possible concerns:

- Audio pops during power switching
- Pairing state loss
- Reconnect delay
- Noise from power rail
- Ground loop noise
- Logic level compatibility

Bluetooth audio should be tested with real speakers/headphones.

---

## BlueRetro Power Notes

BlueRetro may need special power consideration.

Possible goals:

- Keep BlueRetro always available for controller wake
- Disable BlueRetro when wired controllers are preferred
- Reset BlueRetro from touchscreen
- Disable individual channels if supported
- Avoid power loss that breaks expected controller behavior

BlueRetro power strategy depends on the build.

Some builds may need BlueRetro always powered.

Other builds may only need an enable or reset control.

---

## SD2PSX Power Notes

SD2PSX may need to remain stable for memory card behavior.

Possible concerns:

- Avoid power cycling during gameplay
- Avoid corrupting data
- Avoid removing power during writes
- Avoid confusing OPL or game saves
- Keep memory card behavior predictable

Button Butler should be conservative with anything related to memory card power or control.

Status display is safer than aggressive power switching.

---

## RGB Lighting Power Notes

RGB lighting can draw more current than expected.

Possible control options:

- Simple on/off control
- Brightness control
- Preset modes
- Status lighting only
- Disable during gameplay
- Disable when console is off
- Touchscreen control

Important notes:

- Add current limiting.
- Confirm LED current.
- Avoid heat buildup.
- Avoid injecting noise into audio or video.
- Avoid pulling too much power from small regulators.

---

## Internal Router Power Notes

An internal router or network module can draw significant current.

Possible goals:

- Power router only when needed
- Keep router on before console boot if using network loading
- Wait for router ready state before allowing console startup
- Show router status on touchscreen
- Provide manual restart option

Possible concerns:

- Startup time
- Heat
- Current draw
- Wi-Fi connection delay
- USB storage mount delay
- Signal backfeeding
- Power sequencing

If the PS2 depends on the router for game loading, power sequencing becomes more important.

---

## Power Good Signals

Some regulators or modules may provide power-good signals.

Possible uses:

- Confirm accessory power is stable
- Delay startup until accessory is ready
- Show status on touchscreen
- Prevent commands until power is valid
- Detect faults

Power-good signals are useful but should not be trusted without testing.

---

## Fault Detection

Button Butler may eventually detect power faults.

Possible fault conditions:

- Accessory power failed to turn on
- Accessory power failed to turn off
- Overcurrent detected
- Regulator overheated
- Power-good signal missing
- Video board not responding
- Touchscreen not responding
- I/O expansion not responding

Early prototypes may only log or display faults.

Advanced versions may take automatic action.

---

## Touchscreen Power Page

The touchscreen could include a power management page.

Possible controls:

- Show console power state
- Show accessory power states
- Toggle video board power
- Toggle Bluetooth audio power
- Toggle RGB lighting
- Restart internal router
- Show power-good status
- Show warnings
- Enable or disable auto power saving

The page should clearly show what is safe to control.

---

## Automatic Power Saving

Automatic power saving should be added carefully.

Possible automatic actions:

- Dim touchscreen after inactivity
- Turn off RGB after timeout
- Disable Bluetooth audio when not connected
- Disable video board when console is off
- Disable internal router when not needed
- Sleep support boards when idle

Automatic actions should never interrupt gameplay, saves, or critical boot behavior.

---

## Manual Override

A manual override should be available for power-managed accessories.

Possible override methods:

- Touchscreen button
- Service mode command
- Physical jumper
- UART command
- Firmware profile setting

Manual override is useful during testing and troubleshooting.

---

## Default Power States

Each controlled power output needs a default state.

Possible defaults:

| Output | Possible Default |
| --- | --- |
| Touchscreen | On briefly, then sleep if idle |
| RGB lighting | Off |
| Bluetooth audio | Off or build-profile dependent |
| Video board | On when console is on |
| Internal router | Build-profile dependent |
| BlueRetro | Build-profile dependent |
| SD2PSX support | On if required |
| Chime circuit | Off |
| Accessory regulator | Off unless required |

The correct default depends on the build.

Defaults should be documented per build profile.

---

## Build Profiles

Power management should support different build profiles.

Possible profiles:

- Basic profile
- BlueRetro profile
- Bluetooth audio profile
- SD2PSX profile
- Internal router profile
- Premium Ultra Slim profile
- Service/testing profile

Each profile may have different power defaults.

---

## Example Power Profiles

### Basic Profile

Possible behavior:

- Touchscreen not installed
- No accessory power switching
- Button interposer always ready
- All optional outputs inactive

---

### BlueRetro Profile

Possible behavior:

- BlueRetro stays powered if controller wake is needed
- Optional BlueRetro reset output
- Touchscreen can show BlueRetro control if installed
- RGB lighting off by default

---

### Bluetooth Audio Profile

Possible behavior:

- Bluetooth audio off by default
- Pairing trigger available
- Touchscreen can enable audio
- Audio board can be reset if pairing fails

---

### Internal Router Profile

Possible behavior:

- Router powers before console boot
- Button Butler waits for router ready signal if available
- Touchscreen shows router status
- Router can be restarted from service page

---

### Premium Ultra Slim Profile

Possible behavior:

- Touchscreen enabled
- Video board power managed
- BlueRetro enabled
- SD2PSX support active
- Bluetooth audio user-controlled
- RGB lighting user-controlled
- Service page available

---

## Safety Rules

Power management should follow strict safety rules.

Recommended rules:

- Do not switch unknown loads without testing.
- Do not power-cycle SD2PSX during saves.
- Do not power-cycle video hardware during normal gameplay unless intended.
- Do not allow floating enable lines.
- Do not backfeed unpowered devices.
- Do not exceed regulator limits.
- Do not rely on software alone for safe default states.
- Do not let touchscreen failure disable basic console use.
- Do not let accessory faults hold the PS2 in reset.
- Do not make customer operation confusing.

Safety comes before feature count.

---

## Testing Requirements

Power management testing should include:

- Current measurement
- Voltage measurement
- Heat testing
- Startup testing
- Shutdown testing
- Repeated power cycling
- Accessory enable/disable testing
- UART communication while switching power
- Backfeeding checks
- Console boot testing
- Gameplay testing
- Long-duration testing

Testing should be done before installing into customer builds.

---

## Suggested Test Measurements

Useful measurements:

| Measurement | Purpose |
| --- | --- |
| Standby current | Check always-on load |
| Console-on current | Check total draw |
| Accessory current | Check each added board |
| Regulator temperature | Check thermal safety |
| Video board temperature | Check heat reduction |
| UART voltage | Check logic compatibility |
| Enable line voltage | Confirm default and active states |
| Backfeed voltage | Detect unwanted power paths |
| Startup timing | Confirm power sequence |
| Shutdown decay | Confirm clean power-down |

Real measurements should be saved in the `Test-Data/Power-Measurements/` folder.

---

## Recommended Development Order

Recommended order:

1. Define power domains
2. Identify always-on and switched devices
3. Measure accessory current
4. Add simple enable output
5. Test enable output with a safe dummy load
6. Add MOSFET or load switch prototype
7. Test accessory power switching outside the console
8. Test inside the console with one accessory
9. Add touchscreen status display
10. Add manual control
11. Add automatic timeout only after manual control is stable
12. Add power-good or fault detection later

Do not begin with automatic power sequencing.

Start with manual control and measurement.

---

## Minimum Viable Power Management

The minimum useful power management feature is one safe accessory enable output.

Minimum viable features:

- One defined output
- Known default state
- Manual control
- Status reporting
- No floating state
- No backfeeding
- Tested with a dummy load
- Tested with one real accessory

This is enough to prove the concept.

---

## Open Questions

Open questions for power management:

- Which accessories should be always powered?
- Which accessories should be switched?
- Should video board power be controlled by regulator enable or load switch?
- Should BlueRetro remain powered for controller wake?
- Should SD2PSX ever be power-managed, or only monitored?
- How should internal router startup delay be handled?
- Which power domains are available in each PS2 model?
- How much standby current is acceptable?
- What is the safest default state for each accessory?
- Should power profiles be selected by firmware, touchscreen, or hardware jumper?
- Should current monitoring be included in future hardware?

These questions should be answered during prototype testing.

---

## Risks

Possible risks:

- Accessory backfeeding
- Regulator overheating
- Excessive current draw
- Noisy power rails
- Console startup instability
- Touchscreen causing power commands at the wrong time
- Video board instability after power cycling
- SD2PSX data risk if power is interrupted
- Bluetooth reconnect delays
- Internal router not ready before console boot
- Customer confusion from automatic power behavior

These risks should guide the design.

---

## Summary

Power management is an optional but important part of the Button Butler project.

The first goal is not to control everything.

The first goal is to safely define power domains, measure current draw, and add simple accessory enable control.

Advanced versions may allow the touchscreen to control accessory power, reduce heat by powering down unused video hardware, manage Bluetooth audio, control RGB lighting, and coordinate internal router startup.

The system should always prioritize safe defaults, clean power sequencing, and reliable PS2 operation over flashy features.
