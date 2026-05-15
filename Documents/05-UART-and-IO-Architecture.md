# 05 - UART And I/O Architecture

## Overview

The **FBD PS2 Button Butler** project is intended to connect several internal PS2 mods, control boards, and optional display features together in a clean and manageable way.

This document covers the early UART and digital I/O architecture ideas for the project.

The goal is to keep communication simple, reliable, and serviceable while still allowing Button Butler to grow from a simple button interposer into a larger touchscreen-based control system.

---

## Main Purpose

The purpose of the UART and I/O architecture is to give Button Butler a clean way to:

- Read button states
- Read lid switch state
- Control outputs
- Talk to the touchscreen
- Send commands to other internal boards
- Receive status from other internal boards
- Control mods without adding extra external switches
- Support future expansion

The system should be easy to debug with basic tools such as a multimeter, logic analyzer, or USB-to-serial adapter.

---

## Design Goals

The communication and I/O design should follow these goals:

1. Keep the base system simple
2. Avoid unnecessary complexity
3. Use reliable point-to-point communication where possible
4. Avoid fragile multi-drop UART wiring unless specifically designed for it
5. Make each signal easy to test
6. Use clear labels and connectors
7. Keep safety-critical power/reset behavior on the interposer board
8. Allow the touchscreen to request actions, not directly force them
9. Keep raw I/O available for simple mod control
10. Allow future expansion without redesigning the entire project

---

## Main Hardware Blocks

The Button Butler architecture may include several hardware blocks.

| Block | Purpose |
| --- | --- |
| Button Interposer Board | Handles PS2 power/reset button behavior and core safety logic |
| Touchscreen Controller | Provides user interface and sends requests to the interposer |
| I/O Expansion Board | Provides extra raw inputs and outputs |
| Mod Control Outputs | Control ChipSlayer, modchip enable, Bluetooth audio, RGB, and other features |
| UART Devices | Devices that can report status or accept commands |
| Power Management Section | Controls accessory power rails or video board power |

Not every build will use every block.

The architecture should scale depending on the build level.

---

## Recommended Control Philosophy

The recommended control philosophy is:

- The **Button Interposer Board** handles critical console button behavior.
- The **Touchscreen Controller** acts as the user interface.
- The **I/O Expansion Board** provides extra inputs and outputs.
- External mods are controlled through simple enable lines, trigger lines, or UART commands when available.
- Safety-critical actions are approved by the interposer before being executed.

The touchscreen should not directly control the PS2 power/reset line by itself.

The interposer should remain the final authority for console power/reset behavior.

---

## Why The Interposer Should Be The Core Controller

The interposer board is closest to the original PS2 power/reset button circuit.

Because of that, it should handle:

- Button input
- Button debounce
- Short press detection
- Long press detection
- Lid switch detection
- Pass-through behavior
- Blocked button behavior
- Console reset or power request behavior
- Safe default states
- Basic output control
- Touchscreen command validation

This keeps the most important logic in one predictable place.

---

## UART Basics

UART is a simple serial communication method.

It usually uses:

- TX
- RX
- Ground
- Optional power reference
- Optional enable or wake line

UART is normally point-to-point.

That means one device TX connects to the other device RX, and the second device TX connects back to the first device RX.

---

## Important UART Rule

Standard UART should not be treated like a normal shared bus unless the hardware and firmware are designed for that.

In most cases:

- One TX should not drive several RX/TX devices at the same time unless only one device talks back.
- Multiple TX lines should not be tied together directly.
- UART devices should not all talk at the same time.
- Multi-device UART needs switching, muxing, tri-state control, or a proper bus design.

For Button Butler, point-to-point UART links are preferred whenever possible.

---

## Possible UART Devices

Button Butler may eventually communicate with several UART-capable devices.

Possible UART devices:

- Touchscreen controller
- Button interposer PIC
- BlueRetro-related controller
- SD2PSX-related controller
- Bluetooth audio board
- Mechacon UART
- Emotion Engine UART
- A5-V11 or internal router controller
- Service/debug header
- Future LazyrSavre-style diagnostic board

