# QA Checklist

## Overview

This document defines the quality assurance checklist for the **FBD PS2 Button Butler** project.

The purpose of this checklist is to make sure Button Butler boards, firmware, wiring, and installed systems are inspected and tested before being used in a real PlayStation 2 build.

Button Butler interacts with console power/reset behavior, internal mod control, optional touchscreen control, and accessory power features. Because of that, testing must be careful and repeatable.

---

## Purpose

The QA checklist is used to verify that each Button Butler build is:

- Assembled correctly
- Programmed with the correct firmware
- Electrically safe
- Set to safe default states
- Communicating correctly
- Working with the intended PS2 model
- Not causing random resets or startup problems
- Not backfeeding other boards
- Not interfering with normal console behavior
- Ready for bench testing, console testing, or customer use

This checklist should be updated as hardware revisions become real.

---

## QA Status Levels

Use these status labels when testing boards or installs.

| Status | Meaning |
| --- | --- |
| Not Tested | Item has not been checked yet |
| Pass | Item passed inspection or testing |
| Fail | Item failed and needs correction |
| Retest Required | Item was corrected but needs to be tested again |
| Not Applicable | Item does not apply to this build |
| Deferred | Item will be tested later |
| Blocked | Item cannot be tested until another issue is resolved |

Do not treat a board as ready if any safety-critical item is marked Fail, Retest Required, or Blocked.

---

## Build Information

Record this information before testing.

| Item | Value |
| --- | --- |
| Build ID |  |
| Date Tested |  |
| Tester |  |
| Console Model |  |
| Motherboard Revision |  |
| Button Butler Board Revision |  |
| I/O Expansion Board Revision |  |
| Touchscreen Hardware Revision |  |
| Power Management Board Revision |  |
| Interposer Firmware Version |  |
| Touchscreen Firmware Version |  |
| I/O Expansion Firmware Version |  |
| Active Build Profile |  |
| Installed Mods |  |
| Notes |  |

---

## Installed Feature Checklist

Mark which features are installed in this build.

| Feature | Installed | Notes |
| --- | --- | --- |
| Button Interposer Board |  |  |
| I/O Expansion Board |  |  |
| Touchscreen Hub |  |  |
| Power Management Board |  |  |
| ChipSlayer Control |  |  |
| Modchip Control |  |  |
| Bluetooth Audio Control |  |  |
| BlueRetro Control |  |  |
| RGB Lighting Control |  |  |
| RetroGEM Power Control |  |  |
| ElectronAnalog Power Control |  |  |
| SD2PSX Status Support |  |  |
| Internal Router Control |  |  |
| Chime / Buzzer Output |  |  |
| Service Mode |  |  |
| Safe Mode |  |  |

---

## Required Test Tools

Recommended tools for QA testing:

- Digital multimeter
- Bench power supply if available
- USB-to-serial adapter
- Logic analyzer if available
- Current meter or USB power meter where applicable
- Thermal camera or temperature probe if available
- PIC programmer or selected MCU programmer
- Serial terminal software
- Known-good PS2 console
- Known-good controller
- Known-good power supply
- Known-good display or HDMI monitor
- Dummy load for power output testing
- Magnification for solder inspection

Not every test requires every tool, but power and safety checks should not be skipped.

---

## Board Identification

Before powering the board, confirm identification.

| Check | Status | Notes |
| --- | --- | --- |
| Board name is visible |  |  |
| Board revision is visible |  |  |
| PCB matches expected design |  |  |
| Silkscreen labels are readable |  |  |
| Connector labels are readable |  |  |
| Pin 1 markings are clear |  |  |
| Voltage labels are clear |  |  |
| Board is not mixed with another revision |  |  |
| Correct BOM was used |  |  |
| Correct firmware target is known |  |  |

---

## Visual Inspection

Inspect the board before applying power.

| Check | Status | Notes |
| --- | --- | --- |
| No missing components |  |  |
| No incorrect component values seen |  |  |
| No reversed polarized parts |  |  |
| No solder bridges |  |  |
| No cold solder joints |  |  |
| No lifted pads |  |  |
| No cracked components |  |  |
| No damaged traces |  |  |
| No loose wires |  |  |
| No flux residue causing concern |  |  |
| No exposed copper where it could short |  |  |
| No component installed in wrong orientation |  |  |
| MCU orientation is correct |  |  |
| Diode orientation is correct |  |  |
| MOSFET or transistor orientation is correct |  |  |
| Connector orientation is correct |  |  |
| Programming pads are accessible |  |  |
| Test points are accessible |  |  |

