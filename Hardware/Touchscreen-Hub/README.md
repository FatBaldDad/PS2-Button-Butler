# Touchscreen Hub Hardware

## Overview

This folder contains hardware planning notes, schematic ideas, PCB notes, wiring notes, and future design files for the **Touchscreen Hub** used in the **FBD PS2 Button Butler** project.

The Touchscreen Hub is the optional premium user-interface layer for Button Butler.

The base Button Butler system starts with the Button Interposer Board. The Touchscreen Hub expands the system by giving the user a clean visual interface for controlling internal PlayStation 2 mods, viewing status, accessing service pages, and sending command requests to the Button Interposer Board.

The Touchscreen Hub should make advanced PS2 builds easier to use while keeping the install clean and intentional.

---

## Purpose

The purpose of the Touchscreen Hub hardware is to provide a central control panel for advanced FBD PS2 builds.

Possible touchscreen functions include:

- Console power control
- Console reset control
- ChipSlayer enable/disable
- Modchip enable/disable
- Bluetooth audio enable/disable
- Bluetooth audio pairing
- BlueRetro enable/disable
- BlueRetro reset or channel control
- RGB lighting control
- SD2PSX status display
- RetroGEM or ElectronAnalog power control
- Internal router status or reset
- Service and diagnostic pages
- Future LazyrSavre-style laser feedback display
- Build-specific branding and status display

The touchscreen should replace the need for several separate switches, buttons, and LEDs.

---

## Project Status

| Area | Status |
| --- | --- |
| Hardware concept | In planning |
| Touchscreen module | Not finalized |
| Controller MCU | Not finalized |
| Communication method | Concept planning |
| Mounting method | Not finalized |
| Power source | Not finalized |
| Wiring harness | Not finalized |
| Bench testing | Not started |
| Console testing | Not started |
| Customer-ready status | Not ready |

This hardware should be treated as experimental until it is built and tested.

---

## Relationship To Button Butler

The Touchscreen Hub is part of the larger Button Butler system.

| Board | Purpose |
| --- | --- |
| Button Interposer Board | Handles PS2 power/reset behavior and safety logic |
| Touchscreen Hub | Provides user interface and sends command requests |
| I/O Expansion Board | Provides extra raw inputs and outputs |
| Power Management Board | Controls accessory power if used |
| Service Adapter | Allows debugging and firmware development |

The Touchscreen Hub should not replace the Button Interposer Board.

The Button Interposer Board should remain responsible for critical console power/reset behavior.

---

## Main Design Goal

The main design goal is:

**Provide a clean touchscreen control interface without making the PS2 shell look cluttered or hacked up.**

The touchscreen should look like a designed feature.

It should not look like a random screen glued onto the console.

---

## Control Philosophy

The Touchscreen Hub should use a request-and-confirm control model.

Recommended control flow:

| Step | Action |
| --- | --- |
| 1 | User taps a touchscreen control |
| 2 | Touchscreen sends a command request |
| 3 | Button Interposer checks safety rules |
| 4 | Button Interposer accepts or rejects the command |
| 5 | Button Interposer performs the safe action |
| 6 | Button Interposer sends a response |
| 7 | Touchscreen updates the display |

The touchscreen should not assume that a command worked until the interposer confirms it.

---

## Why The Interposer Stays In Charge

The Button Interposer Board stays in charge because it is directly connected to the PS2 power/reset button behavior.

The interposer should handle:

- Button input
- Button debounce
- Lid switch input
- Console-side power/reset behavior
- Safe output defaults
- Service mode
- Safe mode
- Command validation
- Risky action lockouts

The touchscreen is the user interface.

The interposer is the safety controller.

---

## Hardware Philosophy

The Touchscreen Hub should follow the same design philosophy as the rest of Button Butler:

1. Clean installation
2. Minimal shell modification
3. Clear wiring
4. Safe command behavior
5. Serviceable mounting
6. Low heat
7. Low unnecessary power draw
8. Reliable communication
9. Build-specific flexibility
10. Premium FBD presentation

The touchscreen should improve the build, not complicate it.

---

## Possible Touchscreen Hardware

Possible touchscreen hardware options include:

| Hardware Type | Notes |
| --- | --- |
| Small round touchscreen module | Good visual fit for premium builds |
| Round display with separate touch input | Possible if touch module is separate |
| Waveshare-style round touchscreen | Good candidate for early testing |
| RP2040-based display module | Good development ecosystem |
| RP2350-based display module | Stronger option with more resources |
| ESP32-based display module | Wi-Fi/Bluetooth capable, but may add complexity |
| Custom FBD display board | Future product direction |

