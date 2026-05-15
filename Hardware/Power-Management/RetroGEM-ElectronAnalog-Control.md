# RetroGEM And ElectronAnalog Control

## Overview

This document covers early planning notes for **RetroGEM** and **ElectronAnalog** power/control integration in the **FBD PS2 Button Butler** project.

RetroGEM and ElectronAnalog are internal video-output upgrades that may be installed in advanced PlayStation 2 builds.

The goal of Button Butler power management is not to interfere with video hardware in a risky way.

The goal is to explore safe methods for controlling or managing video-board power so advanced builds can reduce heat, save power, and provide better user control through the Button Butler touchscreen or control system.

---

## Purpose

The purpose of this document is to define possible ways Button Butler could interact with internal video boards.

Possible goals:

- Enable video board power
- Disable video board power
- Show video board power state
- Add touchscreen control
- Add service-mode control
- Reduce idle heat
- Save power when the console is not active
- Provide manual override
- Avoid unnecessary runtime for internal video hardware
- Keep video behavior predictable for the user

This document is only a planning reference.

Any video power-control feature must be tested carefully before use in customer builds.

---

## Important Warning

Video hardware should be handled carefully.

Poorly designed video power control can cause:

- No video output
- Video dropouts
- HDMI handshake issues
- Unstable startup behavior
- Backfeeding through video or signal lines
- Heat issues
- User confusion
- Possible damage to video hardware or the console

Do not power-cycle internal video hardware during gameplay or normal use unless that behavior is fully tested and intentional.

---

## Scope

This document focuses on two common internal video upgrade targets:

- RetroGEM
- ElectronAnalog

The exact control method may be different for each board.

This document does not define a finished circuit yet.

It collects possible control ideas, risks, testing goals, and design rules.

---

## Button Butler Role

Button Butler may interact with video hardware through:

- Digital enable output
- Accessory power enable signal
- Load switch enable signal
- Regulator enable signal
- Touchscreen command
- Service mode command
- Power-good input
- Status input
- Manual override state

Button Butler should not directly modify video signals.

If power control is used, it should control the accessory power path or enable input only.

---

## Recommended Control Philosophy

The recommended approach is:

1. Do not touch video signal lines.
2. Do not interrupt critical console video paths.
3. Start with manual control only.
4. Use enable pins where available.
5. Use a proper load switch if power switching is needed.
6. Add power-good feedback if useful.
7. Add touchscreen status only after the hardware works.
8. Add automatic power saving last.

Manual control and safe defaults should come before automation.

---

## RetroGEM Notes

RetroGEM is an advanced internal digital video upgrade.

Possible Button Butler interactions:

- Enable or disable RetroGEM power
- Monitor RetroGEM power-good if available
- Show RetroGEM power state on the touchscreen
- Allow service-mode power testing
- Provide manual override
- Possibly reduce idle heat in compact builds

RetroGEM power behavior must be tested carefully.

If the board expects to be powered whenever the console is active, Button Butler should not interrupt it without a proven safe method.

---

## ElectronAnalog Notes

ElectronAnalog is an internal analog-to-HDMI style video output solution often used in FBD-style PS2 builds.

Possible Button Butler interactions:

- Enable or disable ElectronAnalog power
- Control the regulator or load switch feeding ElectronAnalog
- Show HDMI board power state
- Add manual touchscreen control
- Add service-mode testing
- Reduce heat when the console is idle or off

ElectronAnalog may be a practical early target for basic video-board power control because it is often installed as an internal accessory board.

Actual behavior still needs to be measured and tested.

---

## Main Goals

Possible goals for video power control:

| Goal | Description |
| --- | --- |
| Heat reduction | Turn off video board power when not needed |
| Power saving | Reduce unnecessary current draw |
| Manual control | Let the installer or user control video board power |
| Touchscreen status | Show whether video power is enabled |
| Service testing | Allow installer testing from a service page |
| Safe defaults | Keep normal video behavior predictable |
| Fault awareness | Detect whether video power failed if feedback exists |

The first goal should be safe manual control, not automation.

---

## Non-Goals

Early video power control should not try to:

- Modify video signal routing
- Switch HDMI lines
- Interfere with PS2 video encoder signals
- Power-cycle video hardware during gameplay
- Automatically disable video without user awareness
- Depend on touchscreen control for basic video output
- Replace the video board’s own power design
- Control unknown pins without documentation or testing

Keep the system simple and safe.

---

## Possible Control Methods

Possible video power-control methods:

| Method | Description | Notes |
| --- | --- | --- |
| Regulator enable | Control the enable pin of a regulator feeding the video board | Usually clean if available |
| Load switch IC | Switch accessory power with a dedicated load switch | Good final design candidate |
| High-side MOSFET | Switch positive supply to video board | Must be designed carefully |
| Low-side MOSFET | Switch ground return | Usually not preferred for video boards |
| Manual jumper | Installer-only enable/disable | Useful for testing |
| Touchscreen command | User requests power state change | Should go through Button Butler safety logic |
| Service command | Installer tests video power from service mode | Useful during development |

The best method depends on the installed video board and power path.

---

## Preferred Method

The preferred method is usually:

1. Use a regulator enable pin if the video board or added regulator provides one.
2. Use a dedicated load switch IC if the power rail must be switched.
3. Avoid low-side switching for video boards.
4. Avoid switching video signal lines.

Regulator enable or load switch control is cleaner than lifting grounds or interrupting signals.

---

## Why Low-Side Switching Is Not Preferred

Low-side switching disconnects the ground path.

This can cause problems with video hardware because:

- The board ground may float.
- HDMI or video signal lines may still be connected.
- Signal lines may backfeed the board.
- Audio or video noise may increase.
- The board may partially power through protection diodes.
- Debugging becomes more confusing.

Low-side switching may be acceptable for LEDs or buzzers, but it is not ideal for video boards.

---

## High-Side Switching

High-side switching controls the positive supply to the video board.

Possible advantages:

- Video board ground remains connected.
- Signal reference stays stable.
- Better for boards connected to PS2 signals.
- Cleaner than low-side switching.

Possible concerns:

- Needs correct MOSFET or load switch selection.
- Must handle inrush current.
- Must prevent reverse current if needed.
- Must avoid startup glitches.
- Must be rated for the accessory current.

A dedicated load switch IC may be better than a hand-built MOSFET switch for final hardware.

---

## Load Switch IC Option

A load switch IC is a strong option for video power control.

Possible advantages:

- Clean enable pin
- Controlled turn-on behavior
- Current limiting on some parts
- Thermal shutdown on some parts
- Reverse-current blocking on some parts
- Small package
- Predictable behavior

Possible design targets:

- One load switch for RetroGEM power
- One load switch for ElectronAnalog power
- One shared video accessory rail
- Separate video power-good feedback if supported

The selected part must match the board current draw and voltage.

---

## Regulator Enable Option

If the video board is powered by a dedicated regulator, Button Butler may control the regulator enable pin.

Possible advantages:

- Low-current control
- Fewer high-current traces on Button Butler
- Cleaner layout
- Easier to isolate
- Lower heat on the control board

Possible concerns:

- Not every regulator has an enable pin.
- Enable logic may be active-high or active-low.
- Startup timing must be tested.
- Signal lines may still backfeed the video board if the regulator is off.

This is likely one of the cleanest control methods if available.

---

## Manual Control First

The first video power-control prototype should be manual.

Manual control methods:

- Service jumper
- UART command
- Touchscreen service page
- Physical test pad
- Temporary bench switch

Automatic behavior should not be added until manual control is proven safe.

---

## Touchscreen Control

The Button Butler touchscreen may eventually include a video power page.

Possible controls:

- Video Board ON
- Video Board OFF
- Auto Power Saving ON/OFF
- Manual Override
- Restore Default
- Service Test
- Show Power-Good State
- Show Warning If Video Is Off

The touchscreen should request video power actions from the Button Interposer or Power Management Board.

The touchscreen should not assume the video board changed state unless it receives confirmation.

---

## Suggested Touchscreen Labels

Possible touchscreen labels:

| Label | Meaning |
| --- | --- |
| `Video Power` | General video power page |
| `HDMI Board` | Generic user-friendly label |
| `RetroGEM` | RetroGEM-specific page or state |
| `ElectronAnalog` | ElectronAnalog-specific page or state |
| `Video ON` | Video board power enabled |
| `Video OFF` | Video board power disabled |
| `Auto Save` | Automatic power saving enabled |
| `Manual Override` | User is forcing power state |