---

## Continuity Checks

Perform continuity checks before powering the board.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| VCC to GND short check | No short |  |  |
| 3.3V to GND short check | No short |  |  |
| 5V to GND short check | No short |  |  |
| VIN to GND short check | No short |  |  |
| Output pins to GND | No unexpected short |  |  |
| Output pins to VCC | No unexpected short |  |  |
| UART TX to GND | No short |  |  |
| UART RX to GND | No short |  |  |
| Button input to ground | Matches design |  |  |
| Button output to ground | Matches design |  |  |
| Lid input to ground | Matches design |  |  |
| Programming pins not shorted | No shorts |  |  |
| Adjacent connector pins | No unwanted bridges |  |  |

---

## Power-Up Safety Check

Power the board carefully for the first time.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| Current draw is reasonable | No excessive draw |  |  |
| MCU does not overheat | Cool or mildly warm |  |  |
| Regulator does not overheat | Within safe range |  |  |
| No smoke or smell | None |  |  |
| No visible component heating | None |  |  |
| VCC voltage is correct | Matches design |  |  |
| 3.3V rail is correct | About 3.3V if used |  |  |
| 5V rail is correct | About 5V if used |  |  |
| Output defaults are safe | Inactive or expected |  |  |
| No output pulses at startup | None unless intended |  |  |
| Status LED behavior is expected | Matches firmware |  |  |
| UART boot message appears if enabled | Expected boot text |  |  |

---

## Firmware Programming Check

Verify firmware before functional testing.

| Check | Status | Notes |
| --- | --- | --- |
| Correct firmware file selected |  |  |
| Correct target MCU selected |  |  |
| Correct board revision selected |  |  |
| Firmware flashes successfully |  |  |
| Firmware verifies successfully |  |  |
| Configuration bits are correct |  |  |
| Oscillator setting is correct |  |  |
| Watchdog setting is intentional |  |  |
| Brown-out setting is intentional |  |  |
| Firmware version is recorded |  |  |
| Firmware responds after reset |  |  |
| Firmware matches active build profile |  |  |

---

## Firmware Version Verification

Use UART, touchscreen, or programming notes to confirm firmware identity.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| `VERSION?` command works if supported | Version returned |  |  |
| Firmware version matches build record | Match |  |  |
| Hardware revision matches firmware target | Match |  |  |
| Active profile is correct | Expected profile |  |  |
| Protocol version is recorded | Known version |  |  |
| Feature list matches installed hardware | Match |  |  |

---

## UART Communication Test

Test serial communication before console installation.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| UART ground connected | Shared ground |  |  |
| TX and RX are crossed correctly | TX to RX, RX to TX |  |  |
| Logic voltage is correct | 3.3V or 5V as designed |  |  |
| Serial baud rate is correct | Matches firmware |  |  |
| Boot message appears | `EVT BOOT` or similar |  |  |
| Ready message appears | `EVT READY` or similar |  |  |
| `PING` command works | `OK PONG` |  |  |
| `STATUS` command works | State returned |  |  |
| Unknown command is rejected safely | `UNKNOWN` or `ERR` |  |  |
| Invalid command does not change outputs | Outputs unchanged |  |  |
| Long command does not crash firmware | Safe rejection |  |  |
| UART does not backfeed unpowered board | No unwanted voltage |  |  |

---

## Button Input Test

Test the physical button input or simulated button input.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| Button released state reads correctly | Released / inactive |  |  |
| Button pressed state reads correctly | Pressed / active |  |  |
| Button input is not floating | Stable state |  |  |
| Debounce works | No multiple false presses |  |  |
| Short press detected | Correct event |  |  |
| Long press detected | Correct event |  |  |
| Button release detected | Correct event |  |  |
| Button stuck condition handled if supported | Error or safe behavior |  |  |
| UART reports button events | Expected events |  |  |
| Touchscreen displays button state if installed | Correct state |  |  |

---

## Lid Switch Input Test

