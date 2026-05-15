# Test Data

## Overview

This folder contains test data, measurement records, logs, validation notes, and failure reports for the **FBD PS2 Button Butler** project.

Button Butler is a work-in-progress PlayStation 2 mod control platform. Because it may interact with power/reset behavior, mod control, touchscreen communication, accessory power, and future diagnostics, testing must be organized and repeatable.

This folder is where real test results should be saved.

---

## Purpose Of This Folder

The `Test-Data/` folder is used to collect proof, measurements, and observations from Button Butler development.

This may include:

- UART logs
- Button timing tests
- Lid switch tests
- Power measurements
- Heat testing
- Console testing notes
- Touchscreen test results
- I/O expansion test results
- Power management test results
- Video power control tests
- Backfeeding measurements
- Failure reports
- Fix verification notes

The goal is to separate tested behavior from ideas.

---

## Current Status

| Area | Status |
| --- | --- |
| UART logs | Planned |
| Button tests | Planned |
| Power measurements | Planned |
| Heat testing | Planned |
| Console tests | Planned |
| Touchscreen tests | Planned |
| I/O expansion tests | Planned |
| Failure reports | Planned |
| Final validation data | Not started |

This folder should be updated as real hardware prototypes are built and tested.

---

## Suggested Folder Structure

```text
Test-Data/
├── README.md
├── UART-Logs/
│   └── README.md
├── Button-Tests/
│   └── README.md
├── Lid-Switch-Tests/
│   └── README.md
├── Power-Measurements/
│   └── README.md
├── Heat-Testing/
│   └── README.md
├── Console-Tests/
│   └── README.md
├── Touchscreen-Tests/
│   └── README.md
├── IO-Expansion-Tests/
│   └── README.md
├── Power-Management-Tests/
│   └── README.md
├── Backfeeding-Tests/
│   └── README.md
├── Scope-Captures/
│   └── README.md
├── Logic-Analyzer-Captures/
│   └── README.md
└── Failure-Reports/
    └── README.md
```

This structure can be adjusted as the project develops.

---

## Test Data Philosophy

Button Butler should be developed from measured results, not guesses.

Test data should help answer:

- Does the board power safely?
- Does the firmware boot correctly?
- Does the button behave correctly?
- Does the lid switch read correctly?
- Do outputs default to safe states?
- Does UART communication work?
- Does the touchscreen receive confirmed states?
- Are there backfeeding issues?
- Does accessory power switching behave safely?
- Does the console still work normally?
- Does the system remain stable during gameplay?
- Does anything overheat?

Good test data protects the project from bad assumptions.

---

## Test Status Labels

Use these labels when documenting test results.

| Status | Meaning |
| --- | --- |
| Not Tested | Test has not been performed |
| Pass | Test passed |
| Fail | Test failed |
| Partial Pass | Some parts passed, but more work is needed |
| Retest Required | A fix was made and needs verification |
| Blocked | Test cannot continue until another issue is resolved |
| Not Applicable | Test does not apply to this hardware or build |
| Inconclusive | Results were unclear or not repeatable |

Do not treat inconclusive tests as proof.

---

## Test Confidence Labels

Use confidence labels when needed.

| Confidence | Meaning |
| --- | --- |
| Low | One quick test or incomplete setup |
| Medium | Repeated test or mostly controlled setup |
| High | Repeated testing with stable results |
| Proven | Repeated bench and console testing with documentation |

A feature should not be treated as customer-ready from a single test.

---

## Required Test Information

Each test file should include basic information.

| Item | Description |
| --- | --- |
| Test name | What was tested |
| Date | When the test was performed |
| Tester | Who performed the test |
| Board name | Which board was tested |
| Board revision | Hardware revision |
| Firmware version | Firmware used during the test |
| Console model | PS2 model if used |
| Motherboard revision | PS2 motherboard revision if known |
| Installed mods | Relevant installed mods |
| Test setup | Tools, wiring, and conditions |
| Results | What happened |
| Status | Pass, Fail, Partial Pass, etc. |
| Notes | Any extra observations |
| Next action | What should happen next |

This makes test results useful later.

---

