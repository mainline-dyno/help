---
title: "Ramp Test Setup"
description: "How to configure the ramp test settings"
---

## Overview
The Ramp Test Settings dialog in the Mainline DynoLog dynamometer software, part of the CAN Control System (CCS), allows operators to configure controlled acceleration and deceleration tests for accurate vehicle performance measurement.

Ramp tests replicate real-world conditions by ramping speed from a start to a peak, with optional holds, followed by deceleration. The dyno uses eddy current braking for precise load control. Tests capture power, torque, inertia, and other metrics, with triggering via speed, RPM, or torque. Data logs in real-time, graphs (e.g., power/torque vs. speed/RPM), and saves to the database.

The graphical preview displays the profile: orange for holds, green/yellow for ramps. Summary calculations convert between Eng RPM, Dyno RPM, and Speed, factoring in ratios.

## Ramp Profile Configuration
Define the test's speed curve. The graph previews the profile for quick validation.

- **Ramp Type**: Selects the curve type. Options include:
  - **Linear**: Constant rate (default; detailed below).
  - **Non Linear**: Customizable acceleration profile defined by a user-editable table of time vs. speed targets. Supports user-definable non-linear ramps for simulating complex scenarios like drag strip runs (over distance, time, vehicle weight). Also includes presets or sub-modes for Non-Linear and User Non-Linear acceleration tests, as well as Drag Mode Simulations.

In Linear mode, configure:
- **Start Speed (RPM)**: Ramp-up start point after pre-start. Default: 2500.0 RPM.
- **Pre-start Speed (RPM)**: Initial hold for preparation. Default: 2000.0 RPM. Auto-adjustable.
- **Ramp-up Rate (RPM/s) or Ramp Time (s)**: Increase rate from Start to End Speed. Input one; software calculates the other. Default: 500.0 RPM/s (9.0 s). Rates can mimic track conditions (e.g., 400-500 RPM/s for higher gears). ProHub supports up to 600+ RPM/s safely.
- **End Speed (RPM)**: Peak after ramp-up.
- **End Hold Delay (s)**: Hold at End Speed.
- **Ramp-down Rate (RPM/s)**: Deceleration rate.
- **Finish Speed (RPM)**: End point after ramp-down.
- **Finish Hold Delay (s)**: Hold at Finish Speed.


In Non Linear mode, the ramp-up phase uses a table-based profile instead of fixed rates:
- The graph shifts to Speed Target (RPM) vs. Time (s), previewing the custom curve.
- **Table Operations**:
  - **Save Table**: Exports the current table to a file for reuse.
  - **Load Table**: Imports a saved table file to populate the profile.
  - **Copy TSV/Paste TSV**: Copies/pastes tab-separated values (e.g., for editing in text editors).
  - **Copy CSV/Paste CSV**: Copies/pastes comma-separated values (e.g., for Excel or spreadsheets).
- **Editing the Table**: Add/edit points with Time (s) and Speed Target (RPM). Points are interpolated (likely linearly) to form the curve. Table Size displays the number of points (e.g., 0 for empty). Status shows "OK" if valid; errors if points are unsorted or invalid.
- **Non Linear Ramp/Linear Ramp**: Toggle or select sub-modes within Non Linear for preset non-linear curves or revert to linear previews.
- Ramp-down and holds remain as in Linear mode, applied after the custom profile reaches End Speed.

Non Linear is ideal for advanced simulations, such as drag runs or variable acceleration tests. Define points starting from Time 0 (Pre-start Speed) and increasing to End Speed. Ensure points are chronologically ordered for smooth interpolation.

## Auto Arm Configuration
Automates readiness:
- **Auto Arm Test Preset (RPM)**: Arms above threshold.
- **Auto Disarm Below Arm (RPM)**: Disarms if below after arming.

## Auto Stop Configuration
Automatic termination:
- **End Test on Torque Drop**: Stops on torque drop (e.g., fuel cut).
- **Enable Trigger Min**: Activates min trigger.
- **External Start Configuration**: Uses external signals.

## Setting Automation
Streamlines setups:
- **Automatically Adjust Pre-start Speed**: Sets dynamically via offset. Default difference: 500.0 RPM.
- **Finish Speed Same As Start Speed**: Mirrors for symmetry.
- **Pre-start Speed Difference (RPM)**: Offset for auto-adjust.
- **Finish Speed Same As Start Speed**

## Speed +/- Step
Increments for adjustments:
- **Speed +/- Step**
- **Rate +/- Step**
- **Time +/- Step**

## Auto Start Configuration
Hands-free start:
- **Auto Start Test**: Enables on criteria.
- **Auto Start Type**: E.g., "Torque".
- **Auto Start Torque Min (Nm)**: Trigger threshold.
- **Auto Start Settle Time (s)**: Post-trigger delay.

## Trigger and Enable Channels
External integration:
- **Trigger Channel**: Input (e.g., "Switch 1").
- **Trigger Edge**: E.g., "Rising Edge".
- **Enable Channel**: For enable/disable.
- **Enable Indicator Min**: Min value.
- **Enable If Greater Than**: Threshold.

## Start Hold Configuration
Initial management:
- **Minimum Start Speed Offset (RPM)**: For min start. Default: 2000.0 RPM.
- **Resulting Minimum Start Speed (RPM)**: Computed (e.g., 0 RPM).

## Performing a Ramp Test
1. Enter vehicle details in CCS database.
2. Select Ramp Type (Linear or Non Linear); configure profile (rates for Linear, table for Non Linear).
3. Enter ramp test mode.
4. Accelerate to Pre-start Speed; stabilize.
5. Start (auto on torque/speed or manual).
6. Monitor graphs.
7. Ends at Finish Speed, trigger, or manually.
8. Review/save data for export (CSV/Excel) or reports.