Not all of these should be connected at the same time in the first version.

The first prototype should use only the UART links needed to prove the system.

---

## UART Architecture Options

There are several possible UART architecture options.

| Option | Description | Notes |
| --- | --- | --- |
| Direct Point-To-Point | One UART per device | Cleanest and easiest to debug |
| UART Mux | One controller UART switched between devices | Useful when only one device needs to be accessed at a time |
| Software Serial | Extra UARTs created in firmware | Can work but may be less reliable |
| Multi-UART MCU | Controller has several hardware UARTs | Best for touchscreen hub style builds |
| RS-485 Style Bus | Shared differential bus | More robust but more complex |
| Open-Drain UART Bus | Shared line with pull-up and controlled talkers | Possible but needs careful design |
| I2C Expansion | Shared bus for I/O chips | Useful for local board expansion, not a UART replacement |

For early Button Butler development, direct UART and simple muxing are preferred.

---

## Preferred Early UART Layout

The preferred early layout is simple:

| Link | Purpose |
| --- | --- |
| Interposer UART to Touchscreen | Main control and status link |
| Interposer UART to Debug Header | Firmware debugging and testing |
| Touchscreen UART to SD2PSX or BlueRetro | Optional status/control link depending on build |
| Touchscreen UART through Mux | Optional access to lower-priority devices |

The first version does not need to connect every possible device.

Start with the interposer and touchscreen communication first.

---

## Touchscreen As A UART Hub

For advanced builds, the touchscreen controller may act as a communication hub.

This is especially useful if the touchscreen module has multiple hardware UARTs.

Possible touchscreen hub connections:

| Touchscreen UART | Possible Device |
| --- | --- |
| UART 0 | Programming or debug |
| UART 1 | Button Interposer Board |
| UART 2 | BlueRetro-related controller |
| UART 3 | SD2PSX-related controller |
| Muxed UART | Mechacon, Emotion Engine, A5-V11, or service devices |

This allows the touchscreen to collect information and present it in a clean interface.

---

## Interposer UART Role

The interposer UART should be simple and reliable.

Possible uses:

- Report button events
- Report lid switch state
- Report output states
- Receive touchscreen commands
- Receive debug commands
- Confirm command acceptance
- Reject unsafe commands
- Report firmware version
- Report hardware revision
- Enter service mode if allowed

The interposer does not need to understand every external device.

Its main job is console button behavior and safe output control.

---

## Touchscreen UART Role

The touchscreen UART role is more user-interface focused.

Possible uses:

- Request console power action
- Request console reset action
- Request output toggle
- Request current state
- Display received status
- Show errors
- Show warnings
- Control user menus
- Communicate with optional devices
- Forward safe commands when appropriate

The touchscreen should not assume an action worked unless the interposer confirms it.

---

## I/O Expansion Role

The I/O Expansion Board is intended to provide extra raw control points.

Possible uses:

- Turn LEDs on or off
- Enable or disable RGB lighting
- Trigger Bluetooth audio pairing
- Enable or disable Bluetooth audio
- Enable or disable ChipSlayer
- Enable or disable modchip control circuit
- Control accessory power switches
- Read extra switches or jumpers
- Read service inputs
- Provide spare outputs for future mods

The I/O Expansion Board keeps the base interposer from becoming too crowded.

---

## Digital Input Types

Button Butler may use several types of digital inputs.

Possible inputs:

- Power/reset button input
- Lid switch input
- Service jumper
- Touchscreen wake input
- Accessory status input
- Mod state feedback input
- BlueRetro enable feedback
- Bluetooth audio status
- ChipSlayer state feedback
- Video board power-good signal
- User-defined input

Inputs should have defined pull-up or pull-down states.

Floating inputs should be avoided.

---

## Digital Output Types

Button Butler may use several types of outputs.

Possible outputs:

