# Laboratory Activity 4: Analog Input, PWM, and DAC

**Name:** Rhea Leen Castor  
**Course:** BCA188 – Programming for Internet of Things  

## Objective

To test the analog input, PWM output, and DAC output of an ESP32-WROOM using Examples 3–5. The activity also compares the expected and actual results from the ADC, PWM, and DAC.

---

# Example 3: Analog Input

## Materials Used

- ESP32-WROOM Development Board (USB Type-C)
- USB Type-C Data Cable
- 10 kΩ Potentiometer
- Breadboard
- Jumper Wires

## Circuit Wiring

| Potentiometer Pin | ESP32 Connection | Purpose |
| :--- | :--- | :--- |
| VCC | 3V3 | Power |
| SIGNAL | GPIO 34 | Analog input |
| GND | GND | Ground |

## Recorded Results

| Potentiometer Position | Raw ADC Reading | Reported Voltage (mV) |
| :--- | ---: | ---: |
| Minimum | 0 | 142 |
| About ¼ | 612 | 649 |
| Halfway | 1993 | 1727 |
| About ¾ | 3282 | 2718 |
| Maximum | 4095 | 3139 |

## Observation

The ADC reading increased as the potentiometer was rotated from minimum to maximum.

The readings changed slightly at some positions, which is normal for analog input. At the maximum position, the raw ADC reading reached 4095, which is the highest value of a 12-bit ADC.

The voltage values in the table were reported by `analogReadMilliVolts()` and were not measured using a multimeter.

## Documentation

https://github.com/user-attachments/assets/ac7f8c2c-90b0-40a9-a814-3033ec8f5473

**Circuit Setup:**  

<img width="4096" height="2304" alt="IMG_20261002_121848" src="https://github.com/user-attachments/assets/4da3021e-7987-42d5-8e57-150c425cddff" />

**Serial Monitor / Recorded Readings:**  
<img width="1512" height="982" alt="image" src="https://github.com/user-attachments/assets/9d29c8a8-6fe1-408b-9f3e-037fde54aa1b" />

<img width="1512" height="982" alt="Screenshot 2026-10-02 at 12 02 48 PM" src="https://github.com/user-attachments/assets/c666cd33-8f42-497c-a763-c160ea33dab7" />

<img width="1512" height="982" alt="Screenshot 2026-10-02 at 12 03 17 PM" src="https://github.com/user-attachments/assets/5a1a17a4-480c-4355-963f-6d115c1c9e96" />

<img width="1512" height="982" alt="Screenshot 2026-10-02 at 12 03 29 PM" src="https://github.com/user-attachments/assets/287abdf0-3b62-44f6-a137-77b0df3cf27a" />

<img width="1512" height="982" alt="Screenshot 2026-10-02 at 12 03 42 PM" src="https://github.com/user-attachments/assets/ca877a5a-e51f-4bc8-8971-6c884cb7ec64" />

# Example 4: PWM LED Brightness

## Materials Used

- ESP32-WROOM Development Board (USB Type-C)
- USB Type-C Data Cable
- 10 kΩ Potentiometer
- 1 LED
- 330 Ω Resistor
- Breadboard
- Jumper Wires

## Circuit Wiring

### Potentiometer

| Potentiometer Pin | ESP32 Connection | Purpose |
| :--- | :--- | :--- |
| VCC | 3V3 | Power |
| SIGNAL | GPIO 34 | Analog input |
| GND | GND | Ground |

### LED

| Component | Connection | Purpose |
| :--- | :--- | :--- |
| LED Anode (+) | GPIO 19 through 330 Ω resistor | PWM output |
| LED Cathode (-) | GND | Ground |

## Recorded Results

| Knob Position | Raw Input | PWM Setting | Duty Cycle |
| :--- | ---: | ---: | ---: |
| Minimum | 0 | 0 | 0.0% |
| About ¼ | 1062 | 66 | 25.9% |
| Halfway | 2099 | 130 | 51.0% |
| About ¾ | 3105 | 193 | 75.7% |
| Maximum | 4095 | 255 | 100.0% |

The PWM duty cycle was calculated using:

`Duty Cycle (%) = (PWM Setting / 255) × 100`

## Observation

The LED brightness changed when the potentiometer was rotated.

At the minimum position, the PWM setting was 0, and the LED was off. As the potentiometer value increased, the PWM setting also increased and the LED became brighter.

At the maximum position, the PWM setting reached 255, or a 100% duty cycle, giving the LED its highest brightness.

The PWM values are duty-cycle settings and are not DAC voltage measurements.

## Documentation

https://github.com/user-attachments/assets/ac4b8dec-a24f-4296-98c4-3e0fe19ad733

**Circuit Setup:**  

<img width="4096" height="2304" alt="IMG_20261002_140228" src="https://github.com/user-attachments/assets/5876a95b-395c-4d6e-8798-3546f3d93baa" />