Customer-facing labels should be simple and clear.

---

## Warning Message Ideas

If video power can be disabled, the UI should warn the user.

Possible warning messages:

| Situation | Possible Message |
| --- | --- |
| User taps Video OFF | Turning this off may disable HDMI video. Continue? |
| Video board off | HDMI board power is off |
| Power-good missing | HDMI board power not confirmed |
| Command rejected | Video power action blocked |
| Auto mode active | Video power is controlled automatically |
| Service mode only | Video power control requires service mode |

Warnings should be short enough for a small screen.

---

## Command Ideas

Possible UART or internal command ideas:

| Command | Purpose |
| --- | --- |
| `VIDEO_PWR ON` | Enable video board power |
| `VIDEO_PWR OFF` | Disable video board power |
| `VIDEO_PWR TOGGLE` | Toggle video board power |
| `VIDEO_PWR?` | Read video board power state |
| `VIDEO_PG?` | Read video power-good state if available |
| `VIDEO_AUTO ON` | Enable automatic video power saving |
| `VIDEO_AUTO OFF` | Disable automatic video power saving |
| `VIDEO_DEFAULT` | Restore build-profile default |
| `VIDEO_LOCK` | Lock video power changes |
| `VIDEO_UNLOCK` | Unlock video power changes if service mode permits |

Video power commands should be protected.

---

## Command Safety

Video power commands should not be treated like simple lighting commands.

Suggested safety levels:

| Command | Safety Level | Notes |
| --- | --- | --- |
| `VIDEO_PWR?` | Safe | Read-only |
| `VIDEO_PG?` | Safe | Read-only |
| `VIDEO_PWR ON` | Limited | Usually safe if rail is valid |
| `VIDEO_PWR OFF` | Protected | Can blank display |
| `VIDEO_PWR TOGGLE` | Protected | Can blank display |
| `VIDEO_AUTO ON` | Protected | Changes automatic behavior |
| `VIDEO_AUTO OFF` | Limited | Usually safer than enabling auto mode |
| `VIDEO_DEFAULT` | Service Only | Changes control state |
| `VIDEO_LOCK` | Service Only | Installer function |
| `VIDEO_UNLOCK` | Service Only | Installer function |

This table should be adjusted after testing.

---

## Default State

The safest default for most builds is likely:

**Video board power on when the console is on.**

Possible default states:

| Build Type | Suggested Default |
| --- | --- |
| Basic build | Video power always follows console power |
| ElectronAnalog build | On when console is on |
| RetroGEM build | On when console is on |
| Premium touchscreen build | On by default, manual control available |
| Service/test build | Manual control allowed |
| Low-power experimental build | Profile-dependent |

Avoid defaulting video power off unless there is another guaranteed video output path.

---

## Safe State

A safe state should avoid confusing the user.

Possible safe-state behavior:

- Keep video board powered when console is on
- Disable automatic video-off behavior
- Lock video-off commands unless service mode is active
- Report state to touchscreen
- Allow manual restore to video on
- Avoid cycling video power repeatedly

In most cases, safe state should favor keeping video available.

---

## Manual Override

Manual override is important.

Possible manual override options:

- Force video power on
- Force video power off in service mode
- Disable automatic power saving
- Restore profile default
- Lock video power state

Manual override is useful during testing and troubleshooting.

---

## Auto Power Saving

Automatic power saving should be added only after manual control is stable.

Possible automatic behavior:

- Turn video board off when console is off
- Turn video board off after long idle time
- Turn video board off only in standby
- Keep video board on during gameplay
- Keep video board on during service mode
- Wake video board before console power-on if needed

Auto behavior must not surprise the user.

---

## Idle Detection

Auto power saving depends on idle detection.

Possible idle signals:

- Console switched power off
- Console standby state
- Touchscreen inactive
- No service mode active
- No reset or startup sequence active
- Optional timer expired
- Optional accessory state indicates idle

Idle detection should be conservative.

It is better to leave video hardware on than to turn it off at the wrong time.

---

## Startup Sequence

Video power startup must be predictable.