The first hardware choice should prioritize ease of testing and clean mounting.

---

## Preferred Touchscreen Traits

Useful touchscreen traits:

- Small footprint
- Round shape if possible
- Bright display
- Low enough power draw
- Reliable touch detection
- Good documentation
- Simple firmware flashing
- UART access
- Enough GPIO or UARTs for expansion
- Reasonable mounting options
- Available from common suppliers

A round touchscreen is attractive because it can look like a factory-style premium control feature.

---

## Touchscreen Size Considerations

The screen should be large enough to use but small enough to mount cleanly.

Important size factors:

- Shell space
- Mounting location
- Touch target size
- Display readability
- Internal board clearance
- Cable routing
- Shell curvature
- Build aesthetics

A screen that looks good in a mockup still needs to fit inside the actual console.

---

## Round Display Benefits

A round display has several benefits for this project.

Possible benefits:

- Looks intentional
- Matches power-button style themes
- Works well as a status dial
- Can fit custom shell designs
- Can become a recognizable FBD feature
- Looks cleaner than a square screen cut into the shell

Possible drawbacks:

- UI layout is more limited
- Corners are not available for buttons
- Text space is smaller
- Mounting may be trickier
- Firmware graphics may need special planning

The UI should be designed around the display shape.

---

## Display Resolution

Display resolution affects UI design.

Important resolution factors:

- Text size
- Icon quality
- Number of buttons per page
- Touch accuracy
- Graphics memory usage
- Firmware performance
- Asset size

The first UI should use large simple buttons and short labels.

Do not design the first UI like a full computer screen.

---

## Touch Input

Touch input must be reliable.

Touch input concerns:

- Touch accuracy
- Edge detection
- False touches
- Touch calibration
- Touch debounce
- Touch response time
- Touch behavior after mounting
- Touch behavior through acrylic or protective covers
- Noise from internal electronics

Touchscreen actions should be filtered so accidental touches do not trigger risky functions.

---

## Touch Input Safety Rules

Recommended touch safety rules:

- Ignore touches during startup
- Require touch release before another action
- Use large touch targets
- Avoid risky buttons near screen edges
- Confirm reset or power-off actions
- Confirm video power-off actions
- Lock service functions
- Ignore repeated accidental taps
- Disable buttons while a command is pending

The screen should not accidentally reset or power off the console.

---

## Touchscreen Mounting Goals

The touchscreen should be mounted cleanly.

Mounting goals:

- Secure fit
- Clean appearance
- Minimal shell cutting
- No loose screen movement
- Good viewing angle
- Touch surface accessible
- Wires hidden
- No interference with internal components
- No blocked airflow
- Serviceable if needed

The screen should look like it belongs on the build.

---

## Possible Mounting Methods

Possible mounting methods:

| Method | Notes |
| --- | --- |
| Shell cutout | Clean if cut precisely |
| Acrylic top plate | Good for custom shell builds |
| 3D printed bezel | Good for prototypes and repeatability |
| Metal bezel | Premium option for final builds |
| Internal bracket | Keeps display secure from inside |
| Adhesive mount | Useful for prototypes but not ideal long-term |
| Screw-mounted carrier | Strong and serviceable |
| Magnetic cover access | Possible for service-friendly custom builds |

The final method may vary by build type.

---

## Mounting Location Ideas

Possible touchscreen mounting locations:

| Location | Notes |
| --- | --- |
| Top shell | Most visible and premium |
| Front panel | Easy to reach, may be tight |
| Disc lid area | Useful on no-disc or custom builds |
| Custom top plate | Best for FBD custom shell builds |
| Ultra Slim top face | Strong candidate for premium builds |
| PS2 Fat top shell | More room but different style |
| Service-access panel | Useful for installer-focused builds |

The best location depends on the console model and build style.

---

## Shell Cutting Considerations

If the shell must be cut, the cut should be clean.

Shell cutting rules:

- Cut only when the feature justifies it.
- Use a template or CNC where possible.
- Avoid rough hand-cut openings for final builds.
- Leave enough material for strength.
- Avoid screw posts and internal supports.
- Avoid areas that flex heavily.
- Keep the screen centered or visually intentional.
- Plan bezel coverage before cutting.

A bad screen cut can ruin an otherwise premium build.

---

## Bezel Planning

A bezel may be needed around the screen.

Possible bezel materials:

- 3D printed plastic
- Acrylic
- Aluminum
- Brass
- Copper
- Painted plastic
- Resin printed part
- Custom machined ring
- Laser-cut plate

The bezel should hide the cut edge and make the touchscreen look intentional.

---

## Internal Clearance

Internal clearance must be checked.

Clearance concerns:

- Touchscreen board thickness
- Display cable
- Connector height
- RF shield
- Optical drive
- Fan duct
- Memory card slots
- Controller ports
- BlueRetro board
- SD2PSX hardware
- HDMI board
- Power wiring
- Screw posts

Fitment should be checked before committing to a shell cut.

---

## Touchscreen Cable Routing

Touchscreen wiring should be clean and serviceable.

Cable routing goals:

- Short wiring where possible
- No pinch points
- No sharp bends
- No pressure under shell
- No routing over hot parts
- No routing through screw posts
- No interference with fan or drive
- Strain relief near connector
- Easy unplugging if service is needed

A touchscreen should not become a fragile wire trap.

---

## Suggested Touchscreen Signals

Possible touchscreen connector signals:

| Signal | Purpose |
| --- | --- |
| `VCC` | Touchscreen logic/display power |
| `GND` | Ground |
| `UART_TX` | Touchscreen transmit to interposer |
| `UART_RX` | Touchscreen receive from interposer |
| `WAKE` | Optional wake signal |
| `RESET` | Optional touchscreen reset |
| `BL_EN` | Optional backlight enable |
| `SERVICE` | Optional service or mode input |
| `SDA` | Optional I2C data |
| `SCL` | Optional I2C clock |
| `GPIO1` | Optional spare signal |
| `GPIO2` | Optional spare signal |

The first version may only need power, ground, TX, and RX.

---

## Minimum Touchscreen Wiring

Minimum wiring for early testing:

| Signal | Purpose |
| --- | --- |
| `VCC` | Power |
| `GND` | Ground reference |
| `TX` | Touchscreen sends commands |
| `RX` | Touchscreen receives responses |

Optional signals can be added later.

---

## UART Communication

UART is the preferred early communication method.

UART is useful because it is:

- Simple
- Easy to debug
- Easy to test with a USB-to-serial adapter
- Common on microcontrollers
- Good enough for command and status traffic

The touchscreen should follow the protocol notes in the `Firmware/Protocol-Notes/` folder.

---

## UART Wiring Rules

UART wiring rules:

- Touchscreen TX connects to Interposer RX.
- Touchscreen RX connects to Interposer TX.
- Ground must be shared.
- Logic levels must match.
- Do not connect 5V UART to a 3.3V-only input.
- Do not tie multiple TX lines together directly.
- Avoid backfeeding unpowered boards through UART lines.

UART should be point-to-point unless a proper mux or bus design is used.

---

## Multiple UARTs

Some touchscreen controllers may have multiple UARTs.

Multiple UARTs could be useful for:

- Button Interposer communication
- BlueRetro status or control
- SD2PSX status or control
- Bluetooth audio module control
- Internal router service UART
- Mechacon or Emotion Engine service access
- Debug adapter

Not every UART device should be added to the first version.

---

## Touchscreen As UART Hub

In advanced builds, the touchscreen may act as a UART hub.

Possible UART assignments:

| Touchscreen UART | Possible Use |
| --- | --- |
| UART 0 | Programming or debug |
| UART 1 | Button Interposer |
| UART 2 | BlueRetro-related controller |
| UART 3 | SD2PSX-related controller |
| Muxed UART | Mechacon, Emotion Engine, router, or service devices |

This is a future concept and should not complicate the first prototype.

---

## UART Mux Option

A UART mux may allow one touchscreen UART to access multiple low-priority devices.

Possible muxed devices:

- Mechacon UART
- Emotion Engine UART
- Internal router UART
- Service debug UART
- Future diagnostic board

Muxed UART should only be used when devices do not need constant communication.

The mux must prevent multiple devices from talking at the same time.

---

## I2C Option

I2C may be useful for local expansion.

Possible I2C uses:

- GPIO expander
- Temperature sensor
- EEPROM
- LED driver
- Touchscreen support chip
- Local sensor board

I2C should be kept short and clean inside the console.

I2C should not be treated as a replacement for every UART device.

---

## SPI Option

Some displays use SPI internally.

SPI may be used for:

- Display communication
- Flash memory
- Local peripherals

SPI is usually local to the touchscreen module and should not need to run across the console unless a future design requires it.

Long SPI wiring inside the console should be avoided unless carefully tested.

---

## Power Source

The touchscreen needs a stable power source.

Possible power sources:

| Power Source | Notes |
| --- | --- |
| PS2 standby rail | Allows screen or controller to remain available when console is off |
| PS2 switched rail | Screen only active when console is on |
| Accessory 5V rail | May be useful for display/backlight |
| 3.3V logic rail | May power MCU logic |
| Dedicated regulator | Cleanest if display current is significant |
| Power Management Board | Future controlled touchscreen power |