## Suggested Test File Template

Use this format for new test records:

```markdown
# Test Name

## Test Information

| Item | Value |
| --- | --- |
| Date |  |
| Tester |  |
| Board |  |
| Board Revision |  |
| Firmware Version |  |
| Console Model |  |
| Motherboard Revision |  |
| Installed Mods |  |
| Test Status |  |
| Confidence |  |

---

## Purpose

Describe what this test is trying to prove.

---

## Test Setup

Describe the setup, wiring, power source, tools, and connected boards.

---

## Test Steps

1. 
2. 
3. 

---

## Expected Result

Describe what should happen.

---

## Actual Result

Describe what actually happened.

---

## Measurements

| Measurement | Value | Notes |
| --- | --- | --- |
|  |  |  |

---

## Result

Pass, fail, partial pass, or inconclusive.

---

## Notes

Add observations, concerns, or unexpected behavior.

---

## Next Action

Describe what should be tested or fixed next.
```

---

## UART Logs

The `UART-Logs/` folder should contain serial logs from Button Butler firmware.

Possible UART logs:

- First boot logs
- Firmware ready messages
- Button event logs
- Lid switch event logs
- Output control logs
- Touchscreen command logs
- Service mode logs
- Safe mode logs
- Error response logs
- Unknown command tests
- Version command results

UART logs are important because they show what the firmware actually reported.

---

## UART Log Naming

Suggested UART log naming:

```text
YYYY-MM-DD-board-firmware-test-description.txt
```

Examples:

```text
2026-05-15-interposer-v0.1.0-first-boot.txt
2026-05-15-interposer-v0.1.0-button-events.txt
2026-05-15-touchscreen-v0.2.0-ping-test.txt
2026-05-15-io-expansion-v0.1.0-output-test.txt
```

File names should include the date, board, firmware version, and test purpose.

---

## UART Log Notes

Each UART log should include:

- Board revision
- Firmware version
- Baud rate
- Serial terminal used
- Test commands sent
- Full responses received
- Any unexpected messages
- Whether the test passed

Do not paste logs without context.

---

## Button Tests

The `Button-Tests/` folder should contain power/reset button behavior tests.

Possible button tests:

- Button released state
- Button pressed state
- Debounce behavior
- Short press detection
- Long press detection
- Double press detection if supported
- Startup button state
- Pass-through behavior
- Blocked button behavior
- Lid-open alternate behavior
- Service mode entry behavior

Button tests are safety-critical.

---

## Button Test Measurements

Useful button measurements:

| Measurement | Purpose |
| --- | --- |
| Button released voltage | Confirms idle state |
| Button pressed voltage | Confirms active state |
| Debounce time | Confirms firmware filtering |
| Short press timing | Confirms expected action |
| Long press timing | Confirms protected action |
| Startup state | Confirms no false press |
| Pass-through voltage | Confirms console receives proper signal |
| Blocked state voltage | Confirms console does not receive unwanted signal |

Document the console model for every button test.

---

## Lid Switch Tests

The `Lid-Switch-Tests/` folder should contain lid switch behavior tests.

Possible lid switch tests:

- Lid closed voltage
- Lid open voltage
- Active-high or active-low behavior
- Pull-up or pull-down behavior
- Stability of input
- Firmware reporting
- Touchscreen reporting
- Lid-open button shortcut behavior
- Model-specific lid switch differences

The lid switch may behave differently between PS2 models.

---

## Power Measurements

The `Power-Measurements/` folder should contain voltage and current measurements.

Possible power measurements:

- Button Butler standby current
- Button Butler active current
- Touchscreen current
- I/O Expansion Board current
- Power Management Board current
- BlueRetro current
- Bluetooth audio current
- RGB lighting current
- RetroGEM current
- ElectronAnalog current
- Internal router current
- Accessory rail voltage
- Startup inrush current if measured

Power measurements help with heat and reliability planning.

---

## Power Measurement Table Format

Use this format for power measurements:

| Device / Rail | State | Voltage | Current | Power | Notes |
| --- | --- | --- | --- | --- | --- |
| Button Interposer | Idle |  |  |  |  |
| Touchscreen | Full brightness |  |  |  |  |
| RGB Lighting | On |  |  |  |  |
| ElectronAnalog | Active |  |  |  |  |
| Internal Router | Booting |  |  |  |  |

Use real measured values when available.

---

## Heat Testing

The `Heat-Testing/` folder should contain temperature and runtime test data.

Possible heat tests:

- Console idle temperature
- Console gameplay temperature
- Touchscreen full brightness temperature
- Touchscreen sleep temperature
- Video board temperature
- Power Management Board temperature
- Internal router temperature
- RGB lighting temperature
- Shell surface temperature
- Long-duration closed-shell test

Heat testing should be done with the console fully assembled whenever possible.

---

## Heat Test Table Format

Use this format for heat testing:

| Location | Start Temp | 15 Min | 30 Min | 60 Min | Notes |
| --- | --- | --- | --- | --- | --- |
| Button Interposer |  |  |  |  |  |
| Touchscreen |  |  |  |  |  |
| Video Board |  |  |  |  |  |
| Internal Router |  |  |  |  |  |
| Shell Surface |  |  |  |  |  |

Record whether the shell was open or closed.

---

## Console Tests

The `Console-Tests/` folder should contain real PS2 console testing results.

Possible console tests:

- Console powers on normally
- Console resets normally
- Button pass-through works
- Lid-open alternate behavior works
- Touchscreen communication works inside console
- BlueRetro still works
- Bluetooth audio still works
- SD2PSX still works
- Video output remains stable
- No random resets
- No random controller input
- No memory card issues
- Shell closes cleanly

Console testing is required before any customer use.

---

## Console Test Information

Each console test should include:

| Item | Value |
| --- | --- |
| Console Model |  |
| Motherboard Revision |  |
| Power Supply Used |  |
| Video Output Used |  |
| Controller Used |  |
| Memory Card / SD2PSX Used |  |
| Installed Mods |  |
| Runtime Length |  |
| Test Result |  |

Console tests should not be vague.

---

## Touchscreen Tests

The `Touchscreen-Tests/` folder should contain touchscreen hardware and firmware test results.

Possible touchscreen tests:

- Display startup
- Backlight control
- Touch accuracy
- Touch debounce
- UI page navigation
- UART command test
- Interposer connection test
- Command confirmation behavior
- Error message display
- Sleep and wake behavior
- Current draw
- Heat
- Fitment after mounting

Touchscreen tests should confirm that the UI does not send accidental commands.

---

## I/O Expansion Tests

The `IO-Expansion-Tests/` folder should contain I/O Expansion Board test data.

Possible tests:

- Board power-up
- Output default states
- Output toggle commands
- Pulse output timing
- Input readback
- Communication test
- Service mode test
- Safe mode test
- Backfeeding check
- Load test with dummy loads
- Real mod-control test

Each output should be tested before connecting it to a real mod.

---

## Power Management Tests

The `Power-Management-Tests/` folder should contain accessory power-control test results.

Possible tests:

- Enable input behavior
- Load switch behavior
- MOSFET switching behavior
- Regulator enable behavior
- Dummy load test
- Real accessory test
- Power-good signal test
- Inrush current test
- Repeated power-cycle test
- Heat test
- Backfeeding test

Power tests should begin with dummy loads.

---

## Backfeeding Tests

The `Backfeeding-Tests/` folder should contain measurements related to unwanted power paths.

Possible backfeeding tests:

- UART backfeeding
- I/O output backfeeding
- Touchscreen backfeeding
- USB programmer backfeeding
- Disabled accessory rail backfeeding
- Video board disabled rail voltage
- Bluetooth audio disabled rail voltage
- I2C pull-up backfeeding
- Power-good line backfeeding
- Enable line backfeeding

Backfeeding tests are important before multiple boards are connected.

---

## Backfeeding Test Table Format

Use this format:

| Test Point | Powered Device | Unpowered Device | Measured Voltage | Expected | Result |
| --- | --- | --- | --- | --- | --- |
| UART TX |  |  |  | 0V or safe |  |
| OUT1 |  |  |  | 0V or safe |  |
| VIDEO_OUT |  |  |  | 0V |  |
| TOUCHSCREEN_RX |  |  |  | 0V or safe |  |