Test the lid switch input if installed.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| Lid closed state reads correctly | Closed |  |  |
| Lid open state reads correctly | Open |  |  |
| Lid input is not floating | Stable state |  |  |
| Lid state change is reported over UART | Event reported |  |  |
| Lid state change appears on touchscreen if installed | Correct display |  |  |
| Lid-open alternate behavior works if enabled | Expected action |  |  |
| Lid-closed normal behavior works | Expected action |  |  |
| Lid input does not affect console behavior unexpectedly | No issue |  |  |

---

## Button Output / Pass-Through Test

Test the console-side button output behavior.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| Pass-through mode works | Normal button behavior |  |  |
| Button output default state is safe | Inactive or pass-through |  |  |
| Button output does not glitch on startup | No unwanted pulse |  |  |
| Button output does not glitch on firmware reset | No unwanted pulse |  |  |
| Button block mode works if supported | Button action blocked only when intended |  |  |
| Alternate action mode works if supported | Expected alternate output |  |  |
| Safe mode preserves expected button behavior | Expected safe behavior |  |  |
| Touchscreen command cannot bypass safety logic | Rejected if unsafe |  |  |

---

## Output Default State Test

Before connecting outputs to real mods, test their default states.

| Output | Expected Default | Measured State | Status | Notes |
| --- | --- | --- | --- | --- |
| OUT1 | Inactive |  |  |  |
| OUT2 | Inactive |  |  |  |
| OUT3 | Inactive |  |  |  |
| OUT4 | Inactive |  |  |  |
| ChipSlayer output | Profile-defined |  |  |  |
| Modchip output | Safe known state |  |  |  |
| Bluetooth pairing output | Inactive |  |  |  |
| BlueRetro reset output | Inactive |  |  |  |
| RGB enable output | Off |  |  |  |
| Video power enable | Profile-defined |  |  |  |
| Buzzer output | Off |  |  |  |

No output should float into an active state.

---

## Output Function Test

Test outputs with LEDs, dummy loads, or safe test circuits before connecting to real mods.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| OUT1 turns on when commanded | Correct state |  |  |
| OUT1 turns off when commanded | Correct state |  |  |
| OUT1 toggles when commanded | Correct state |  |  |
| OUT1 readback works if supported | Correct state |  |  |
| OUT2 turns on when commanded | Correct state |  |  |
| OUT2 turns off when commanded | Correct state |  |  |
| Pulse output uses correct timing | Correct pulse |  |  |
| Pulse output has safe maximum | Long pulses rejected |  |  |
| Locked output rejects command | `LOCKED` or `UNSAFE` |  |  |
| Outputs return to safe state after reset | Safe state |  |  |
| Outputs return to safe state after power cycle | Safe state |  |  |

---

## Backfeeding Test

Backfeeding must be checked before connecting multiple boards.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| UART does not power unpowered board | No unwanted voltage |  |  |
| Output line does not power target board when off | No unwanted voltage |  |  |
| Pull-up resistors do not backfeed disabled rails | No unwanted voltage |  |  |
| Touchscreen does not backfeed interposer | No unwanted voltage |  |  |
| Interposer does not backfeed touchscreen | No unwanted voltage |  |  |
| I/O board does not backfeed accessories | No unwanted voltage |  |  |
| Power management board does not backfeed source rail | No unwanted voltage |  |  |
| USB programmer does not backfeed console rail | No unwanted voltage |  |  |
| Disabled video board rail stays off | No unwanted voltage |  |  |
| Disabled Bluetooth audio rail stays off | No unwanted voltage |  |  |

Backfeeding failures must be fixed before final installation.

---

## Safe Mode Test

Test safe mode if supported.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| Safe mode can be entered | Confirmed |  |  |
| Safe mode can be exited if allowed | Confirmed |  |  |
| Safe mode disables noncritical outputs | Expected outputs off |  |  |
| Safe mode blocks risky commands | Commands rejected |  |  |
| Safe mode preserves normal button behavior if designed | Expected behavior |  |  |
| Safe mode reports over UART | State reported |  |  |
| Touchscreen shows safe mode if installed | Warning shown |  |  |
| Safe mode survives reset only if intended | Expected behavior |  |  |

---

## Service Mode Test

