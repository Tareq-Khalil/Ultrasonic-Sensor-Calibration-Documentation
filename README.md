# ORBIT-Sensor-Calibration-Documentation

## HC-SR04 Ultrasonic Sensor — Calibration

## Group 10345 — [ORBIT]
---
### 1. Purpose
Short statement of purpose (1–2 lines):

> This appendix documents the calibration procedure, raw data, calculation and software implementation used to correct systematic measurement error of the HC-SR04 ultrasonic distance sensor used in the Capstone project.

---

### 2. Equipment & Conditions
- Sensor: HC-SR04
- Controller: Raspberry Pi
- Tools: Measuring Tape
- Power: 5V regulated
- Target: flat vertical cardboard piece, sensor placed perpendicular to target
- Environment: indoor, 12 °C
- Sampling: 5 readings

---

### 3. Procedure
1. Mount the HC-SR04 so its front face is perpendicular to the target.
2. Place the target at known distances from the sensor.
3. For each nominal distance record 10 measurements, then Compute the arithmetic mean.
4. Repeat the proccess with 5 different distances.
---

### 4. Raw Data (example template)

| Real Distance (cm) | Avg Measured (cm) | Error (Measured − Real) (cm) |
| 10 | 11.2 | +1.20 |
| 20 | 21.0 | +1.00 |
| 40 | 41.8 | +1.80 |
| 60 | 62.1 | +2.10 |
| 100 | 103.4 | +3.40|

*Each "Avg Measured" value above is the mean of 10 readings taken at that nominal distance.*

---

### 5. Calibration model and calculation
We use a linear correction model (simple and effective for HC-SR04 across typical ranges):

\[ D_{corrected} = a \cdot D_{measured} + b \]

Where:  
- \(D_{measured}\) is the raw distance from the sensor (cm)  
- \(D_{corrected}\) is the corrected distance (cm)  
- \(a\) is the scale factor and \(b\) is the offset.

#### Calculation method
Use least-squares linear regression mapping measured -> real (i.e., fit real = a * measured + b). Using the example dataset above the fitted coefficients are:

- **a = 0.975**  
- **b = -0.686 cm**

**Therefore:**

\[ D_{corrected} = 0.975 \cdot D_{measured} - 0.686 \]

---

### 6. Before vs After validation
| Real (cm) | Measured (cm) | Error before (cm) | Corrected (cm) | Error after (cm) |
|---:|---:|---:|---:|---:|
| 10 | 11.2 | +1.20 | 10.23 | +0.23 |
| 20 | 21.0 | +1.00 | 19.78 | -0.22 |
| 40 | 41.8 | +1.80 | 40.05 | +0.05 |
| 60 | 62.1 | +2.10 | 59.84 | -0.16 |
| 100 | 103.4 | +3.40 | 100.09 | +0.09 |

**Result :** Maximum absolute error reduced from **3.40 cm** to **≈0.23 cm** across the tested range.



## MQ-135 Gas Sensor — Calibration
### Group 10345 — [ORBIT]
---

### 1. Purpose
> This appendix documents the calibration procedure, raw data, calculations, software implementation, and limitations for the MQ-135 gas sensor used to monitor relative air-quality in the project.
---
This calibration provides a reproducible baseline (R₀) determination in clean air and documents how to convert raw ADC readings to normalized resistance (Rₛ/R₀).
---

### 2. Equipment & Conditions
- Sensor: *MQ-135*
- Controller: **Arduino / Raspberry Pi** (specify in your report)  
- Supply voltage: 5V 
- Environment: well-ventilated indoor area for clean-air baseline; record temperature & relative humidity during calibration
- Burn-in time: **48 hours** 
- Samples per measurement: **50–100 readings** (to average)

---

### 3. Warm-up
> The MQ-135 was powered for 48 hours prior to calibration to ensure sensor stability and repeatability of readings.

---
From MQ-135 typical clean-air ratio from datasheet:

\[ \frac{R_s}{R_0} \approx 3.6 \quad \text{(clean air)} \]

Therefore:

\[ R_0 = \frac{R_s}{3.6} \]

Using the example R_s ≈ 41.1 kΩ:

\[ R_0 \approx 41{,}100 / 3.6 \approx 11{,}420\ \Omega \]


---

\[ ratio = \frac{R_s}{R_0} \]

This ratio is the calibrated quantity plotted for air-quality events.

---
### 4. Solve for PPM:

\[ \text{PPM} = 10^{m \cdot \log(R_s/R_0) + c} \]

---

### 5. Validation
| Test | Condition | Avg R_s/R_0 | Comment |
|---:|---|---:|---|
| 1 | Clean air | 3.6 | Baseline |
| 2 | Near kitchen smoke | 0.9 | Rapid drop indicates VOCs |

---

| **Team 10345** |
|---------------|