<img width="4096" height="2304" alt="IMG_20261002_141924" src="https://github.com/user-attachments/assets/3523874d-5640-4b56-a8e1-6c7421f722fc" />


**PWM / Serial Monitor Results:**  

<img width="1512" height="982" alt="Screenshot 2026-10-02 at 2 23 39 PM" src="https://github.com/user-attachments/assets/2950eb76-200f-4ca5-ad7b-ca7ff5b65fc8" />

<img width="1512" height="982" alt="Screenshot 2026-10-02 at 2 24 33 PM" src="https://github.com/user-attachments/assets/42450f5b-9fa7-46b3-8798-12ba0e863713" />

<img width="1512" height="982" alt="Screenshot 2026-10-02 at 2 25 06 PM" src="https://github.com/user-attachments/assets/59e3d9e3-72c5-417f-88db-7d87fb3cb7a3" />

<img width="1512" height="982" alt="Screenshot 2026-10-02 at 2 25 50 PM" src="https://github.com/user-attachments/assets/24f4fd40-ab18-4e3d-8eae-68cd88746874" />

<img width="1512" height="982" alt="Screenshot 2026-10-02 at 2 26 27 PM" src="https://github.com/user-attachments/assets/8fab57f9-9472-4789-9be2-076755a52291" />

---

# Example 5: DAC Output

## Materials Used

- ESP32-WROOM Development Board (USB Type-C)
- USB Type-C Data Cable
- Breadboard
- Jumper Wires
- Digital Multimeter

## Circuit Wiring

| Connection | ESP32 Connection | Purpose |
| :--- | :--- | :--- |
| Multimeter Positive Probe | GPIO 25 | DAC output |
| Multimeter Negative Probe | GND | Ground reference |

The multimeter was set to DC voltage mode to measure the output from GPIO 25.

## Recorded Results

| Setting | DAC Code | Predicted Voltage | Measured Voltage |
| :---: | ---: | ---: | ---: |
| 1 | 0 | 0.00 V | 0.09 V |
| 2 | 64 | 0.83 V | 0.87 V |
| 3 | 128 | 1.66 V | 1.68 V |
| 4 | 192 | 2.48 V | 2.47 V |
| 5 | 255 | 3.30 V | 3.26 V |

The predicted DAC voltage was calculated using:

`Predicted Voltage = (DAC Code / 255) × 3.3 V`

## Observation

The measured voltage increased as the DAC code increased.

The measured values were close to the predicted values. Small differences can happen because of the actual ESP32 supply voltage, DAC characteristics, wiring, and multimeter accuracy.

The voltages in this table were measured directly from GPIO 25 using a digital multimeter.

## Documentation

Uploading example5.mov…

**Multimeter Setup:**  

[Insert image here]

**DAC Code 0 – 0.09 V:**  

[Insert image here]

**DAC Code 64 – 0.87 V:**  

[Insert image here]

**DAC Code 128 – 1.68 V:**  

[Insert image here]

**DAC Code 192 – 2.47 V:**  

[Insert image here]

**DAC Code 255 – 3.26 V:**  

[Insert image here]

---

# Discussion

## 1. Why is PWM different from DAC output?

PWM and DAC do not produce the same type of signal.

PWM on GPIO 19 rapidly switches between LOW and HIGH. The duty cycle controls how long the signal stays HIGH. This is why it can control the brightness of an LED.

The DAC on GPIO 25 produces an analog voltage based on the DAC code. For example, a higher DAC code produces a higher output voltage.

On an oscilloscope, PWM would appear as a square wave, while a fixed DAC output would appear as an approximately steady voltage level.

Because of this, the PWM setting should not be treated as a DAC voltage measurement.

## 2. Why can the ADC endpoint saturate?

The ESP32 uses a 12-bit ADC, so its raw reading can only go from 0 to 4095.

When the input reaches the upper range of the ADC, the reading cannot go higher than 4095 even if the input voltage increases further. This is called ADC saturation.

In Example 3, the maximum potentiometer position gave a raw reading of 4095 while `analogReadMilliVolts()` reported 3139 mV. This shows that a reading of 4095 means the ADC reached its maximum code and does not necessarily mean exactly 3.3 V.

## 3. Predicted vs. Observed Results

For the analog input, the ADC reading increased as the potentiometer was rotated.

For PWM, increasing the potentiometer reading increased the PWM duty cycle and made the LED brighter.

For the DAC, the measured voltages were close to the predicted values:

| DAC Code | Predicted | Measured |
| ---: | ---: | ---: |
| 0 | 0.00 V | 0.09 V |
| 64 | 0.83 V | 0.87 V |
| 128 | 1.66 V | 1.68 V |
| 192 | 2.48 V | 2.47 V |
| 255 | 3.30 V | 3.26 V |

The results were close to the expected values, with only small differences in the actual measurements.
