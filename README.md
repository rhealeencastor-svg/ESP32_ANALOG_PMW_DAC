# Laboratory Activity 4: Analog Input, PWM, and DAC

**Name:** Rhea Leen Castor  
**Course:** BCA188 – Programming for Internet of Things  

---

## Objective

Run Examples 3–5 separately using an ESP32-WROOM development board to examine analog input, PWM output, and true DAC output.

The activity aims to:

- Read an analog input from a potentiometer using the ESP32 ADC.
- Convert the ADC reading into an 8-bit PWM duty setting.
- Observe LED brightness using PWM output.
- Generate and measure true analog voltage using the ESP32 DAC.
- Compare predicted values with observed results.
- Explain the difference between PWM and DAC output.
- Explain why the ADC may saturate near its maximum input range.

---

# Example 3: Analog Input

## Materials Used

- ESP32-WROOM Development Board with USB Type-C connector
- USB Type-C Data Cable
- 10 kΩ Potentiometer
- Breadboard
- Jumper Wires

---

## Circuit Wiring

| Component | Terminal | ESP32 Connection | Purpose |
| :--- | :--- | :--- | :--- |
| Potentiometer | Outer Terminal 1 | 3V3 | Power supply |
| Potentiometer | Middle Terminal (Wiper) | GPIO 34 | ADC input |
| Potentiometer | Outer Terminal 2 | GND | Ground |

---

## Sketch

**File:** `example3_analog_input.ino`

The program reads the voltage from the potentiometer using the ESP32's analog-to-digital converter (ADC).

The ADC uses a 12-bit reading range, producing raw values from:

- **0** at the lower endpoint
- **4095** at the upper endpoint

The program also uses `analogReadMilliVolts()` to obtain an ESP32 software-reported voltage value in millivolts.

---

## Recorded Results

| Potentiometer Position | Raw ADC Reading | Reported Voltage (mV) | Screenshot Time |
| :--- | ---: | ---: | :--- |
| Minimum | 0 | 142 | 12:02:16 PM |
| About ¼ | 612 | 649 | 12:02:48 PM |
| Halfway | 1993 | 1727 | 12:03:17 PM |
| About ¾ | 3282 | 2718 | 12:03:29 PM |
| Maximum | 4095 | 3139 | 12:03:42 PM |

---

## Observations

The recorded ADC reading increased as the potentiometer was rotated from its minimum position toward its maximum position.

At intermediate knob positions, the values fluctuated slightly. This is expected because ADC readings can be affected by electrical noise, component tolerances, and the characteristics of the ESP32 ADC.

At the maximum potentiometer position, the raw ADC reading reached **4095**, which is the maximum value that can be represented by a 12-bit ADC reading.

The voltage values shown in the table were obtained using `analogReadMilliVolts()` and were therefore software-reported ESP32 ADC values rather than external multimeter measurements.

---

## Documentation

<!-- Add your Example 3 photos/screenshots here later. -->

**Circuit Setup:**  
[Insert image here]

**Serial Monitor – Minimum Position:**  
[Insert image here]

**Serial Monitor – ¼ Position:**  
[Insert image here]

**Serial Monitor – Halfway Position:**  
[Insert image here]

**Serial Monitor – ¾ Position:**  
[Insert image here]

**Serial Monitor – Maximum Position:**  
[Insert image here]

---

# Example 4: PWM LED Brightness

## Materials Used

- ESP32-WROOM Development Board with USB Type-C connector
- USB Type-C Data Cable
- 10 kΩ Potentiometer
- 1x LED
- 330 Ω Resistor
- Breadboard
- Jumper Wires

---

## Circuit Wiring

### Potentiometer

| Terminal | ESP32 Connection | Purpose |
| :--- | :--- | :--- |
| Outer Terminal 1 | 3V3 | Power supply |
| Middle Terminal (Wiper) | GPIO 34 | ADC input |
| Outer Terminal 2 | GND | Ground |

### LED

| Component | Connection | Purpose |
| :--- | :--- | :--- |
| GPIO 19 | 330 Ω resistor → LED | PWM output |
| LED Cathode (-) | GND | Ground |

The LED is driven by the PWM output from **GPIO 19**. The 330 Ω resistor limits the current through the LED.

---

## Sketch

**File:** `example4_pwm.ino`