Possible startup sequence:

1. Button Butler powers up.
2. Outputs default to safe state.
3. Video power enable defaults according to build profile.
4. Video rail turns on if required.
5. Power-good is checked if available.
6. Touchscreen shows video power state.
7. Console power/reset behavior continues normally.

The video board should not receive repeated enable glitches during startup.

---

## Shutdown Sequence

Possible shutdown sequence:

1. Console enters standby or powers off.
2. Button Butler detects console-off state if available.
3. Noncritical accessories may be disabled.
4. Video board power may turn off if the profile allows it.
5. Touchscreen records or displays state if still powered.
6. Control lines return to safe states.
7. No signal line backfeeds the video board.

Shutdown behavior must be tested for backfeeding.

---

## Power-Good Feedback

A power-good signal can be useful if available.

Possible uses:

- Confirm video rail is active.
- Show video power state on touchscreen.
- Detect failed load switch or regulator.
- Delay commands until video power is stable.
- Report fault if video power fails.

Possible status values:

| Status | Meaning |
| --- | --- |
| `PGOOD=ON` | Video rail appears valid |
| `PGOOD=OFF` | Video rail is not valid |
| `PGOOD=UNKNOWN` | No feedback available |
| `PGOOD=FAULT` | Expected state does not match feedback |

Power-good should be tested before relying on it.

---

## Status Reporting

Button Butler may report video state.

Possible status values:

| State | Meaning |
| --- | --- |
| `VIDEO_PWR=ON` | Video power enable is active |
| `VIDEO_PWR=OFF` | Video power enable is inactive |
| `VIDEO_PWR=AUTO` | Automatic control is enabled |
| `VIDEO_PWR=LOCKED` | Control is locked |
| `VIDEO_PG=ON` | Power-good reports valid |
| `VIDEO_PG=OFF` | Power-good reports invalid |
| `VIDEO_STATE=UNKNOWN` | State cannot be confirmed |

The touchscreen should display unknown states clearly.

---

## Backfeeding Risks

Video boards may have several possible backfeeding paths.

Possible backfeeding paths:

- Video signal lines
- HDMI-related lines
- Audio lines
- UART or control lines
- Enable lines
- Power-good lines
- Pull-up resistors
- Ground reference issues
- Debug adapter connections

Backfeeding can cause partial power-up, unstable behavior, or damage.

---

## Backfeeding Prevention

Possible prevention methods:

- Use high-side switching instead of low-side switching.
- Avoid driving signals into an unpowered video board.
- Use series resistors on control lines.
- Use open-drain enable control where appropriate.
- Avoid pull-ups to rails that may be off.
- Use load switches with reverse-current blocking if needed.
- Confirm no voltage appears on the video board power rail when it is disabled.
- Test with a meter before connecting to the console.

Backfeeding tests are required before considering this feature stable.

---

## Inrush Current

Video boards may draw inrush current when powered on.

Possible causes:

- Input capacitors
- HDMI circuitry
- FPGA or processor startup
- Regulators
- Filtering capacitors

Possible mitigation:

- Load switch with controlled slew rate
- Soft-start regulator
- Current-limited switch
- Proper bulk capacitance
- Avoid turning on several accessories at once

Inrush should be measured during testing.

---

## Heat Considerations

One reason to control video power is heat reduction.

Possible heat sources:

- Video board regulator
- FPGA or processor
- HDMI transmitter
- Internal converter board
- Nearby PS2 regulators
- Tight shell space
- Poor airflow

Testing should compare:

- Console with video board always on
- Console with video board power-managed
- Console idle temperature
- Console gameplay temperature
- Shell temperature after long runtime

Heat data should be saved in the repo.

---

## Current Measurement

Before designing the power switch, measure current draw.

Useful measurements:

| Measurement | Purpose |
| --- | --- |
| Video board idle current | Baseline load |
| Video board active current | Normal operating load |
| Startup inrush current | Switch stress |
| Console total current | System impact |
| Standby current | Always-on impact |
| Load switch temperature | Thermal margin |
| Regulator temperature | Heat behavior |

Measurements should be taken with real hardware.

---

## Noise Considerations

Switching video board power can create noise.

Possible noise effects:

