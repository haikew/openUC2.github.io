# Linear Encoder Test Report

This report covers three tests on the Frame 10: a 24 h long-run test with step-loss recovery, thermal drift over 12 h, and backlash.

## Results at a glance

| Test | Result |
|---|---|
| 24 h, 779 moves between slot 1 and slot 4 | Position stays within ±7.8 µm (4 counts); all 4 lost-step events recovered |
| 12 h thermal drift | About 0.1 µm/h along X, 0.2 µm/h along Y |
| Backlash | About 1–15 µm, depending on slot, Y position and speed |

**Common setup**:

- Frame 10 with a 20x objective lens
- AS5311 magnetic linear encoder on the X axis, 1.95 µm/count
- Black dot or grid cross on the calibration slide as the reference target
- Room temperature
- All shifts are measured by the camera, with calibration.

## 24 h test

**Setup** (in addition to the common setup):

- Calibration slide in slot 1
- Motor in open loop (mode 0). A Python script applies the compensation in the compensation zone; this will later move into the firmware.
- Speed 100,000 steps/s, acceleration 100,000 steps/s²

**Method:** Two target positions are given, one in slot 1 and one in slot 4, 92.5 mm apart. Every 10 min, the stage moves to the other position with 2–5 random movements, then waits another 10 min, goes back to slot 1, and takes a picture of the calibration slide.

**Results:**

![24 h: position error along the X axis at the calibration slide, with the compensation zone and the lost-step events](encoder_test_report_images/position_error.png)

*Position error: X position of the stage at the calibration slide measured by the camera, relative to the first picture. Shaded band: compensation zone of ±7.8 µm (4 counts). Dashed lines: the 4 lost-step events.*

![24 h: encoder error, encoder minus photo along the X axis](encoder_test_report_images/encoder_error.png)

*Encoder error: position reported by the encoder minus position measured by the camera (0 = the encoder saw the shift).*

- The maximum deviation between the real position and the position reported by the encoder is around 8 µm. The compensation range should be set to 4 counts for the linear encoder.
- The linear encoder keeps the stage within the compensation zone over 24 h and recovers from step-loss events.

## 12 h thermal drift

**Method:** Take a picture of the black dot every 10 min for 12 h. Use the first picture as the reference and calculate the shift along the X and Y axes.

**Results:**

![12 h drift: shift of the photos against the first one](encoder_test_report_images/2_1_drift_shift.png)

*Shift of the stage measured from the photos, relative to the first photo.*

![12 h drift: linear encoder reading against the photo](encoder_test_report_images/2_2_drift_encoder.png)

*Black dots: linear encoder reading (1.95 µm/count). Red line: x shift measured from the photos.*

- The thermal drift is around 0.1 µm/h along the X axis (red line) and 0.2 µm/h along the Y axis (blue line).

## Backlash test

**Method:** Move in one direction for 256 steps, then take the first picture of the calibration slide. Move the motor 20,000 steps further, return to the starting position, and take another picture. Calculate the shift from the two pictures and compare it with the linear encoder.

| Run | Speed | Acceleration |
|---|---|---|
| Slow | 10,000 steps/s | 100,000 steps/s² |
| Fast | 100,000 steps/s | 1,000,000 steps/s² |

**Results:**

![Backlash of the X axis per slot and Y position, slow and fast](encoder_test_report_images/4_3_backlash.png)

*Backlash at slots 2 and 4, each measured at an upper ("up") and a lower ("down") Y position, 10 rounds per run. Filled markers: measured from the image. Open markers: reported by the encoder.*

- Backlash depends on position and speed. Lower speeds, a centered position, and being closer to the motor all result in higher backlash.