- Push-pull logic output
- Open-drain output
- Open-collector style output
- MOSFET low-side control
- MOSFET high-side enable
- Relay control
- LED output
- Buzzer or chime output
- Accessory enable line
- UART wake line

The output type should match the device being controlled.

---

## Open-Drain Style Outputs

Open-drain or open-collector style outputs are useful when Button Butler only needs to pull a signal low.

Possible uses:

- Pulling an enable line low
- Triggering a reset input
- Simulating a button press
- Sharing a line with another circuit
- Avoiding direct voltage conflict

Open-drain outputs are often safer for mixed-signal control because they avoid forcing a line high against another device.

---

## Push-Pull Outputs

Push-pull outputs can actively drive high and low.

Possible uses:

- Driving logic inputs with known voltage compatibility
- Controlling local logic
- Driving enable pins where the voltage domain is known
- Talking to devices on the same logic level

Push-pull outputs should be used carefully when connecting to PS2 signals or third-party boards.

If there is any risk of contention, open-drain style control may be safer.

---

## MOSFET Controlled Outputs

MOSFET outputs may be needed when controlling power or loads.

Possible uses:

- LED power
- RGB lighting
- Accessory power enable
- Bluetooth audio power enable
- Video board power enable
- Relay coil control
- Buzzer power

MOSFET outputs should include proper gate resistors, pull-downs, and flyback protection where needed.

---

## Output Default States

Every output should have a known default state.

Possible default states:

| Output Type | Recommended Default |
| --- | --- |
| Mod enable output | Defined off or pass-through safe state |
| ChipSlayer control | Defined safe state |
| Bluetooth pairing trigger | Inactive |
| RGB lighting | Off or last saved state |
| Accessory power | Off unless required |
| Reset trigger | Inactive |
| Buzzer/chime | Off |
| Open-drain control | High impedance unless active |

No output should float into an accidental active state.

---

## Logic Level Considerations

Different devices may use different logic levels.

Possible logic levels:

- 3.3V logic
- 5V logic
- PS2 internal logic levels
- Accessory board logic levels
- UART adapter logic levels

Important rules:

- Do not connect 5V logic directly to a 3.3V-only input unless it is 5V tolerant.
- Use level shifting where needed.
- Share ground between UART devices.
- Avoid backfeeding unpowered boards through signal lines.
- Use series resistors where helpful.
- Check every connected device before wiring.

Button Butler should be designed around known voltage domains.

---

## Grounding

UART and digital I/O signals need a common ground reference.

Important grounding notes:

- UART devices must share ground.
- Control outputs need a return path.
- Avoid long messy ground loops where possible.
- Keep high-current accessory grounds away from sensitive logic where possible.
- Use good grounding practice for internal PS2 installs.
- Label ground pads clearly.

Bad grounding can make UART communication unreliable and cause random behavior.

---

## Backfeeding Risks

Backfeeding is a major concern in a multi-board PS2 mod system.

Backfeeding can happen when one powered board sends voltage into another board that is turned off.

Possible backfeeding paths:

- UART TX line
- Pull-up resistors
- Enable lines
- LED lines
- I/O pins
- Accessory power-good signals
- Touchscreen communication pins

Ways to reduce backfeeding:

- Use series resistors
- Use open-drain outputs
- Use level shifters with proper power domains
- Use MOSFET isolation where needed
- Use high-impedance defaults
- Avoid powering signal lines before the receiving board is powered
- Design outputs so inactive means safe

Backfeeding should be considered from the start.

---

## UART Muxing

A UART mux can allow one UART port to connect to several devices one at a time.

This is useful when some devices only need occasional access.

Possible muxed devices:

- Mechacon UART
- Emotion Engine UART
- A5-V11 router control UART
- Service/debug UART
- Optional diagnostic board

The mux should ensure only one selected device can talk to the controller at a time.

---

## When A UART Mux Makes Sense

A UART mux makes sense when:

- Devices do not need live updates all the time
- Only one device is accessed at a time
- The controller has limited UART ports
- The extra devices are mainly for service or status checks
- The firmware can control the mux select lines safely

