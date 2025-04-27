# 555 Timer Astable Multivibrator Project

## Overview

This repository documents a simple yet versatile electronic circuit built using the classic NE555 timer IC. The circuit is configured as an **astable multivibrator**, meaning it generates a continuous oscillating square wave signal at its output (Pin 3) without needing an external trigger.

This implementation includes two potentiometers, allowing for adjustable control over the output waveform's **frequency** and **duty cycle**.

![20250427_235338](https://github.com/user-attachments/assets/b7386917-a634-4cf4-860a-6624ba7f2902)


![20250427_234944](https://github.com/user-attachments/assets/2f598003-23ea-446f-9c78-67782b4e78d3)


![20250427_234947](https://github.com/user-attachments/assets/7f93ca44-d45c-4a2f-91ef-05a00e83e82b)


## How it Works

The 555 timer in astable mode operates by repeatedly charging and discharging an external capacitor (`C1` in this circuit) between two threshold levels (1/3 Vcc and 2/3 Vcc).

1.  **Charging Phase:** The capacitor `C1` (10nF connected to Pins 6/2 and Ground) charges towards Vcc through two resistances: `Pot_RA` (the potentiometer between Vcc and Pin 7) and `Pot_RB` (the potentiometer between Pin 7 and Pin 6). During this phase, the output (Pin 3) is HIGH.
2.  **Discharging Phase:** When the voltage across `C1` reaches 2/3 Vcc (detected by Pin 6 - Threshold), the internal flip-flop changes state. The output (Pin 3) goes LOW, and the internal discharge transistor (connected to Pin 7) turns ON, effectively grounding Pin 7. The capacitor `C1` now discharges through `Pot_RB` only towards ground.
3.  **Cycle Repeat:** When the voltage across `C1` drops to 1/3 Vcc (detected by Pin 2 - Trigger), the internal flip-flop resets. The output (Pin 3) goes HIGH again, the discharge transistor turns OFF, and `C1` begins charging again through `Pot_RA` and `Pot_RB`. This cycle repeats continuously.

**Key Components for Adjustment:**

*   **`Pot_RA` (Potentiometer Vcc <-> Pin 7):** This potentiometer primarily controls the **charging time** (`T_high`) of the capacitor `C1`. Adjusting `Pot_RA` significantly impacts both the frequency and the duty cycle. Increasing its resistance increases `T_high`, which lowers the frequency and increases the duty cycle. *(Note: In the provided schematic, this is labeled "RA")*.
*   **`Pot_RB` (Potentiometer Pin 7 <-> C1 ):** This potentiometer affects **both the charging time (`T_high`) and the discharging time (`T_low`)** because the capacitor charges through `Pot_RA + Pot_RB` but discharges only through `Pot_RB`. Adjusting `Pot_RB` changes both `T_high` and `T_low`, thus changing the frequency. It also influences the duty cycle, but its primary effect shared across both phases makes frequency the main variable controlled. *(Note: In the provided schematic, this is labeled "Pot")*.
*   **`C1` (Timing Capacitor Pin 6/2 <-> GND):** This capacitor (10nF in this build) is **fundamental to the oscillation frequency**. The time it takes to charge and discharge determines the period of the waveform.
    *   **Frequency is inversely proportional to the capacitance:** `f ≈ 1.44 / ((R_A + 2 * R_B) * C1)`
    *   Where `R_A` is the resistance set on `Pot_RA` and `R_B` is the resistance set on `Pot_RB`.
    *   To change the *range* of frequencies achievable, replace `C1` with a different value (larger C = lower frequency, smaller C = higher frequency).

**Other Components:**

*   **`C2` (Capacitor Pin 5 <-> GND):** The 10nF capacitor on Pin 5 (Control Voltage) is used for noise decoupling on the internal voltage divider, improving stability. Standard practice recommends 0.01µF (10nF).
*   **Pin 4 (Reset):** Tied to Vcc to ensure the timer is always enabled and oscillating.
*   **Pin 8 (Vcc) & Pin 1 (GND):** Power supply connections.

## Components List

*   1 x NE555 Timer IC (or equivalent, e.g., LM555, TLC555)
*   1 x Potentiometer (e.g., 10kΩ - 100kΩ for `Pot_RA`)
*   1 x Potentiometer (e.g., 10kΩ - 100kΩ for `Pot_RB`)
*   1 x Capacitor, 10nF (0.01µF), Ceramic or Film (`C1` - Timing)
*   1 x Capacitor, 10nF (0.01µF), Ceramic (`C2` - Control Voltage)
*   Hook-up Wires
*   Solderless Breadboard
*   DC Power Supply (5V to 15V, depending on 555 variant)

*(Note: The exact values of the potentiometers are 10kΩ ; typical values range from 1kΩ to 1MΩ depending on the desired frequency range.)*

## Observed Results

The circuit was powered and the output at Pin 3 was measured using a Hantek DSO2D15 oscilloscope. With the potentiometers set as shown in the breadboard image, the following waveform characteristics were observed:

*   **Frequency:** ~446.43 kHz
*   **Duty Cycle:** ~41.96 %
*   **Peak-to-Peak Voltage:** ~6V (Suggesting Vcc was likely around 6-7V)
*   **Waveform:** Rectangular pulse train, showing the characteristic shape of a 555 astable output.

*Note: The relatively high frequency observed suggests that the resistance values set on the potentiometers were quite low at the time of measurement.*

## How to Use and Adjust

1.  Assemble the circuit on a breadboard according to the schematic.
2.  Connect a DC power supply (ensure voltage is within the 555's operating range, typically 4.5V to 16V for NE555) to the Vcc and GND rails.
3.  Connect an oscilloscope probe to Pin 3 (Output) and GND.
4.  **Adjust `Pot_RA`** to change the frequency and duty cycle (primarily affects the high time).
5.  **Adjust `Pot_RB`** to change the frequency and duty cycle (affects both high and low times).
6.  To shift the entire frequency range, **replace `C1`** with a capacitor of a different value.

## Potential Improvements / Further Exploration

*   Add fixed resistors in series with the potentiometers to prevent setting resistance to 0Ω, which could lead to excessive current or unexpected behavior.
*   For duty cycles closer to 50% or less than 50%, add a signal diode (e.g., 1N4148) in parallel with `Pot_RB` (cathode towards Pin 7, anode towards Pin 6). This bypasses `Pot_RB` during charging, making `T_high` dependent only on `Pot_RA` and `T_low` dependent only on `Pot_RB`.
*   Add an LED with a current-limiting resistor to the output (Pin 3) for visual indication.
*   Buffer the output using a transistor or logic gate if driving heavier loads.