The correct source depends on desired behavior.

---

## Standby Power Option

Standby power may allow:

- Touchscreen wake behavior
- Console power-on request
- Status screen while console is off
- Always-ready user interface
- BlueRetro or other wake-related features

Possible concerns:

- Standby current draw
- Heat
- Screen staying on unnecessarily
- Battery-like behavior expectations
- Need for sleep mode
- Backfeeding through UART or GPIO

If the touchscreen is standby-powered, sleep behavior becomes important.

---

## Switched Power Option

Switched power may be simpler.

Possible advantages:

- Lower standby current
- Screen only active when console is on
- Less always-on heat
- Simpler power behavior

Possible disadvantages:

- Touchscreen cannot request power-on if it is unpowered
- No screen status while console is off
- Startup delay after console powers on
- Less premium control behavior

This may be good for early prototypes.

---

## Touchscreen Backlight Power

The display backlight may draw more current than the controller logic.

Backlight control may include:

- Always on when touchscreen is active
- PWM dimming
- Backlight enable line
- Sleep timeout
- Manual brightness setting
- Auto dim after inactivity

Backlight power should be measured.

Do not assume the display current is small.

---

## Power Draw

The touchscreen power draw should be measured.

Useful measurements:

| Measurement | Purpose |
| --- | --- |
| Display off current | Sleep or idle current |
| Backlight low current | Dimmed mode current |
| Backlight full current | Worst-case display current |
| Touch active current | Normal usage |
| Startup current | Inrush or boot behavior |
| Standby current | Always-on impact |

Power draw affects heat and power management decisions.

---

## Heat Considerations

The touchscreen can add heat.

Heat concerns:

- Backlight heat
- Display controller heat
- Regulator heat
- Tight shell space
- No airflow near mounted screen
- Heat from nearby video boards
- Heat from internal router or ESP32 modules

The screen should not be placed where heat will damage it or make the shell uncomfortable.

---

## Touchscreen Sleep Behavior

Sleep behavior can reduce heat and current draw.

Possible sleep modes:

| Mode | Description |
| --- | --- |
| Dim | Backlight reduced |
| Display Off | Backlight off, controller awake |
| Controller Sleep | MCU low-power mode |
| Full Off | Power removed if hardware supports it |

The interposer should continue working even if the touchscreen is asleep.

---

## Wake Signals

Possible wake signals:

| Wake Source | Description |
| --- | --- |
| Touch input | User taps screen |
| Button event | Power/reset button activity |
| Lid event | Lid switch changes state |
| UART event | Interposer sends event |
| Console power state | Console turns on or off |
| Service jumper | Installer wakes service screen |
| Error event | Warning wakes screen |

Wake behavior should not trigger accidental commands.

---

## Reset Signal

A reset signal may be useful.

Possible reset uses:

- Reset touchscreen controller
- Recover from UI lockup
- Allow Button Interposer to restart display
- Service mode testing
- Firmware flashing support

The reset signal should not be confused with PS2 console reset.

Label it clearly.

---

## Service Access

The Touchscreen Hub may need service access.

Possible service access methods:

- UART debug pads
- USB port on touchscreen module
- Programming pads
- Hidden service page
- Service jumper
- Shell-accessible service connector
- Removable cover panel

Service access should be planned before the touchscreen is permanently mounted.

---

## Firmware Flashing Access

Firmware flashing access depends on the controller.

Possible flashing methods:

- USB-C or Micro USB on display module
- UF2 drag-and-drop
- SWD pads
- UART bootloader pads
- USB bootloader button combination
- External programmer pads

If the touchscreen is buried inside the console, firmware flashing becomes harder.

Plan access early.

---

## Button And Boot Pins

Some touchscreen controllers may need boot or reset pins.

Possible signals:

| Signal | Purpose |
| --- | --- |
| `BOOT` | Enter bootloader mode |
| `RESET` | Reset controller |
| `USB_D+` / `USB_D-` | USB programming if exposed |
| `SWDIO` | SWD programming data |
| `SWCLK` | SWD programming clock |
| `UART_BOOT` | UART bootloader if supported |

These should be documented for the selected hardware.

---

## Connector Planning

The Touchscreen Hub should use a clean connector system.

Possible connector groups:

| Connector | Signals |
| --- | --- |
| Main connector | Power, ground, UART |
| Backlight connector | Backlight enable or power if separate |
| Service connector | Debug or firmware flashing |
| Expansion connector | Spare UART, I2C, GPIO |
| Touchscreen module connector | Display module wiring |
| Interposer connector | Direct link to Button Interposer |

Connector choice affects serviceability.

---