For example, Mechacon and Emotion Engine UART access may not need to be live all the time.

They could share a muxed UART if only one is selected when needed.

---

## When A UART Mux Does Not Make Sense

A UART mux may not be ideal when:

- A device needs constant communication
- Two devices must talk at the same time
- The mux switching could interrupt important data
- The device sends unsolicited data constantly
- The firmware cannot reliably manage selection
- Debugging would become too confusing

Devices that need frequent status updates may deserve their own UART.

---

## Possible UART Priority

Possible UART priority for advanced builds:

| Priority | Device | Reason |
| --- | --- | --- |
| High | Button Interposer | Core console control |
| High | Touchscreen | Main user interface |
| Medium | BlueRetro | User-facing controller status/control |
| Medium | SD2PSX | User-facing status/control |
| Low | Bluetooth audio | Often simple trigger/control only |
| Low | Mechacon UART | Service or diagnostic use |
| Low | Emotion Engine UART | Service or diagnostic use |
| Low | A5-V11 UART | Router/service use |

This priority may change depending on the build.

---

## BlueRetro Communication

Button Butler may support BlueRetro in more than one way.

Possible BlueRetro control methods:

- Simple enable/disable line
- Reset line
- Channel disable lines
- UART status if available
- Separate BlueRetro-related controller
- Touchscreen page for user control

Early support may only use simple digital I/O.

Advanced support can be added later if reliable communication is available.

---

## SD2PSX Communication

SD2PSX support may also be staged.

Possible SD2PSX support methods:

- Status input
- UART status if available
- Game ID display if available
- Touchscreen status page
- Build-specific control options
- Future communication protocol support

Early support may only display basic status or reserve a page for future use.

The system should not depend on SD2PSX communication for basic console power/reset behavior.

---

## Bluetooth Audio Communication

Bluetooth audio boards may not need full UART integration.

Possible control methods:

- Enable line
- Pairing trigger line
- Reset line
- Status input
- UART if the board supports useful commands

For early versions, simple I/O control may be enough.

Button Butler can trigger pairing without adding an external button.

---

## RetroGEM And ElectronAnalog Power Control

Button Butler may control power to internal video hardware.

Possible control methods:

- Accessory power enable line
- MOSFET power switch
- Power-good input
- Idle detection
- Touchscreen manual override
- Firmware-controlled timeout

Possible goals:

- Reduce heat
- Save energy
- Disable video board when not needed
- Keep compact builds cooler
- Coordinate video hardware with console state

This should be tested carefully so video hardware is not power-cycled in a harmful way.

---

## ChipSlayer Control

ChipSlayer is one of the most important control targets.

Possible control methods:

- Dedicated enable line
- Dedicated disable line
- Open-drain control
- Output latch
- Touchscreen toggle
- Lid switch plus button shortcut
- State feedback input if available

Button Butler should be able to control ChipSlayer without needing a separate external switch.

---

## Modchip Control

Modchip control may be handled through a dedicated control circuit.

Possible functions:

- Enable modchip
- Disable modchip
- Disable modchip for one boot
- Toggle modchip state from touchscreen
- Toggle modchip state using hidden button behavior
- Display modchip state

Modchip control must be designed carefully so the console does not boot into an unsafe or confusing state.

---

## RGB And Lighting Control

RGB and lighting control may use raw I/O or a dedicated controller.

Possible control methods:

- Simple on/off output
- PWM outputs
- Addressable LED controller
- External RGB controller enable line
- Touchscreen color selection
- Build-specific lighting profiles

Early versions should start with simple on/off or enable control.

Advanced RGB effects can come later.

---

## Chime Or Buzzer Control

Button Butler may support a chime or buzzer output.

Possible uses:

- Startup chime
- Shutdown chime
- Button accepted tone
- Error tone
- Pairing mode tone
- Service mode tone

Possible output methods:

- Passive buzzer driven by MCU
- Active buzzer controlled by output
- Small audio module
- Bluetooth audio board trigger

Chime feedback should be optional.

---

## I/O Expansion Board Options

The I/O Expansion Board could be designed in several ways.

Possible options:

| Option | Description |
| --- | --- |
| PIC-Based I/O Board | Small MCU handles local inputs and outputs |
| I2C I/O Expander | Simple chip provides extra GPIO over I2C |
| Shift Register Outputs | Adds many outputs with few pins |
| Direct GPIO Board | Passive breakout of MCU pins |
| MOSFET Driver Board | Focused on switching loads |
| LED/RGB Board | Focused on lighting control |

A PIC-based board may fit the FBD style because it can be simple, flexible, and firmware-controlled.

---

## PIC-Based I/O Expansion

A PIC-based I/O expansion board could provide:

- Extra digital outputs
- Extra digital inputs
- Output defaults
- Local state control
- UART or I2C command interface
- Watchdog behavior
- LED or chime control
- Simple autonomous behavior

This allows some outputs to keep working even if the touchscreen is not active.

---

## I2C Expansion

I2C can be useful for adding local I/O devices.

Possible I2C devices:

- GPIO expanders
- LED drivers
- Temperature sensors
- EEPROM
- Small peripheral chips

I2C is a shared bus, but it should be kept short and clean inside the console.

Important I2C notes:

- Requires pull-up resistors
- Devices need unique addresses
- Long wires can cause reliability issues
- Noise can cause communication errors
- Voltage level compatibility matters

I2C may be useful inside one board stack, but UART is likely better for board-to-board service/debug links.

---

## Direct GPIO Control

Some features may not need a communication protocol.

Simple direct GPIO control is often best for:

- Enable lines
- Pairing triggers
- Reset triggers
- LED control
- Simple mode select lines
- ChipSlayer control
- Accessory power enable

Direct I/O is easy to test and reliable when wired correctly.

Not every feature needs UART.

---

## Command Structure Concept

Button Butler may use a simple command structure over UART.

The exact format can be defined later, but each command should include:

- Command type
- Target
- Requested action
- Optional value
- Response or acknowledgement
- Error reporting

The protocol should be readable and easy to debug.

A human-readable protocol may be useful during early development.

A compact binary protocol can be considered later if needed.

---

## Human-Readable UART Commands

Human-readable commands are useful during early testing.

Possible command ideas:

| Command Idea | Meaning |
| --- | --- |
| `PING` | Check if device responds |
| `STATUS` | Request current state |
| `BTN?` | Request button state |
| `LID?` | Request lid switch state |
| `OUT1 ON` | Turn output 1 on |
| `OUT1 OFF` | Turn output 1 off |
| `OUT1 TOGGLE` | Toggle output 1 |
| `RESET REQ` | Request console reset |
| `POWER REQ` | Request console power action |
| `SERVICE ON` | Request service mode |
| `SERVICE OFF` | Exit service mode |

These are only early ideas.

---

## Command Response Concept

Each command should receive a response.

Possible responses:

| Response | Meaning |
| --- | --- |
| `OK` | Command accepted |
| `ERR` | Command rejected or failed |
| `BUSY` | Device is busy |
| `LOCKED` | Action is locked |
| `UNKNOWN` | Command not recognized |
| `UNSAFE` | Command blocked for safety |
| `STATE` | State data follows |
| `VERSION` | Firmware or hardware version data follows |

The touchscreen should use these responses to update the UI.

---

## Event Reporting Concept

Button Butler may send events when something changes.

Possible events:

| Event | Meaning |
| --- | --- |
| `EVT BTN_PRESS` | Button was pressed |
| `EVT BTN_RELEASE` | Button was released |
| `EVT SHORT_PRESS` | Short press detected |
| `EVT LONG_PRESS` | Long press detected |
| `EVT LID_OPEN` | Lid changed to open |
| `EVT LID_CLOSED` | Lid changed to closed |
| `EVT OUT_CHANGE` | Output state changed |
| `EVT ERROR` | Error state occurred |
| `EVT SERVICE` | Service mode changed |

