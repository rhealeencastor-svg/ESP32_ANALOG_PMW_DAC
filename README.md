# Laboratory Activity 4: Analog Input, PWM, and DAC

**Name:** Rhea Leen Castor  
**Course:** BCA188 – Programming for Internet of Things  

## Objective
Run Examples 3–5 separately to examine analog input, PWM output, and true DAC output. The goal is to compare predicted values with recorded readings and observe the behavioral differences between simulated analog signals (PWM) and continuous analog voltage (DAC).

## Materials
* ESP32 Development Board (ESP32-WROOM)
* Micro-USB Data Cable
* 10 kΩ Potentiometer
* Breadboard and Jumper Wires
* 1x LED and 330 Ω Resistor
* Digital Multimeter

## Circuit Wiring
| Component | Pin / Terminal | ESP32 Connection | Notes |
| :--- | :--- | :--- | :--- |
| **Potentiometer** | Outer Terminal 1 | **3V3** | Power supply |
| | Middle Terminal (Wiper) | **GPIO 34** | ADC input (ADC_11db attenuation) |
| | Outer Terminal 2 | **GND** | Ground |
| **LED** | Anode (+) | **GPIO 19** | PWM output (LEDC) |
| | Cathode (-) | **GND** via 330 Ω resistor | Current limiter |
| **Test Point** | Multimeter Probe | **GPIO 25** | DAC Channel 1 (Pending testing) |

---

## Sketches and Results

### Example 3: Analog Input (`example3_analog_input.ino`)
Reads the potentiometer voltage using 12-bit ADC resolution (0–4095). 

**Observations:**
The recorded readings increased as the potentiometer moved toward its maximum position. Repeated readings at intermediate positions fluctuated slightly due to electrical noise. At maximum, the raw reading repeatedly reached the ceiling of 4095. The millivolt values are readings reported by `analogReadMilliVolts()`, not external multimeter measurements.

**Recorded Results:**
| Potentiometer Position | Raw ADC Reading | Reported Voltage (mV) |
| :--- | :--- | :--- |
| Minimum | 0 | 142 |
| About ¼ | 612 | 649 |
| Halfway | 1993 | 1727 |
| About ¾ | 3282 | 2718 |
| Maximum | 4095 | 3139 |

### Example 4: PWM LED Brightness (`example4_pwm.ino`)
Maps the 12-bit ADC reading to an 8-bit PWM duty cycle (0–255) using a 5 kHz frequency on GPIO 19.

**Observed Results:**
The LED successfully responded to the potentiometer. At the minimum knob position, the LED was completely off. As the knob was turned clockwise, the LED brightness gradually and smoothly increased until reaching peak brightness at the maximum position. 

**Recorded Software-Reported PWM Settings:**
*(Note: These are calculated predictions based on the code's mapping, not oscilloscope measurements)*

| Knob Position | Raw Input | PWM Setting | Calculated Duty (%) |
| :--- | :--- | :--- | :--- |
| Minimum | 0 | 0 | 0.0% |
| About ¼ | 1062 | 66 | 25.9% |
| Halfway | 2099 | 130 | 51.0% |
| About ¾ | 3105 | 193 | 75.7% |
| Maximum | 4095 | 255 | 100.0% |

### Example 5: DAC Output (`example5_dac.ino`)
*Status: Work In Progress 🚧*
Testing the internal Digital-to-Analog Converter to generate true analog voltages on GPIO 25. 

**Measurement Table (Pending):**
| Setting | DAC Code | Predicted Voltage (V) | Measured Voltage (V) |
| :---: | :---: | :---: | :---: |
| 1 | 0 | 0.00 V | *Pending* |
| 2 | 64 | 0.83 V | *Pending* |
| 3 | 128 | 1.66 V | *Pending* |
| 4 | 192 | 2.49 V | *Pending* |
| 5 | 255 | 3.30 V | *Pending* |

*(Note: PWM voltage measurements must not be recorded as DAC measurements.)*

---

## Discussion

**1. Why is PWM different from DAC output?**
PWM (Pulse Width Modulation) repeatedly switches a digital pin between LOW (0V) and HIGH (3.3V). The duty cycle controls the proportion of time spent HIGH, changing the *apparent* brightness of an LED through time-averaging. A DAC (Digital-to-Analog Converter) produces a true, continuous analog voltage level. On an oscilloscope, PWM is a square wave, while a DAC output is a flat, steady line.

**2. Why can the ADC endpoint saturate?**
The ESP32's ADC has a limited linear measurable input range. Even with `ADC_11db` attenuation, voltages near the 3.3V rail can exceed the internal limit. The ADC hits its highest code (4095) early, even if the physical voltage continues to rise. Therefore, a reading of 4095 means the input is at *or above* the saturation point, not necessarily exactly 3.3V.