- Video glitches
- HDMI instability
- Audio noise
- UART errors
- Console reset instability
- Touchscreen noise
- Wireless interference

Possible mitigation:

- Proper decoupling
- Load switch with controlled rise time
- Short power wiring
- Good grounding
- Separate noisy power wiring from signal wiring
- Ferrite or filtering if needed
- Avoid rapid power cycling

Noise should be checked during gameplay and startup.

---

## Grounding

Video boards are sensitive to grounding.

Grounding rules:

- Do not float video board ground if avoidable.
- Keep ground reference stable.
- Use proper ground return paths.
- Avoid long skinny ground wires for power return.
- Keep audio and video grounding clean.
- Avoid forcing video board current through small signal grounds.
- Confirm common ground between Button Butler and video board.

High-side switching is preferred partly because it keeps ground connected.

---

## Control Line Electrical Style

Control lines should be safe.

Possible styles:

| Style | Use |
| --- | --- |
| Open-drain | Pull-low enable or disable signals |
| Push-pull | Same-voltage known logic input |
| Series resistor | Added protection for signal lines |
| Level shifter | Different voltage domains |
| Buffer | Clean logic isolation |
| Optocoupler | Usually unnecessary but possible in special cases |

The control style depends on the video board and load switch used.

---

## Enable Line Default

The enable line needs a defined state.

Possible default behavior:

| Enable Default | Meaning |
| --- | --- |
| Pulled inactive | Video board stays off until enabled |
| Pulled active | Video board stays on unless disabled |
| Profile controlled | Firmware sets state after boot |
| Hardware forced on | Video board remains on even if MCU is missing |
| Service selectable | Jumper selects default behavior |

For customer-safe behavior, hardware-forced-on or profile-forced-on may be safest for video output.

---

## Fail-Safe Behavior

Fail-safe behavior should favor preserving video.

Possible fail-safe goals:

- If Button Butler fails, video board stays powered when console is on.
- If touchscreen fails, video board stays in default state.
- If UART fails, video board does not toggle randomly.
- If MCU resets, video power does not glitch.
- If service mode exits, video returns to safe default.
- If auto mode fails, video stays on.

A video output failure is highly visible to the user, so fail-safe design matters.

---

## RetroGEM Control Concept

A possible RetroGEM control concept:

| Signal | Purpose |
| --- | --- |
| `RG_EN` | Enables RetroGEM power path or regulator |
| `RG_PG` | Optional power-good feedback |
| `RG_LOCK` | Firmware/software lockout state |
| `RG_OVERRIDE` | Manual override from service mode |
| `RG_STATUS` | Reported state to touchscreen |

Possible behavior:

- Default state is on when console is on.
- Video-off command requires confirmation.
- Auto mode is disabled by default.
- Service mode can test power control.
- Power-good fault is shown on touchscreen if available.

This is only a concept.

---

## ElectronAnalog Control Concept

A possible ElectronAnalog control concept:

| Signal | Purpose |
| --- | --- |
| `EA_EN` | Enables ElectronAnalog power path or regulator |
| `EA_PG` | Optional power-good feedback |
| `EA_LOCK` | Firmware/software lockout state |
| `EA_OVERRIDE` | Manual override from service mode |
| `EA_STATUS` | Reported state to touchscreen |

Possible behavior:

- Default state is on when console is on.
- Manual off is allowed only from service or confirmed touchscreen action.
- Auto-off may be tested later.
- Power-good feedback is optional.
- Touchscreen shows HDMI board power state.

This may be a practical early video-power test target.

---

## Shared Video Power Rail Concept

Some builds may use one shared video accessory rail.

Possible shared rail targets:

- ElectronAnalog
- RetroGEM
- HDMI support board
- Video-related accessory circuit

Possible signals:

| Signal | Purpose |
| --- | --- |
| `VID_EN` | Enables shared video accessory rail |
| `VID_PG` | Reports shared rail valid |
| `VID_OUT` | Switched output to video accessory |
| `VID_GND` | Ground return |
| `VID_FAULT` | Optional fault output from load switch |

A shared rail is simpler, but less flexible than separate control per board.

---

## Separate Video Control Concept

Some builds may need separate control.

Possible separate controls:

| Signal | Purpose |
| --- | --- |
| `RG_EN` | RetroGEM enable |
| `RG_PG` | RetroGEM power-good |
| `EA_EN` | ElectronAnalog enable |
| `EA_PG` | ElectronAnalog power-good |
| `VID_AUX_EN` | Extra video accessory enable |

Separate control is more flexible but uses more outputs and wiring.

---

## Service Mode Use

Service mode may allow video power testing.

Possible service functions:

- Read video power state
- Turn video board on
- Turn video board off
- Read power-good state
- Test load switch
- Measure current draw
- Restore default state
- Lock video power on
- Disable automatic mode

Service mode should clearly warn if video may turn off.

---

## Test Procedure

A safe test procedure should start outside the console.

Recommended order:

1. Identify video board power input.
2. Measure voltage and current draw.
3. Identify whether an enable pin exists.
4. Build a bench test power switch.
5. Test with dummy load.
6. Test with the video board outside the console if possible.
7. Check inrush current.
8. Check heat.
9. Check power-good behavior if available.
10. Check for backfeeding when disabled.
11. Install in console with manual control only.
12. Test video startup.
13. Test HDMI handshake.
14. Test repeated power cycling.
15. Test gameplay.
16. Test shutdown.
17. Document results.

Do not skip dummy-load testing.

---

## Bench Testing Checklist

Bench testing should include:

- Correct input voltage
- Correct switched output voltage
- Enable signal works
- Enable default state is correct
- No startup glitch
- No shutdown glitch
- Dummy load works
- Video board powers up
- Current draw is measured
- Inrush behavior is observed
- Load switch or MOSFET temperature is checked
- No backfeed voltage appears when disabled
- Power-good signal works if used

Bench testing should be documented before console testing.

---

## Console Testing Checklist

Console testing should include:

- Console works before modification
- Video board works without Button Butler control
- Power control circuit installed but forced on
- Console boots normally
- HDMI output appears normally
- Video power state is reported correctly
- Manual video off behaves as expected
- Manual video on restores output
- No random video loss occurs
- No extra heat problem appears
- No backfeeding occurs when disabled
- No startup delay causes problems
- Shell closes cleanly
- Long-duration gameplay works

Do not use automatic video power saving until manual control is stable.

---

## Heat Test Checklist

Heat testing should include:

- Video board always on
- Video board manually off when console idle
- Console idle temperature
- Console gameplay temperature
- Shell closed temperature
- Touchscreen active temperature
- Internal accessory temperature
- Long-duration test

Record whether power management actually improves heat enough to justify the feature.

---

## User Experience Notes

Video power control can confuse users if not presented clearly.

Customer-facing rules:

- Do not hide video power state.
- Do not turn video off without clear reason.
- Do not make video output disappear unexpectedly.
- Show a warning before disabling video.
- Provide an easy way to restore video power.
- Avoid making video power control necessary for normal use.

The user should not think the console is broken because the video board was turned off.

---

## Suggested Touchscreen Page

A simple video page could include:

| UI Item | Function |
| --- | --- |
| Video Power State | Shows ON/OFF/UNKNOWN |
| Power-Good State | Shows valid/invalid if available |
| Turn On Button | Requests video power on |
| Turn Off Button | Requests video power off with confirmation |
| Auto Mode Toggle | Enables/disables future automatic behavior |
| Restore Default | Returns to build-profile default |
| Service Lock Indicator | Shows if controls are locked |

Keep the page simple.

---

## Suggested Default UI Behavior

Recommended first UI behavior:

- Show video board as ON by default.
- Allow read-only status first.
- Add manual ON/OFF in service mode.
- Add customer-facing OFF only after testing.
- Require confirmation before OFF.
- Keep Auto Mode hidden until proven.

This avoids exposing risky features too early.

---

## Documentation Requirements

Each tested video-control setup should document:

- Video board used
- Console model
- Motherboard revision if known
- Power source
- Control method
- Load switch or regulator part number
- Current draw
- Inrush behavior if measured
- Heat results
- Backfeeding results
- Startup behavior
- Shutdown behavior
- HDMI handshake behavior
- Firmware version
- Hardware revision
- Test date
- Known issues

This information should be saved with the build notes.

---

## Suggested Test Data Location