Event messages are useful for the touchscreen and for debugging.

---

## Addressing Concept

If multiple devices are connected through a hub, commands may need targets.

Possible target names:

| Target | Meaning |
| --- | --- |
| `BB` | Button Butler interposer |
| `IO` | I/O expansion board |
| `TS` | Touchscreen |
| `BR` | BlueRetro-related controller |
| `SD` | SD2PSX-related controller |
| `BT` | Bluetooth audio controller |
| `VID` | Video power controller |
| `RGB` | Lighting controller |

Target names should be short and readable.

---

## Example Targeted Command Ideas

Possible targeted command ideas:

| Command Idea | Meaning |
| --- | --- |
| `BB STATUS` | Ask interposer for status |
| `IO OUT1 ON` | Turn I/O expansion output 1 on |
| `BT PAIR` | Trigger Bluetooth audio pairing |
| `BR DISABLE` | Disable BlueRetro control output |
| `SD STATUS` | Request SD2PSX status |
| `VID OFF` | Turn video accessory power off |
| `RGB OFF` | Turn lighting off |

These are early protocol ideas and may change.

---

## Firmware Responsibility Split

The firmware responsibilities should be split clearly.

| Firmware Area | Responsibility |
| --- | --- |
| Interposer Firmware | Button behavior, safety logic, basic I/O, command validation |
| Touchscreen Firmware | Menus, display, touch input, command requests, status display |
| I/O Expansion Firmware | Extra input/output handling, output defaults, local control |
| Device-Specific Firmware | BlueRetro, SD2PSX, Bluetooth audio, or other device logic |

Keeping responsibilities separated makes the project easier to debug.

---

## Safety-Critical Signals

Some signals should be treated as safety-critical.

Safety-critical signals may include:

- PS2 power/reset control
- Console reset line
- Accessory power enable
- Modchip enable state
- ChipSlayer state
- Video board power enable
- Any line that can prevent booting
- Any line that can hold the console in reset
- Any line that can backfeed power

These signals should have defined default states and safe firmware behavior.

---

## Non-Critical Signals

Some signals are less critical.

Non-critical signals may include:

- Decorative LEDs
- RGB lighting
- Chime output
- Status indicators
- Display wake line
- Debug outputs

Non-critical signals still need good design, but they should not be able to stop the console from working.

---

## Startup Behavior

All boards should start in a predictable state.

Recommended startup behavior:

1. Outputs default to safe inactive states
2. MCU initializes GPIO direction
3. UART starts after GPIO is safe
4. Interposer reads button and lid state
5. Interposer reports ready state
6. Touchscreen waits for interposer response
7. Touchscreen loads build profile
8. Optional devices are checked
9. User interface becomes active

No board should blindly activate outputs during startup.

---

## Shutdown Behavior

Shutdown behavior should also be defined.

Possible shutdown behavior:

- Save current state if needed
- Turn off nonessential outputs
- Disable accessory power if safe
- Keep critical defaults stable
- Put touchscreen to sleep
- Leave console button behavior in safe state
- Avoid backfeeding through signal lines

The console should not get stuck because an accessory shut down first.

---

## Watchdog Behavior

A watchdog may be useful for the interposer and I/O expansion boards.

Possible watchdog behavior:

- Reset firmware if it locks up
- Return outputs to safe states
- Notify touchscreen after restart
- Avoid holding console reset active
- Avoid repeating power commands after reset

Watchdog behavior must be tested carefully.

A watchdog reset should not create a random button press or output pulse.

---

## Debug Headers

Debug headers are important for development.

Possible debug header signals:

- Ground
- 3.3V or 5V reference
- UART TX
- UART RX
- Reset
- Programming pins
- Test output
- Test input

Headers should be clearly labeled.

If space is limited, small pads can be used instead of full headers.

---

## Test Points

Useful test points may include:

- Button input
- Console-side button output
- Lid switch input
- MCU power
- Ground
- UART TX
- UART RX
- Output lines
- Reset line
- Accessory enable lines

Good test points make troubleshooting much easier.

---

## Connector Planning

The project should use connectors where practical.

Possible connector groups:

| Connector | Possible Signals |
| --- | --- |
| Button connector | Button input and console output |
| Lid connector | Lid switch input and ground/reference |
| UART connector | TX, RX, ground, power reference |
| I/O connector | Digital outputs and inputs |
| Power connector | Accessory power and ground |
| Touchscreen connector | UART, power, reset/wake, ground |
| Debug connector | Programming and serial debug |

Connector pinouts should be documented early and kept consistent when possible.

---

## Labeling Rules

Every board should have clear labels.

Recommended labels:

- VIN
- 3V3
- 5V
- GND
- BTN_IN
- BTN_OUT
- LID
- TX
- RX
- OUT1
- OUT2
- IN1
- IN2
- RST
- EN
- WAKE
- DBG

Clear labels reduce install mistakes.

---

## Noise Considerations

The inside of a PS2 can be electrically noisy.

Possible noise sources:

- Motors
- Optical drive activity
- Switching regulators
- Video hardware
- Wireless modules
- Long wires
- Poor grounding
- RGB lighting

Ways to improve reliability:

- Keep UART wires short
- Twist signal and ground where helpful
- Avoid routing signal wires near noisy power wiring
- Add series resistors where useful
- Use decoupling capacitors
- Use proper ground references
- Use shielded cable only if truly needed
- Keep high-current switching away from sensitive inputs

---

## Recommended Early Prototype Architecture

The first prototype should stay simple.

Recommended first prototype:

| Function | Recommended Method |
| --- | --- |
| Button input | Direct input to interposer MCU |
| Console output | Controlled pass-through or open-drain style output |
| Lid switch | Direct input to interposer MCU |
| Debug | UART header |
| Touchscreen | Optional second UART or shared debug UART during testing |
| Outputs | One or two simple digital outputs |
| Expansion | Header only, no complex bus yet |

The goal is to prove the core button behavior first.

---

## Recommended Advanced Architecture

An advanced version may look like this:

| Function | Recommended Method |
| --- | --- |
| Button behavior | Interposer MCU |
| Touchscreen UI | Separate touchscreen controller |
| Interposer link | Dedicated UART |
| BlueRetro status/control | Dedicated UART or digital I/O |
| SD2PSX status/control | Dedicated UART or digital I/O |
| Bluetooth audio | Digital I/O or UART if useful |
| Mechacon / EE service access | Muxed UART |
| Extra outputs | I/O expansion board |
| RGB lighting | Dedicated output or lighting controller |
| Video power control | MOSFET power switch controlled by I/O |
| Debugging | Separate service header |

This architecture allows growth without forcing all features into the first board.

---

## Minimum Viable Communication

The minimum useful communication between touchscreen and interposer should include:

- Ping / response
- Request status
- Report button state
- Report lid switch state
- Request output change
- Acknowledge command
- Reject unsafe command
- Report firmware version

This is enough to prove touchscreen and interposer communication.

---

## Minimum Viable I/O

The minimum useful I/O for the first board should include:

- Button input
- Console output
- Lid switch input
- One output for mod control
- One spare output
- UART TX
- UART RX
- Ground
- Power input
- Programming/debug pads

This keeps the first prototype realistic.

---

## Possible Pin Planning

Early pin planning should include:

| Signal | Purpose |
| --- | --- |
| BTN_IN | Reads physical power/reset button |
| BTN_OUT | Controls or passes signal to console |
| LID_IN | Reads lid switch state |
| UART_TX | Sends data to touchscreen/debug |
| UART_RX | Receives commands from touchscreen/debug |
| OUT1 | General mod control output |
| OUT2 | Spare output or LED/chime |
| IN1 | Spare input or service jumper |
| MCU_RST | Programming or reset |
| VCC | Logic power |
| GND | Ground |