Any unexpected voltage should be investigated.

---

## Scope Captures

The `Scope-Captures/` folder should contain oscilloscope screenshots or waveform notes.

Possible scope captures:

- Button press waveform
- Button bounce waveform
- Lid switch waveform
- Output pulse waveform
- UART signal waveform
- Power rail startup
- Power rail shutdown
- Load switch enable timing
- Inrush behavior
- Reset line behavior
- Backfeeding behavior

Scope captures should be labeled clearly.

---

## Logic Analyzer Captures

The `Logic-Analyzer-Captures/` folder should contain serial and digital timing captures.

Possible captures:

- UART command session
- UART errors
- Button timing
- Lid switch timing
- Output pulse timing
- Touchscreen command flow
- I/O expansion command flow
- Service mode session

Each capture should include sample rate and signal labels.

---

## Failure Reports

The `Failure-Reports/` folder should contain records of problems, bugs, damage, and unexpected behavior.

Possible failure reports:

- Console would not power on
- Console stuck in reset
- Button output glitch
- Output active at startup
- UART communication failure
- Touchscreen false touch
- Backfeeding issue
- Regulator overheating
- Load switch failure
- Video board did not start
- HDMI handshake problem
- Bluetooth audio noise
- BlueRetro random inputs
- SD2PSX behavior issue
- Shell fitment problem

Failure reports are valuable.

Do not hide failures.

---

## Failure Report Template

Use this format:

```markdown
# Failure Report - Short Description

## Information

| Item | Value |
| --- | --- |
| Date |  |
| Tester |  |
| Board |  |
| Board Revision |  |
| Firmware Version |  |
| Console Model |  |
| Installed Mods |  |
| Failure Status | Open / Fixed / Retest Required |

---

## Problem

Describe what failed.

---

## Expected Behavior

Describe what should have happened.

---

## Actual Behavior

Describe what actually happened.

---

## Steps To Reproduce

1. 
2. 
3. 

---

## Measurements / Logs

Add measurements, UART logs, photos, or scope captures.

---

## Suspected Cause

Describe the likely cause if known.

---

## Fix Attempted

Describe the correction or change.

---

## Retest Result

Describe whether the fix worked.

---

## Notes

Add extra observations.
```

---

## Test Naming Rules

Use clear names for test files.

Recommended format:

```text
YYYY-MM-DD-board-revision-firmware-test-name.md
```

Examples:

```text
2026-05-15-interposer-v0.1-fw-v0.1.0-button-test.md
2026-05-15-touchscreen-v0.1-fw-v0.2.0-uart-ping.md
2026-05-15-power-v0.1-electronanalog-load-test.md
```

Good file names make test history easier to follow.

---

## Photo And Image Naming Rules

Use clear names for images.

Recommended format:

```text
YYYY-MM-DD-subject-view-description.png
```

Examples:

```text
2026-05-15-interposer-top-assembled.png
2026-05-15-button-line-scope-press.png
2026-05-15-touchscreen-fitment-top-shell.png
2026-05-15-power-board-thermal-test.png
```

Avoid generic names like `image1.png`.

---

## What To Record During Testing

Record more than just pass or fail.

Useful things to record:

- Exact voltage measurements
- Exact current measurements
- Temperature readings
- Firmware version
- Board revision
- Console model
- Test duration
- Commands sent
- Responses received
- Unexpected behavior
- Photos of wiring
- Photos of test setup
- Any changes made during test

Future troubleshooting depends on details.

---

## Safety-Critical Tests

Some tests are safety-critical.

Safety-critical areas:

- Power/reset button behavior
- Console-side button output
- Modchip control
- ChipSlayer control
- Accessory power switching
- Video board power control
- SD2PSX or memory card behavior
- Backfeeding between boards
- Startup output states
- Firmware reset behavior

Safety-critical tests should be Accessory power switching
- Video board power control
- SD2PSX or memory card behavior
- Backfeeding between boards
- Startup output states
- Firmware reset behavior