Test service mode if supported.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| Service mode entry method works | Enters service mode |  |  |
| Service mode exit method works | Exits service mode |  |  |
| Service mode is not entered accidentally | Protected |  |  |
| Inputs can be read in service mode | Correct states |  |  |
| Outputs can be tested in service mode | Correct behavior |  |  |
| Risky actions are still protected if required | Locked or confirmed |  |  |
| Touchscreen service page is protected | Not easy to enter accidentally |  |  |
| Service mode state is reported | UART/display report |  |  |
| Service mode does not break normal console use | Normal after exit |  |  |

---

## Touchscreen Hardware Test

If a touchscreen is installed, test it before final assembly.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| Display powers on | Screen active |  |  |
| Backlight works | Visible display |  |  |
| Touch input works | Touch detected |  |  |
| Touch coordinates are correct | Accurate touches |  |  |
| Display orientation is correct | Correct orientation |  |  |
| UI is readable | Text/icons clear |  |  |
| Touch targets are large enough | Easy to press |  |  |
| No false touches at startup | None |  |  |
| No accidental commands during boot | None |  |  |
| Sleep or dim mode works if supported | Expected behavior |  |  |
| Wake behavior works if supported | Expected behavior |  |  |
| Touchscreen current draw is acceptable | Within expected range |  |  |
| Touchscreen does not overheat | Acceptable temperature |  |  |

---

## Touchscreen Communication Test

Verify touchscreen communication with the interposer.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| Touchscreen sends `PING` | Interposer responds |  |  |
| Touchscreen receives status | Status shown |  |  |
| Touchscreen shows interposer connected | Connected state |  |  |
| Touchscreen shows disconnected state if interposer is absent | Correct warning |  |  |
| Touchscreen handles rejected commands | Error shown |  |  |
| Touchscreen does not assume command success | Waits for confirmation |  |  |
| Touchscreen updates output state after command | Correct state |  |  |
| Touchscreen shows firmware versions | Correct versions |  |  |
| Touchscreen hides unsupported features if implemented | Correct pages |  |  |
| Touchscreen does not send risky commands at startup | No unsafe commands |  |  |

---

## I/O Expansion Board Test

If an I/O Expansion Board is installed, test it separately first.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| Board powers correctly | Correct voltage |  |  |
| Current draw is normal | No excessive draw |  |  |
| Communication works | UART/I2C OK |  |  |
| Firmware version is correct if MCU-based | Correct version |  |  |
| Outputs default safe | Inactive or expected |  |  |
| Inputs read correctly | Correct states |  |  |
| Each output toggles correctly | Correct state |  |  |
| Pulse outputs work correctly | Correct timing |  |  |
| Service mode works if supported | Correct behavior |  |  |
| No backfeeding through I/O lines | No unwanted voltage |  |  |
| No output glitches at startup | None |  |  |

---

## Power Management Board Test

If a Power Management Board is installed, test with a dummy load first.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| Input voltage is correct | Correct voltage |  |  |
| Output voltage is correct when enabled | Correct voltage |  |  |
| Output is off when disabled | No output voltage |  |  |
| Enable signal works | Output follows enable |  |  |
| Default output state is safe | Expected default |  |  |
| Dummy load test passes | Stable output |  |  |
| Current draw is measured | Recorded |  |  |
| Load switch or MOSFET does not overheat | Acceptable temp |  |  |
| Power-good works if installed | Correct state |  |  |
| No inrush issue observed | Stable startup |  |  |
| No backfeeding when disabled | No unwanted voltage |  |  |
| Repeated power cycling passes | Stable behavior |  |  |

---

## RetroGEM / ElectronAnalog Power Control Test

Only use this section if video power control is installed.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| Video board works before power control | Normal video |  |  |
| Power control circuit forced on works | Normal video |  |  |
| Manual video power on works | Video board powers |  |  |
| Manual video power off works only when intended | Controlled off |  |  |
| Video power default is safe | Usually on with console |  |  |
| Video off command requires confirmation if customer-facing | Protected |  |  |
| HDMI output appears normally after power on | Stable video |  |  |
| No HDMI handshake issue after power cycle | Stable video |  |  |
| No backfeeding when video board is off | No unwanted voltage |  |  |
| Power-good reports correctly if installed | Correct state |  |  |
| Video board temperature is acceptable | Recorded |  |  |
| User can restore video power easily | Confirmed |  |  |

Video power control should not be considered customer-ready until it passes repeated testing.

---

## ChipSlayer Control Test