The program reads the potentiometer using the ADC and converts the 12-bit raw ADC value from **0–4095** into an 8-bit PWM duty setting from **0–255**.

The PWM signal is produced on **GPIO 19**.

The approximate conversion can be represented as:

\[
PWM = \frac{Raw\ ADC}{4095} \times 255
\]

The PWM duty percentage is calculated using:

\[
Duty\ (\%) = \frac{PWM\ Setting}{255} \times 100
\]

---

## Recorded Results

| Knob Position | Raw Input | PWM Setting | Calculated Duty (%) |
| :--- | ---: | ---: | ---: |
| Minimum | 0 | 0 | 0.0% |
| About ¼ | 1062 | 66 | 25.9% |
| Halfway | 2099 | 130 | 51.0% |
| About ¾ | 3105 | 193 | 75.7% |
| Maximum | 4095 | 255 | 100.0% |

---

## Observations

The LED brightness changed as the potentiometer was rotated.

At the minimum potentiometer position, the PWM setting was **0**, corresponding to a **0% duty cycle**, and the LED was off.

As the potentiometer was rotated, the ADC reading increased and the PWM duty setting also increased. The LED therefore became progressively brighter.

At approximately the halfway position, the PWM setting was **130**, corresponding to approximately **51% duty cycle**.

At the maximum potentiometer position, the PWM value reached **255**, corresponding to **100% duty cycle**, and the LED reached its maximum brightness.

The PWM values shown in the table are software-generated PWM settings and are **not DAC voltage measurements**.

---

## Documentation

<!-- Add your Example 4 photos/screenshots here later. -->

**Circuit Setup:**  
[Insert image here]

**Minimum PWM / LED Off:**  
[Insert image here]

**¼ Position:**  
[Insert image here]

**Halfway Position:**  
[Insert image here]

**¾ Position:**  
[Insert image here]

**Maximum PWM / Maximum Brightness:**  
[Insert image here]

---

# Example 5: DAC Output

## Materials Used

- ESP32-WROOM Development Board with USB Type-C connector
- USB Type-C Data Cable
- Breadboard
- Jumper Wires
- Digital Multimeter

---

## Circuit Wiring

| Connection | ESP32 Pin | Purpose |
| :--- | :--- | :--- |
| Multimeter Positive Probe | GPIO 25 | DAC output voltage measurement |
| Multimeter Negative Probe | GND | Ground reference |

GPIO 25 is used as the ESP32's true DAC output.

The multimeter was set to measure **DC voltage**.

---

## Sketch

**File:** `example5_dac.ino`

The program outputs five DAC code values through **GPIO 25**.

The ESP32 DAC uses an 8-bit code range from:

- **0**
- to **255**

The expected DAC voltage can be estimated using:

\[
V_{DAC} \approx \frac{DAC\ Code}{255} \times 3.3V
\]

---

## DAC Measurement Table

| Setting | DAC Code | Predicted Voltage (V) | Measured Voltage (V) |
| :---: | ---: | ---: | ---: |
| 1 | 0 | 0.00 V | 0.09 V |
| 2 | 64 | 0.83 V | 0.87 V |
| 3 | 128 | 1.66 V | 1.68 V |
| 4 | 192 | 2.48 V | 2.47 V |
| 5 | 255 | 3.30 V | 3.26 V |

---

## Observations

The measured DAC voltage increased as the programmed DAC code increased.

At DAC code **0**, the multimeter measured approximately **0.09 V**.

At DAC code **128**, which is close to the midpoint of the 8-bit DAC range, the measured output was approximately **1.68 V**.

At the maximum DAC code of **255**, the measured output reached approximately **3.26 V**.

The measured values were close to the theoretical predictions, although small differences were present because the ESP32 DAC output and the actual supply voltage are not ideal precision voltage sources. Multimeter accuracy, wiring, and device tolerances can also contribute to small differences.

These measurements were taken directly from **GPIO 25 using a digital multimeter** and are true DAC voltage measurements. They are separate from the PWM values used in Example 4.

---

## Documentation

<!-- Add your Example 5 photos/screenshots here later. -->

**DAC Circuit / Multimeter Setup:**  
[Insert image here]

**DAC Code 0 – Measured 0.09 V:**  
[Insert image here]

**DAC Code 64 – Measured 0.87 V:**  
[Insert image here]

**DAC Code 128 – Measured 1.68 V:**  
[Insert image here]