Safety-critical tests should be repeated before customer use.

---

## Tests Required Before Console Installation

Before installing Button Butler into a real PS2, complete:

- Visual inspection
- Continuity check
- Power-up test
- Firmware version check
- UART communication test
- Output default state test
- Button input test
- Lid input test if used
- Backfeeding check
- Dummy load test for power outputs
- Safe mode test if supported
- Service mode test if supported

Do not skip bench testing.

---

## Tests Required Before Customer Use

Before using Button Butler in a customer build, complete:

- Bench testing
- Console testing
- Runtime testing
- Heat testing if relevant
- Backfeeding testing
- Button behavior testing
- Touchscreen testing if installed
- Feature-specific testing
- Final QA checklist
- Customer documentation review

Customer-ready means tested, not just assembled.

---

## QA Link

The main QA checklist is located at:

```text
Manufacturing/QA-Checklist.md
```

Test data in this folder should support the QA checklist.

---

## Test Data Versus Documentation

Use the correct folder for each type of information.

| Folder | Use |
| --- | --- |
| `Test-Data/` | Actual test results, measurements, logs, failures |
| `Documents/` | Planning and design explanation |
| `Hardware/` | Schematics, PCB files, board notes |
| `Firmware/` | Source code and firmware notes |
| `Manufacturing/` | QA, assembly, packaging, release notes |
| `References/` | Datasheets, pinouts, external sources |

Do not mix test results with design ideas unless clearly labeled.

---

## Raw Versus Processed Data

Some data should be kept raw.

| Data Type | Recommended Handling |
| --- | --- |
| UART logs | Keep raw logs and add summary notes separately |
| Scope captures | Keep original images and label them |
| Logic analyzer files | Keep original capture files if possible |
| Measurements | Keep raw values and calculated summaries |
| Failure logs | Keep original failure notes and update with fix results |
| Photos | Keep original photos and optional annotated copies |

Raw data helps verify later conclusions.

---

## How To Summarize A Test

A test summary should answer:

- What was tested?
- Why was it tested?
- What hardware was used?
- What firmware was used?
- What was expected?
- What happened?
- Did it pass?
- What should happen next?

A summary should not replace raw data if raw data exists.

---

## Test Result Example

Example:

```markdown
# Button Input Test - Interposer v0.1

## Test Information

| Item | Value |
| --- | --- |
| Date | 2026-05-15 |
| Tester | FBD |
| Board | Button Interposer |
| Board Revision | v0.1 |
| Firmware Version | v0.1.0 |
| Console Model | SCPH-79001 |
| Test Status | Pass |
| Confidence | Medium |

---

## Purpose

Confirm that the firmware can detect button press and release events.

---

## Result

The firmware reported `EVT BTN_PRESS` and `EVT BTN_RELEASE` consistently during repeated button presses.

No false presses were observed during this test.

---

## Next Action

Add short press and long press timing tests.
```

---

## Known Test Gaps

Current known gaps:

- No Button Interposer test data exists yet.
- No Touchscreen Hub test data exists yet.
- No I/O Expansion Board test data exists yet.
- No Power Management Board test data exists yet.
- No console test data exists yet.
- No heat test data exists yet.
- No backfeeding test data exists yet.
- No failure reports exist yet.

This section should be updated as tests are completed.

---

## Open Questions

Open test-data questions:

- Which PS2 model should be used for the first console test?
- Which button signal should be measured first?
- What minimum runtime should count as a basic stability test?
- What temperature limit should be considered acceptable?
- What current draw is acceptable for standby-powered hardware?
- What backfeeding voltage should be considered a fail?
- Should UART logs be saved as `.txt`, `.log`, or `.md`?
- Should scope captures be stored as images or original project files?
- Should test summaries be written after every test session?
- Should failed tests be linked from the QA checklist?

These questions should be answered as testing begins.

---

## Summary

The `Test-Data/` folder is where Button Butler testing becomes real.

This folder should contain logs, measurements, console test results, heat data, failure reports, and validation notes.

The main goal is to prove what works, expose what fails, and keep the project honest.

A feature should not be called stable until the test data supports it.