Only use this section if ChipSlayer control is installed.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| ChipSlayer output default is correct | Profile-defined |  |  |
| ChipSlayer enable command works | Correct state |  |  |
| ChipSlayer disable command works | Correct state |  |  |
| ChipSlayer toggle command works | Correct state |  |  |
| Lid-open shortcut works if enabled | Correct action |  |  |
| Touchscreen control works if installed | Correct action |  |  |
| State feedback works if installed | Correct state |  |  |
| Output does not glitch at startup | No unwanted change |  |  |
| Console boots correctly in each state | Expected boot behavior |  |  |
| Customer-facing behavior is documented | Documentation complete |  |  |

---

## Modchip Control Test

Only use this section if modchip control is installed.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| Modchip default state is correct | Safe known state |  |  |
| Modchip enable works if supported | Correct behavior |  |  |
| Modchip disable works if supported | Correct behavior |  |  |
| Modchip toggle is protected | Confirmation or service required |  |  |
| Modchip state does not change randomly | Stable |  |  |
| Modchip state is set before boot if required | Correct timing |  |  |
| Console boots correctly with modchip enabled | Expected behavior |  |  |
| Console boots correctly with modchip disabled | Expected behavior |  |  |
| Touchscreen warning appears if installed | Clear warning |  |  |
| Customer-facing documentation is complete | Documentation complete |  |  |

Modchip control is safety-sensitive because it affects boot behavior.

---

## Bluetooth Audio Control Test

Only use this section if Bluetooth audio control is installed.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| Bluetooth audio power default is correct | Profile-defined |  |  |
| Audio enable works | Board powers/enables |  |  |
| Audio disable works | Board disables |  |  |
| Pairing trigger works | Enters pairing mode |  |  |
| Pairing trigger does not activate at startup | No accidental pairing |  |  |
| Reset pulse works if installed | Board resets |  |  |
| Touchscreen audio page works if installed | Correct controls |  |  |
| No loud pops during switching | Acceptable audio behavior |  |  |
| No added audio noise | Acceptable audio |  |  |
| No backfeeding through audio/control lines | No unwanted voltage |  |  |

---

## BlueRetro Control Test

Only use this section if BlueRetro control is installed.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| BlueRetro powers correctly | Stable power |  |  |
| BlueRetro default state is correct | Profile-defined |  |  |
| BlueRetro enable works if supported | Correct behavior |  |  |
| BlueRetro disable works if supported | Correct behavior |  |  |
| BlueRetro reset works if supported | Controller recovers |  |  |
| Channel controls work if supported | Correct channels |  |  |
| Wired controller still works when expected | Correct behavior |  |  |
| Wireless controller works when expected | Correct behavior |  |  |
| No random button inputs | Stable input |  |  |
| Touchscreen page works if installed | Correct controls |  |  |
| No backfeeding through controller lines | No unwanted voltage |  |  |

---

## SD2PSX Support Test

Only use this section if SD2PSX support is installed.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| SD2PSX powers normally | Stable behavior |  |  |
| SD2PSX is not power-cycled unexpectedly | No interruption |  |  |
| Status display works if supported | Correct status |  |  |
| Game ID display works if supported | Correct value |  |  |
| Touchscreen storage page works if installed | Correct display |  |  |
| No save interruption occurs | Saves stable |  |  |
| No memory card behavior issue observed | Stable behavior |  |  |
| No risky control commands are exposed | Safe UI |  |  |
| No backfeeding through status/control lines | No unwanted voltage |  |  |

SD2PSX should be treated conservatively.

---

## RGB Lighting Test

Only use this section if RGB or lighting control is installed.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| Lighting default state is correct | Usually off |  |  |
| Lighting turns on when commanded | Correct behavior |  |  |
| Lighting turns off when commanded | Correct behavior |  |  |
| Brightness control works if supported | Correct behavior |  |  |
| Mode control works if supported | Correct behavior |  |  |
| Touchscreen lighting page works if installed | Correct controls |  |  |
| Current draw is measured | Recorded |  |  |
| LEDs do not overheat | Acceptable temp |  |  |
| Lighting does not add video or audio noise | No interference |  |  |
| Wiring is not pinched or exposed | Safe routing |  |  |

---

## Chime / Buzzer Test