The exact pinout depends on the selected microcontroller.

---

## Service Mode Communication

Service mode can expose additional commands.

Possible service commands:

| Command Idea | Meaning |
| --- | --- |
| `TEST OUT1` | Pulse or toggle output 1 |
| `TEST OUT2` | Pulse or toggle output 2 |
| `READ BTN` | Read button input |
| `READ LID` | Read lid input |
| `READ ALL` | Read all known states |
| `PROFILE?` | Show active profile |
| `VERSION?` | Show firmware version |
| `SAFE` | Force safe/default mode |
| `PASS` | Force pass-through mode |

Service commands should be protected if they can affect console behavior.

---

## Configuration Storage

Button Butler may eventually store configuration.

Possible stored settings:

- Active profile
- Default output states
- Button timing values
- Long press behavior
- Touchscreen brightness
- Service lock state
- RGB preference
- Last known modchip state
- Last known ChipSlayer state

Early prototypes may not need persistent settings.

Hardcoded firmware profiles may be easier at first.

---

## Firmware Update Considerations

Firmware update planning should happen early.

Possible update methods:

- Programming pads
- ICSP header
- UART bootloader
- Touchscreen-assisted update
- Separate programmer connection

For early prototypes, simple programming pads may be enough.

Customer-facing firmware updates should only be considered after the hardware is stable.

---

## Documentation Requirements

Every UART and I/O connection should be documented.

Documentation should include:

- Signal name
- Direction
- Voltage level
- Default state
- Active state
- Connected device
- Connector pin
- Notes about pull-ups or pull-downs
- Safety notes

This prevents confusion later when multiple board revisions exist.

---

## Open Questions

Open questions for this architecture:

- Which MCU should be used for the first interposer board?
- How many hardware UARTs are required on the base board?
- Should the touchscreen controller act as the main UART hub?
- Should the I/O expansion board use UART, I2C, or direct GPIO?
- Should outputs be mostly open-drain by default?
- What is the safest default state for ChipSlayer control?
- What is the safest default state for modchip control?
- Should Mechacon and Emotion Engine UARTs be muxed?
- Should BlueRetro and SD2PSX have dedicated UARTs?
- How should backfeeding be prevented between always-on and switched-power boards?
- Should profiles be stored in EEPROM, flash, or set at compile time?

These should be answered through testing and prototype revisions.

---

## Risks

Possible risks:

- UART devices fighting each other
- Tied TX lines causing damage or bad data
- Backfeeding through communication lines
- Wrong voltage levels
- Floating inputs
- Unsafe default output states
- Too many features on the first board
- Touchscreen commands bypassing safety logic
- Long wires causing noise problems
- Different PS2 models behaving differently
- Firmware lockup affecting console power/reset behavior

The safest path is to build the system in stages.

---

## Recommended Development Order

Recommended order:

1. Define basic interposer signals
2. Select first microcontroller
3. Prototype button input and console output
4. Add UART debug output
5. Add lid switch input
6. Add one safe test output
7. Define simple UART status messages
8. Add touchscreen ping/status communication
9. Add command acknowledgement
10. Add output control commands
11. Add I/O expansion only after base behavior is proven
12. Add advanced UART devices later

Do not begin with every possible UART device connected.

Start small and grow.

---

## Summary

The UART and I/O architecture for Button Butler should stay simple, reliable, and modular.

The Button Interposer Board should control the critical PS2 power/reset behavior.

The touchscreen should act as the user interface and request actions from the interposer.

The I/O Expansion Board should provide extra raw control points for lights, mod control, accessory power, and future features.

Standard UART should be treated as point-to-point unless proper muxing or bus hardware is added.

The first prototype should focus on button behavior, lid switch input, UART debug, and one or two safe outputs.

Advanced support for BlueRetro, SD2PSX, Bluetooth audio, Mechacon, Emotion Engine, RGB lighting, video power control, and laser feedback can be added after the core system is proven.