**DAC Code 192 – Measured 2.47 V:**  
[Insert image here]

**DAC Code 255 – Measured 3.26 V:**  
[Insert image here]

---

# Discussion

## 1. Why is PWM different from DAC output?

PWM and DAC produce different types of electrical signals.

PWM, or Pulse Width Modulation, rapidly switches a digital output between approximately **0 V (LOW)** and **3.3 V (HIGH)**.

The duty cycle determines the percentage of time the signal remains HIGH during each cycle.

For example:

- 0% duty cycle means the output remains LOW.
- 50% duty cycle means the output is HIGH for approximately half of each cycle.
- 100% duty cycle means the output remains HIGH.

When PWM is used to control an LED, the rapid switching occurs too quickly for the human eye to distinguish each pulse. Instead, the LED appears dimmer or brighter depending on the duty cycle.

A DAC, or Digital-to-Analog Converter, behaves differently. Instead of rapidly switching between LOW and HIGH, it produces an approximately steady analog voltage corresponding to the selected digital code.

Therefore, PWM must not be reported as a DAC voltage measurement.

If viewed using an oscilloscope, the expected signals would be different:

- **GPIO 19 PWM:** square-wave pulse train
- **GPIO 25 DAC:** approximately steady DC voltage for a fixed DAC code

If an oscilloscope was not available during the experiment, these waveform differences were not directly measured.

---

## 2. Why can the ADC endpoint saturate?

The ESP32 ADC has a limited input measurement range.

A 12-bit ADC can represent digital codes from:

\[
0 \text{ to } 4095
\]

When the input voltage approaches or exceeds the upper measurable range of the ADC, the ADC cannot produce a value higher than **4095**.

This condition is called **saturation**.

During Example 3, the maximum potentiometer position produced a raw ADC reading of:

**4095**

while `analogReadMilliVolts()` reported approximately:

**3139 mV**

This demonstrates that a raw reading of 4095 represents the maximum ADC code and should not automatically be interpreted as exactly 3.3 V.

Once the ADC reaches its maximum code, further increases in input voltage cannot be represented by a higher digital reading.

---

## 3. Comparison of Predicted and Observed Results

### Analog Input

The ADC readings generally increased as the potentiometer was rotated from minimum to maximum.

The raw values ranged from **0 to 4095**, as expected for the configured 12-bit ADC resolution.

Small fluctuations were observed at intermediate positions because real ADC measurements can be affected by noise and hardware characteristics.

The maximum reading reached **4095**, demonstrating ADC saturation at the upper endpoint.

### PWM Output

The raw ADC input was successfully converted into an 8-bit PWM setting from **0 to 255**.

The observed LED brightness followed the PWM duty setting:

- Low PWM duty produced low brightness.
- Medium PWM duty produced medium brightness.
- High PWM duty produced high brightness.
- A PWM setting of 0 turned the LED off.
- A PWM setting of 255 produced maximum brightness.

The PWM values were duty-cycle settings rather than analog voltage measurements.

### DAC Output

The DAC output voltage increased as the programmed DAC code increased.

The measured voltages were:

- **0.09 V**
- **0.87 V**
- **1.68 V**
- **2.47 V**
- **3.26 V**

These values closely followed the predicted values of:

- **0.00 V**
- **0.83 V**
- **1.66 V**
- **2.48 V**
- **3.30 V**

Small differences between the theoretical and measured voltages can be caused by the actual ESP32 supply voltage, DAC characteristics, multimeter accuracy, wiring resistance, and normal hardware tolerances.

Overall, the observed behavior agreed with the expected operation of the ESP32 ADC, PWM, and DAC.

---

# Summary

Laboratory Activity 4 demonstrated three different methods of working with analog and analog-like signals using the ESP32-WROOM.

**Example 3** demonstrated analog input using the ADC and a potentiometer.

**Example 4** demonstrated PWM output by converting the potentiometer reading into an 8-bit PWM duty cycle that controlled LED brightness.

**Example 5** demonstrated the ESP32's true DAC output on GPIO 25, with the output voltage measured using a digital multimeter.

The activity showed that PWM and DAC are fundamentally different. PWM rapidly switches a digital output between LOW and HIGH, while the DAC produces an approximately steady analog voltage corresponding to a digital code.

The experimental measurements generally agreed with the predicted values, with small differences caused by normal real-world hardware characteristics.
