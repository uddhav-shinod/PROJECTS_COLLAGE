# Working Principle – 555 Timer Based PWM DC Motor Controller

## 1. Introduction

This project uses an NE555 timer IC to generate a Pulse Width Modulation (PWM) signal for controlling a DC motor.

The speed of the motor is controlled by varying the duty cycle of the PWM signal. An NMOS transistor acts as an electronic switch, controlling the power delivered to the motor.

The circuit uses a 12 V DC supply and was developed by modifying an existing circuit design. LTspice is used to study its operation and analyse the circuit behaviour.

## 2. Pulse Width Modulation (PWM)

Pulse Width Modulation is a technique used to control the average power delivered to an electrical load by varying the ON and OFF durations of a periodic signal.

A PWM signal consists of two intervals:

- **ON time (Ton):** Duration for which the signal remains HIGH.
- **OFF time (Toff):** Duration for which the signal remains LOW.

The total time period is:

\[
T = T_{ON}+T_{OFF}
\]

The frequency is:

\[
f=\frac{1}{T}
\]

The duty cycle is the percentage of one time period during which the signal remains HIGH.

\[
D=\frac{T_{ON}}{T_{ON}+T_{OFF}}\times100
\]

A higher duty cycle generally results in a higher average voltage applied to the motor, while a lower duty cycle reduces the average voltage.

For an ideal switching circuit:

\[
V_{AVG}\approx D\times V_{S}
\]

where:

- \(V_{AVG}\) = Average voltage applied to the motor
- \(D\) = Duty cycle expressed as a fraction
- \(V_S\) = DC supply voltage

This is an ideal approximation. Actual motor voltage and speed also depend on switching behaviour, motor current, back EMF and mechanical load.

## 3. PWM Generation Using the NE555 Timer

The NE555 timer IC is used to generate the PWM signal.

In this circuit, the timing capacitor is connected to the trigger and threshold pins of the timer. The capacitor charges and discharges through a resistor and potentiometer network.

The NE555 internally monitors the capacitor voltage using two threshold levels:

- Approximately one-third of the supply voltage.
- Approximately two-thirds of the supply voltage.

When the timing capacitor voltage reaches the upper threshold, the timer changes its output state and initiates the discharge interval.

When the capacitor voltage falls to the lower threshold, the output changes state again and initiates the charging interval.

This repeated charging and discharging process generates a periodic output waveform.

## 4. Duty-Cycle Adjustment

A 47 kΩ potentiometer and two diodes are included in the timing network.

The diode arrangement provides different current paths for charging and discharging the timing capacitor.

Adjusting the potentiometer changes the effective resistance in these paths. Consequently, the charging and discharging durations change.

This allows the duty cycle of the generated PWM signal to be adjusted.

The exact PWM frequency and duty-cycle range depend on the effective resistance values, timing capacitance and circuit behaviour. These values should be measured from the LTspice waveforms rather than assumed.

## 5. MOSFET Switching Stage

The PWM output of the NE555 is connected to the gate of an NMOS transistor through a 100 Ω resistor.

The NMOS transistor acts as a low-side electronic switch.

Its operation can be divided into two states:

### State 1: MOSFET ON

When the PWM signal is HIGH, the gate voltage turns the NMOS transistor ON, provided the gate voltage is sufficient for the selected device.

Current flows through the motor and the MOSFET to ground.

The motor receives power and produces torque.

### State 2: MOSFET OFF

When the PWM signal is LOW, the NMOS transistor turns OFF.

The main motor current path through the MOSFET is interrupted.

Since the motor is an inductive load, its current cannot change instantaneously. A suitable flyback path is therefore needed to allow the stored inductive energy to circulate safely.

The circuit includes a diode connected across the motor switching terminals for this purpose. Its orientation and current path should be verified against the schematic.

A 10 kΩ resistor is connected between the MOSFET gate and ground. It acts as a gate pull-down resistor, helping keep the MOSFET OFF when the timer output is not driving the gate.

## 6. DC Motor Speed Control

The motor receives a sequence of ON and OFF voltage pulses.

The mechanical response of the motor depends on the average electrical power delivered over time.

When the duty cycle increases:

- The MOSFET remains ON for a larger proportion of each PWM period.
- The average voltage applied to the motor generally increases.
- Motor current and torque may increase.
- Motor speed generally increases under comparable operating conditions.

When the duty cycle decreases:

- The MOSFET ON duration becomes shorter.
- The average voltage applied to the motor generally decreases.
- The available torque and motor speed may decrease.

The relationship between duty cycle and motor speed is not perfectly linear because of motor resistance, inductance, back EMF and mechanical loading.

## 7. Components and Their Functions

| Component | Function |
|---|---|
| NE555 timer | Generates the PWM signal |
| 47 kΩ potentiometer | Adjusts the timing resistance |
| D1 and D2 | Provide separate charging and discharging paths |
| 100 nF capacitor | Timing capacitor |
| NMOS transistor | Switches motor current |
| 100 Ω resistor | Limits the transient gate-charging current |
| 10 kΩ resistor | Pulls the MOSFET gate towards ground |
| Flyback diode | Provides an inductive current path |
| 12 V supply | Supplies the motor and timer circuit |
| 10 µF capacitor | Supply-rail decoupling |
| 100 nF capacitor | Connected to the timer control-voltage pin |
| 470 Ω resistor | Connected between the supply rail and switched motor node; its intended role should be confirmed from the original design |

## 8. Simulation in LTspice

The circuit is simulated using LTspice transient analysis.

The simulation can be used to examine:

1. PWM output from the NE555 timer.
2. MOSFET gate voltage.
3. Motor terminal voltage.
4. Motor current.
5. Motor speed response, if available from the motor model.

The potentiometer setting can be changed to compare the resulting PWM waveforms and motor behaviour.

The transient simulation directive in the current schematic is:

\[
\texttt{.tran 10}
\]

This specifies a transient simulation duration of 10 seconds. It does not indicate a PWM frequency of 10 Hz.

## 9. Conclusion

This project demonstrates the principle of PWM-based DC motor control using an NE555 timer and an NMOS switching stage.

The timer generates a periodic PWM signal, while the potentiometer and diode network allow the duty cycle to be adjusted. The MOSFET switches the motor current according to this signal, controlling the average power delivered to the motor.

Through this project, the operation of PWM generation, timing networks, semiconductor switching and basic DC motor control can be studied.

The simulation results should be analysed and compared with practical measurements before concluding that the circuit is fully validated.