Only use this section if a chime or buzzer is installed.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| Buzzer default state is off | Silent |  |  |
| Startup chime works if enabled | Correct sound |  |  |
| Error tone works if enabled | Correct sound |  |  |
| Button feedback tone works if enabled | Correct sound |  |  |
| Bluetooth pairing tone works if enabled | Correct sound |  |  |
| Volume is acceptable | Not too loud |  |  |
| No unwanted sound at startup | Silent unless intended |  |  |
| No unwanted sound during reset | Silent unless intended |  |  |
| Chime can be disabled if supported | Disabled |  |  |

---

## Internal Router Control Test

Only use this section if internal router control is installed.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| Router powers correctly | Stable power |  |  |
| Router current draw is measured | Recorded |  |  |
| Router enable works | Correct state |  |  |
| Router reset works if installed | Restarts router |  |  |
| Router ready input works if installed | Correct ready state |  |  |
| Router boot time is recorded | Recorded |  |  |
| Storage mount time is recorded if relevant | Recorded |  |  |
| Touchscreen router page works if installed | Correct controls |  |  |
| Console does not boot before router if sequencing is required | Correct timing |  |  |
| Router heat is acceptable | Recorded |  |  |

---

## Console Installation Pre-Check

Before installing Button Butler into a console, confirm the console is known-good.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| Console powers on before install | Pass |  |  |
| Console resets normally before install | Pass |  |  |
| Console video works before install | Pass |  |  |
| Controller port works before install | Pass |  |  |
| Memory card behavior works before install | Pass |  |  |
| Optical drive behavior works if installed | Pass |  |  |
| Existing mods work before install | Pass |  |  |
| Power supply is known-good | Pass |  |  |
| No known board damage | Pass |  |  |

Do not troubleshoot Button Butler on a console that was not verified before installation.

---

## Console Installation Inspection

Inspect the physical install before powering the console.

| Check | Status | Notes |
| --- | --- | --- |
| Board is mounted securely |  |  |
| Board is insulated from RF shield |  |  |
| No exposed pads touch metal |  |  |
| Wires are not pinched |  |  |
| Wires avoid screw posts |  |  |
| Wires avoid hot components |  |  |
| Wires avoid moving parts |  |  |
| Connectors are seated properly |  |  |
| Strain relief is installed where needed |  |  |
| Shell can close without pressure |  |  |
| Touchscreen mounting is secure if installed |  |  |
| Touchscreen bezel or cutout is clean if installed |  |  |
| Service access is available if planned |  |  |
| Debug pads are not shorted |  |  |

---

## Console Power-On Test

Power the console carefully after installation.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| Standby behavior is normal | Normal standby |  |  |
| Console powers on normally | Powers on |  |  |
| No random reset occurs | Stable |  |  |
| No abnormal current draw | Normal |  |  |
| No overheating during startup | Normal temp |  |  |
| Video output appears | Stable video |  |  |
| Button Butler reports ready | UART/display ready |  |  |
| Touchscreen starts if installed | Screen ready |  |  |
| Outputs remain in expected states | Safe states |  |  |
| Existing mods still work | Expected behavior |  |  |

---

## Console Button Behavior Test

Test normal and alternate button behavior in the installed console.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| Short press with lid closed | Normal behavior |  |  |
| Long press with lid closed | Expected behavior |  |  |
| Short press with lid open | Expected alternate or normal behavior |  |  |
| Long press with lid open | Expected alternate behavior |  |  |
| Service mode entry does not happen accidentally | Protected |  |  |
| Button pass-through still works | Normal behavior |  |  |
| Button blocking only happens when intended | Correct behavior |  |  |
| No random reset during gameplay | Stable |  |  |
| No power-on issue after repeated tests | Stable |  |  |

---

## Gameplay / Runtime Test

Run the console long enough to catch unstable behavior.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| Console boots to menu or loader | Stable |  |  |
| Game launches | Stable |  |  |
| Controller input works | Stable |  |  |
| Memory card saves work | Stable |  |  |
| Video remains stable | No dropouts |  |  |
| Audio remains stable | No unwanted noise |  |  |
| No random reset | Stable |  |  |
| No touchscreen false commands | None |  |  |
| No mod state changes unexpectedly | Stable |  |  |
| No excessive heat after runtime | Acceptable |  |  |

Suggested minimum runtime should be updated after real testing.

---

## Heat Test

Record heat behavior after the console has been running.