## Connector Requirements

Useful connector traits:

- Small size
- Secure fit
- Clear pin 1 marking
- Keyed if possible
- Easy to source
- Easy enough to assemble
- Good current rating for display power
- Flexible wire routing
- Low enough profile for PS2 shell

Final connectors should be chosen after fitment testing.

---

## Suggested Connector Pinout

A simple first connector could be:

| Pin | Signal | Notes |
| --- | --- | --- |
| 1 | `VCC` | Touchscreen power |
| 2 | `GND` | Ground |
| 3 | `TS_TX` | Touchscreen transmit |
| 4 | `TS_RX` | Touchscreen receive |
| 5 | `WAKE` | Optional |
| 6 | `RESET` | Optional |

This gives room for basic control and future wake/reset support.

---

## Test Points

Useful test points:

- `VCC`
- `GND`
- `TS_TX`
- `TS_RX`
- `WAKE`
- `RESET`
- `BL_EN`
- `SDA`
- `SCL`
- `BOOT`
- `SWDIO`
- `SWCLK`

Test points are especially useful during display bring-up.

---

## Status LED

A small status LED may be useful on the Touchscreen Hub board.

Possible LED meanings:

| LED Pattern | Meaning |
| --- | --- |
| Off | No power or sleep |
| Solid on | Powered |
| Slow blink | Firmware running |
| Fast blink | Communication error |
| Double flash | Command sent |
| Triple flash | Command rejected |
| Breathing | Idle or standby |

The LED should be optional if light bleed is a concern.

---

## Touchscreen UI Hardware Controls

Some touchscreen modules may include physical buttons.

Possible hardware buttons:

- Boot
- Reset
- User button
- Back button
- Service button

If used, these buttons should be documented.

Physical buttons should not conflict with PS2 power/reset behavior.

---

## Board Size Goals

The Touchscreen Hub hardware should fit the display module and the PS2 shell.

Board size goals:

- Fit behind the display
- Avoid excessive thickness
- Avoid RF shield conflict
- Leave room for connectors
- Leave room for strain relief
- Avoid blocking airflow
- Allow service access where practical
- Support clean mounting

A display module may need a separate carrier or adapter board.

---

## Adapter Board Concept

An adapter board may be useful between the touchscreen module and Button Butler wiring.

Possible adapter board functions:

- Break out display module pins
- Provide a clean harness connector
- Add level shifting if needed
- Add backlight control
- Add reset or boot button access
- Add UART connector
- Add mounting holes
- Add test points
- Add FBD silkscreen branding

An adapter board can make the install cleaner and more repeatable.

---

## Carrier Plate Concept

A carrier plate could hold the touchscreen mechanically.

Possible carrier plate functions:

- Secure the display
- Hold the bezel
- Provide screw mounts
- Protect the PCB
- Route wires
- Provide service access
- Align the screen with the shell cutout

Carrier plates may be useful for premium builds.

---

## Bezel And Carrier Combination

The cleanest premium install may use both:

- Front bezel
- Rear carrier plate
- Display module
- Touchscreen adapter board
- Harness to Button Butler

This makes the screen feel like a real product feature instead of a glued-on part.

---

## Insulation

The touchscreen board and carrier need insulation.

Insulation concerns:

- PCB against RF shield
- Display board against metal bracket
- Solder joints near shell cutout
- USB or debug pads near metal
- Backlight power near metal
- Wires passing through cut edge

Possible insulation methods:

- Kapton tape
- Fish paper
- Printed bracket
- Plastic spacer
- Acrylic carrier
- Heat-shrink on exposed joints

Do not allow exposed pads to touch metal.

---

## Wire Strain Relief

Touchscreen wiring needs strain relief.

Possible methods:

- Adhesive wire anchors
- Printed cable channel
- Kapton tape
- Small cable tie points
- Connector locking tabs
- Service loop
- Harness clamp

The display should not pull on tiny solder pads when the shell is opened.

---

## Noise Considerations

Touchscreen wiring can be affected by noise.

Possible noise sources:

- Switching regulators
- Video boards
- Internal router
- ESP32 modules
- RGB lighting
- Optical drive motor
- Long signal wires
- Poor grounding

Noise reduction methods:

- Keep UART wires short
- Route signals away from power switching
- Use series resistors if needed
- Share ground properly
- Add decoupling
- Use proper power filtering
- Avoid running display wires over hot/noisy boards

Touchscreen communication should remain reliable after final assembly.

---

## Backfeeding Risks

Backfeeding is a concern if the touchscreen is powered differently from the interposer.

Possible backfeeding paths:

- UART TX/RX
- Wake line
- Reset line
- I2C pull-ups
- Backlight enable line
- USB programming cable
- Debug adapter
- Touchscreen module pull-ups

Backfeeding can partially power one board through another.

---

## Backfeeding Prevention

Ways to reduce backfeeding:

- Use series resistors on UART lines
- Use proper level shifting
- Use open-drain signals where appropriate
- Avoid pull-ups to an unpowered rail
- Power related communication devices from compatible rails
- Add isolation if needed
- Test for voltage on unpowered rails
- Avoid leaving USB connected while console power is off unless designed for it

Backfeeding should be tested during bench work.

---

## Logic Level Considerations

The touchscreen and interposer must use compatible logic.

Possible voltage combinations:

| Touchscreen Logic | Interposer Logic | Needed Action |
| --- | --- | --- |
| 3.3V | 3.3V | Direct connection may be okay |
| 3.3V | 5V | Level shifting may be needed |
| 5V | 3.3V | Level shifting required |
| 5V tolerant input | 5V output | Confirm datasheet first |

Do not guess logic compatibility.

Check the actual hardware.

---

## Grounding

The touchscreen and interposer need a shared ground.

Grounding notes:

- UART needs common ground.
- Touchscreen power needs a solid return path.
- Display backlight current should not disturb sensitive signals.
- Avoid skinny ground wires for display power if current is high.
- Avoid noisy ground paths through video or audio circuits.
- Label ground clearly.

Bad grounding can cause touch glitches or UART errors.

---

## Decoupling And Filtering

The Touchscreen Hub may need local decoupling.

Possible components:

- 0.1 uF ceramic near logic power
- Bulk capacitor for display/backlight
- Ferrite bead if display power is noisy
- Backlight filtering if needed
- Separate decoupling for adapter board ICs

The exact values depend on the display module and power source.

---

## Supported Build Types

The Touchscreen Hub may be used in several build types.

Possible build types:

| Build Type | Touchscreen Use |
| --- | --- |
| Basic Button Butler | Usually not installed |
| ChipSlayer build | Optional status/control screen |
| BlueRetro build | Controller control/status page |
| Bluetooth audio build | Pairing and audio control |
| SD2PSX build | Status display |
| Premium Ultra Slim | Full control hub |
| PS2 Blue Steel-style build | Main design feature |
| Service/test build | Diagnostic and output testing |

Touchscreen hardware should be optional.

---

## PS2 Slim Fitment Notes

PS2 Slim fitment may be challenging.

Slim concerns:

- Limited vertical clearance
- RF shield interference
- Optical drive clearance
- Fan area clearance
- Controller port area crowding
- Memory card slot area
- Existing HDMI board
- BlueRetro board placement
- SD2PSX placement
- Shell strength after cutting

A Slim touchscreen install needs careful planning.

---

## PS2 Fat Fitment Notes

PS2 Fat consoles may offer more room but still need planning.

Fat concerns:

- Different shell shape
- Different button location
- Front panel layout
- Top shell structure
- HDD bay and network adapter area
- Cooling path
- Internal shielding
- Build aesthetics

Fat support may need separate mounting notes.

---

## Ultra Slim Fitment Notes

FBD Ultra Slim-style builds are a strong target for the Touchscreen Hub.

Ultra Slim goals:

- Keep external controls minimal
- Use the touchscreen as the main premium control point
- Keep build compact
- Hide wiring
- Integrate with BlueRetro
- Integrate with SD2PSX
- Integrate with HDMI board power control
- Support custom top-shell aesthetics

The touchscreen can become a major identity feature of premium FBD builds.

---

## Custom Shell Fitment Notes

Future custom shells may be designed around the touchscreen.

Possible custom shell features:

- Built-in round display recess
- Integrated bezel
- Screw-mounted display carrier
- Hidden service access
- Internal wire channel
- Display window
- Replaceable top plate
- Theme-specific faceplate

Custom shells can make the touchscreen look more factory-like.

---

## Bench Testing Plan

Before installing in a console, bench test the Touchscreen Hub.

Bench tests:

1. Visual inspection
2. Power rail check
3. Current draw measurement
4. Display power-up test
5. Backlight test
6. Touch input test
7. Touch coordinate test
8. UART TX test
9. UART RX test
10. Interposer communication test
11. Sleep/wake test if supported
12. Reset test if supported
13. Backfeeding test
14. Heat check

Do not cut a shell until bench tests are successful.

---

## Console Testing Plan

After bench testing, test in a real console.

Console tests:

1. Confirm console works before touchscreen install
2. Confirm screen mounting location
3. Confirm internal clearance
4. Temporarily route wiring
5. Confirm display powers correctly
6. Confirm UART communication
7. Confirm touch still works after mounting
8. Confirm shell closes without pressure
9. Confirm no noise or UART errors
10. Confirm no backfeeding
11. Confirm sleep/wake behavior
12. Confirm command confirmation behavior
13. Confirm no accidental reset or power commands
14. Confirm heat is acceptable

Touchscreen install should be tested before final finishing work.

---

## Fitment Testing

Fitment testing should include:

- Screen depth check
- Connector height check
- Wire bend radius check
- Shell closure test
- RF shield clearance
- Board clearance
- Screw post clearance
- Touch surface alignment
- Bezel alignment
- Service access check

A screen that barely fits may fail after final assembly.

---

## Touch Accuracy Testing

Touch accuracy should be tested after mounting.

Tests:

- Touch center button
- Touch edge buttons
- Touch all navigation buttons
- Touch confirm/cancel buttons
- Test with shell fully assembled
- Test after warm-up
- Test after sleep/wake
- Test with protective cover if used

Touch targets may need to be larger than expected.

---

## Power Testing

Power testing should include:

- Startup current
- Idle current
- Full-brightness current
- Sleep current
- Backlight current
- Current with UART active
- Current with touchscreen idle
- Heat after long runtime

Save measurements in:

`Test-Data/Power-Measurements/`

---

## Heat Testing

Heat testing should include:

- Screen at full brightness
- Screen dimmed
- Screen asleep
- Console idle
- Console during gameplay
- Console shell closed
- Nearby video board active
- Internal router active if installed

Heat data should be saved in:

`Test-Data/Heat-Testing/`

---

## UART Testing

UART testing should include:

- `PING`
- `STATUS`
- `VERSION?`
- Button state request
- Lid state request
- Output command
- Command rejected response
- Interposer restart event
- Touchscreen reconnect behavior
- Invalid response handling
- Disconnected interposer behavior

UART logs should be saved in:

`Test-Data/UART-Logs/`

---

## Suggested Folder Structure

| Path | Purpose |
| --- | --- |
| `KiCad/` | Adapter board or carrier PCB files |
| `Schematics/` | Exported schematic PDFs or images |
| `Wiring/` | Wiring diagrams and pinouts |
| `Images/` | Board renders, display mockups, install photos |
| `Mounting/` | Bezel, bracket, carrier, or fitment notes |
| `BOM/` | Touchscreen hardware BOM files |
| `Test-Notes/` | Bench and console test notes |
| `README.md` | Hardware overview and planning notes |

Additional folders can be added later.

---

## File Naming

Suggested file naming:

| File | Purpose |
| --- | --- |
| `FBD-PS2-Button-Butler-Touchscreen-Hub.kicad_pro` | KiCad project if adapter board is made |
| `FBD-PS2-Button-Butler-Touchscreen-Hub-v0.1.pdf` | Schematic export |
| `FBD-PS2-Button-Butler-Touchscreen-Hub-v0.1-Gerbers.zip` | Prototype Gerbers |
| `FBD-PS2-Button-Butler-Touchscreen-Hub-v0.1-BOM.csv` | BOM |
| `Touchscreen-Pinout-v0.1.md` | Pinout notes |
| `Touchscreen-Mounting-v0.1.md` | Mounting notes |
| `Touchscreen-Wiring-v0.1.md` | Wiring notes |

Use consistent naming so revisions are easy to track.

---

## Revision Naming

Suggested revision naming:

| Revision | Meaning |
| --- | --- |
| `v0.0` | Concept only |
| `v0.1` | First wiring or adapter concept |
| `v0.2` | First bench-tested display setup |
| `v0.3` | First console-fitment test |
| `v0.4` | Touchscreen communication test |
| `v0.5` | Improved prototype |
| `v1.0` | First stable release candidate |

Do not call a touchscreen setup stable until it has been tested in an assembled console.

---

## BOM Notes

The BOM should include:

- Touchscreen module
- Controller board if separate
- Adapter PCB if used
- Connector part numbers
- Cable or wire type
- Bezel material
- Mounting hardware
- Regulator if used
- Level shifter if used
- Capacitors or filtering parts
- Screws or standoffs
- Adhesive or insulation materials

Mechanical parts are part of the touchscreen hardware BOM.

---

## Component Selection Goals

Component selection should prioritize:

- Availability
- Clear documentation
- Stable supply
- Good display quality
- Reliable touch input
- Reasonable power draw
- Easy firmware flashing
- Practical mounting
- Small connector profile
- Serviceability
- Compatibility with Button Butler voltage levels

Avoid touchscreen hardware that cannot be mounted cleanly.

---

## Mechanical Documentation