Suggested folders:

| Folder | Purpose |
| --- | --- |
| `Test-Data/Power-Measurements/` | Current, voltage, and power data |
| `Test-Data/Heat-Testing/` | Temperature and runtime notes |
| `Test-Data/UART-Logs/` | Command and status logs |
| `Hardware/Power-Management/Test-Notes/` | Board-specific test notes |
| `Hardware/Power-Management/Images/` | Photos and board screenshots |

Good data will show whether video power control is worth keeping.

---

## Open Questions

Open questions:

- Should video power control be included in early hardware?
- Should RetroGEM control be delayed until later?
- Is ElectronAnalog a better first test target?
- Should video power default to always on?
- Should there be a hardware jumper to force video power on?
- Should video-off be service-mode only?
- Should automatic video power saving ever be customer-facing?
- Which load switch IC is best for the expected current?
- Is reverse-current blocking required?
- What is the inrush current of each video board?
- Does disabling power cause HDMI handshake problems?
- Does the video board backfeed through signal lines?
- What is the actual heat reduction benefit?
- Should video power state be stored or always profile-default?
- Should the touchscreen hide video controls unless installed?

These questions should be answered through testing.

---

## Known Issues

Current known issues:

- No final control circuit has been selected.
- RetroGEM power behavior has not been tested in this project.
- ElectronAnalog power behavior has not been tested in this project.
- Load switch part is not selected.
- Power-good method is not selected.
- Default state is not finalized.
- Automatic behavior is not defined.
- Backfeeding behavior is unknown.
- Heat reduction benefit is not measured.
- No bench test has been completed.
- No console test has been completed.

This section should be updated as testing begins.

---

## Risks

Possible risks:

- HDMI output disappears unexpectedly
- Video board fails to start
- HDMI handshake fails after power cycling
- Video board backfeeds when disabled
- Console startup becomes unstable
- Load switch overheats
- MOSFET overheats
- Inrush current causes voltage dip
- User thinks the console is broken
- Touchscreen command turns video off accidentally
- Auto power saving turns video off during use
- Power-good state is misread
- Different video boards behave differently
- Different PS2 models behave differently

Video power control should be considered an advanced feature until proven.

---

## Recommended Development Order

Recommended order:

1. Document existing video board power wiring.
2. Measure current draw.
3. Decide whether enable control or load switching is needed.
4. Build a dummy-load test circuit.
5. Test one load switch or regulator enable method.
6. Test with ElectronAnalog or another lower-risk accessory first.
7. Check backfeeding.
8. Check startup and shutdown behavior.
9. Add manual service-mode control.
10. Add touchscreen read-only status.
11. Add touchscreen manual control with confirmation.
12. Add power-good feedback if useful.
13. Add automatic behavior only after long-term testing.

Do not start with automatic video-off behavior.

---

## Minimum First Prototype

A useful first prototype could include:

- One video accessory power input
- One switched video accessory output
- One enable input
- One load switch or regulator enable circuit
- One optional power-good output
- Test points for input voltage, output voltage, enable, and ground
- Hardware default that keeps video on or safely inactive depending on test goal

For customer builds, a force-on jumper may be useful.

---

## Force-On Jumper Idea

A hardware force-on jumper may be useful.

Possible benefits:

- Keeps video board powered even if firmware fails
- Useful during troubleshooting
- Allows bypassing Button Butler video control
- Reduces risk in customer builds
- Allows safe testing of power-control firmware

Possible jumper states:

| Jumper State | Behavior |
| --- | --- |
| Force ON | Video board power stays enabled |
| MCU Control | Button Butler controls video power |
| Force OFF | Service/testing only, video board disabled |

A force-on option may be important for video output safety.

---

## Summary

RetroGEM and ElectronAnalog power control may become useful advanced features for the FBD PS2 Button Butler project.

The main benefits are possible heat reduction, power saving, touchscreen control, and service testing.

However, video power control is riskier than simple RGB or Bluetooth control because it can directly affect the user’s ability to see the console output.

The safest development path is to start with measurement, manual control, dummy-load testing, and read-only touchscreen status.

Automatic video power saving should only be added after the hardware is proven reliable.

For most builds, the safest default is to keep the video board powered whenever the console is on.