| Location | Temperature | Status | Notes |
| --- | --- | --- | --- |
| Button Interposer Board |  |  |  |
| I/O Expansion Board |  |  |  |
| Touchscreen Module |  |  |  |
| Power Management Board |  |  |  |
| Video Board |  |  |  |
| Bluetooth Audio Board |  |  |  |
| BlueRetro Board |  |  |  |
| Internal Router |  |  |  |
| PS2 Regulator Area |  |  |  |
| Shell Surface Near Touchscreen |  |  |  |
| Shell Surface Near Power Boards |  |  |  |

Heat should be tested with the shell closed.

---

## Final Assembly Check

Before final closure or delivery, perform a final inspection.

| Check | Status | Notes |
| --- | --- | --- |
| All boards secured |  |  |
| All connectors seated |  |  |
| All wires dressed cleanly |  |  |
| No wires pinched |  |  |
| No exposed conductors |  |  |
| RF shield installed correctly if used |  |  |
| Shell closes cleanly |  |  |
| Screws installed correctly |  |  |
| Touchscreen aligned if installed |  |  |
| Buttons operate mechanically |  |  |
| Vents are not blocked |  |  |
| Fan path is not blocked |  |  |
| Customer-facing controls are clean |  |  |
| Service notes are recorded |  |  |

---

## Final Functional Check

After final assembly, repeat key tests.

| Check | Expected Result | Status | Notes |
| --- | --- | --- | --- |
| Console powers on | Pass |  |  |
| Console resets or behaves normally | Pass |  |  |
| Video output works | Pass |  |  |
| Controller works | Pass |  |  |
| Memory card / SD2PSX works if installed | Pass |  |  |
| Touchscreen works if installed | Pass |  |  |
| ChipSlayer control works if installed | Pass |  |  |
| Bluetooth audio works if installed | Pass |  |  |
| BlueRetro works if installed | Pass |  |  |
| RGB lighting works if installed | Pass |  |  |
| Video power control works if installed | Pass |  |  |
| No random reset during short runtime | Pass |  |  |
| No abnormal heat | Pass |  |  |

---

## Customer Documentation Check

Before selling or delivering a build, confirm documentation exists.

| Check | Status | Notes |
| --- | --- | --- |
| Build ID recorded |  |  |
| Installed features listed |  |  |
| Button behavior documented |  |  |
| Touchscreen functions documented if installed |  |  |
| Hidden shortcuts documented if customer-facing |  |  |
| Service-only features not exposed unnecessarily |  |  |
| Known limitations documented |  |  |
| Firmware versions recorded |  |  |
| Board revisions recorded |  |  |
| Any special startup behavior documented |  |  |
| Any power management behavior documented |  |  |
| Customer warning notes included if needed |  |  |

---

## Failure Log

Use this table to record issues found during QA.

| Issue Number | Date | Area | Problem | Action Taken | Retest Status |
| --- | --- | --- | --- | --- | --- |
| 1 |  |  |  |  |  |
| 2 |  |  |  |  |  |
| 3 |  |  |  |  |  |

---

## Pass / Fail Summary

Final QA decision.

| Item | Result |
| --- | --- |
| Visual Inspection |  |
| Continuity Checks |  |
| Power Checks |  |
| Firmware Verification |  |
| UART Testing |  |
| Button Behavior Testing |  |
| Output Testing |  |
| Backfeeding Testing |  |
| Touchscreen Testing |  |
| I/O Expansion Testing |  |
| Power Management Testing |  |
| Console Runtime Testing |  |
| Heat Testing |  |
| Final Assembly Check |  |
| Customer Documentation Check |  |

---

## Final QA Decision

| Decision | Mark |
| --- | --- |
| Approved for bench testing only |  |
| Approved for console testing |  |
| Approved for personal prototype use |  |
| Approved for customer build use |  |
| Failed QA |  |
| Retest required |  |

Final notes:

| Notes |
| --- |
|  |

Tester signature or initials:

| Tester | Date |
| --- | --- |
|  |  |

---

## Summary

This QA checklist is intended to keep Button Butler testing organized and repeatable.

The most important checks are:

- No shorts
- Correct firmware
- Safe default states
- Normal console button behavior
- No output glitches
- No backfeeding
- No random resets
- Clean installation
- Clear documentation

Button Butler should not be treated as customer-ready until the hardware, firmware, installation, and safety behavior have all been tested and documented.