Touchscreen mechanical documentation should include:

- Display diameter
- Display board dimensions
- Total module thickness
- Connector location
- Cable exit direction
- Mounting hole locations
- Bezel dimensions
- Shell cutout size
- Internal clearance
- Screw or bracket details

Mechanical fitment is just as important as electronics.

---

## UI Hardware Documentation

Touchscreen hardware docs should also include:

- Display resolution
- Display driver
- Touch controller
- Logic voltage
- Power current
- UART pins
- I2C or SPI pins if used
- Backlight control pin
- Reset pin
- Boot pin
- Programming method
- Known quirks

This information should be saved before firmware work gets complicated.

---

## QA Checklist Ideas

Future QA checklist items:

- Visual inspection
- Confirm display powers on
- Confirm touch works
- Confirm touch calibration
- Confirm UART connection
- Confirm command response handling
- Confirm no accidental commands
- Confirm sleep/wake
- Confirm brightness control
- Confirm mounting is secure
- Confirm shell closes cleanly
- Confirm no backfeeding
- Confirm current draw
- Confirm heat is acceptable
- Confirm service access
- Record firmware and hardware version

A detailed QA checklist should eventually live in:

`Manufacturing/QA-Checklist.md`

---

## Known Issues

Current known issues:

- Touchscreen hardware is not finalized.
- Mounting method is not finalized.
- Connector style is not finalized.
- Power source is not finalized.
- Controller MCU is not finalized.
- UART assignment is not finalized.
- Touchscreen adapter board is not designed.
- Backlight control method is not finalized.
- Fitment is not tested.
- No bench test has been completed.
- No console test has been completed.

This section should be updated as testing begins.

---

## Open Questions

Open hardware questions:

- Which touchscreen module should be used first?
- Should the touchscreen be round for all premium builds?
- Should the touchscreen controller be RP2040, RP2350, ESP32, or another MCU?
- Should the touchscreen be powered from standby power or switched power?
- Should the display backlight have separate power control?
- Should the touchscreen act as the UART hub?
- How many UARTs are actually needed?
- Should BlueRetro and SD2PSX connect directly to the touchscreen?
- Should there be a touchscreen adapter PCB?
- What connector style is best for the display harness?
- What is the cleanest mounting method for Slim builds?
- What is the cleanest mounting method for Ultra Slim builds?
- Should the screen be removable or permanently mounted?
- Should service access be available after final assembly?
- Should the touchscreen have a force-safe disconnect mode?

These questions should be answered through prototype testing.

---

## Risks

Possible hardware risks:

- Screen does not fit cleanly
- Shell cut looks bad
- Touch input is inaccurate after mounting
- Display draws too much power
- Display adds heat
- UART communication is unreliable
- Logic levels do not match
- Backfeeding through UART or USB
- Screen failure affects console operation
- Wires get pinched during shell closure
- Touchscreen is hard to service
- Display becomes unreadable in final location
- UI buttons are too small
- Feature creep delays the base Button Butler system

The touchscreen should be added only after the base interposer is reliable.

---

## Recommended Development Order

Recommended development order:

1. Choose a touchscreen module for testing.
2. Power it on outside the console.
3. Confirm display and touch input.
4. Measure current draw.
5. Test basic firmware.
6. Test UART communication with a serial adapter.
7. Test UART communication with Button Interposer firmware.
8. Create a simple adapter or harness.
9. Test screen fitment in a spare shell.
10. Create a rough mounting bracket or bezel.
11. Test touch accuracy after mounting.
12. Test shell closure.
13. Add simple UI commands.
14. Add service page later.
15. Add premium pages only after core communication is stable.

Do not cut a final shell until fitment and function are proven.

---

## Minimum First Prototype

A good first Touchscreen Hub prototype could include:

- Touchscreen module
- Power input
- Ground
- UART TX/RX to Button Interposer
- Simple harness
- Temporary mount or bracket
- Basic firmware
- One test screen
- One test button
- One status display
- One safe command

This is enough to prove the touchscreen layer.

---

## Summary

The Touchscreen Hub is the premium user-interface hardware for the FBD PS2 Button Butler project.

Its job is to provide a clean visual control point for advanced PS2 builds while keeping the Button Interposer Board responsible for safety-critical console behavior.

The touchscreen should help reduce external switches, buttons, LEDs, and shell clutter.

The first hardware prototype should focus on display bring-up, touch input, UART communication, power measurement, and fitment testing.

Advanced pages for ChipSlayer, modchip control, BlueRetro, Bluetooth audio, SD2PSX, RGB lighting, video power, internal router control, service diagnostics, and future laser feedback can be added after the basic touchscreen hardware is proven.
