# Project 1 – Switch-based LED & PWM Control  
This assembly-language project for the MSP430 microcontroller reads four switches (P1.0–P1.3) as a 4-bit binary value `SWstate`, and executes one of four operations:

## ✅ Functionality  
- If `SWstate` matches `01x0`, the eight LEDs on PORT2 display a binary count **upward**, from 0 to 0xFF, repeated continuously with a 1 second delay between increments.  
- If `SWstate` matches `02x0`, the LEDs display a binary count **downward**, from 0xFF to 0, with 1 second delay.  
- If `SWstate` matches `04x0`, the program generates a PWM signal on pin P1.7: frequency = 1 kHz, duty cycle = 75%. Timing is cycle-accurate (using the simulator’s counter-cycle and oscilloscope measurement).  
- For any other value of `SWstate`, turn off all LEDs and set P1.7 output to ‘0’.

## 🔧 Key Topics Covered  
- GPIO input (switches) and output (LEDs, PWM)  
- Reading switch states and decoding binary values  
- Looping, delays (cycle-counted) and repetition in assembly  
- PWM generation and measurement  
- Use of simulator tools (counter cycle) and real-hardware verification (oscilloscope)  
- Low-level assembly coding for the MSP430 (no RESET during runtime)

## 📁 Files in this folder  
- `main.asm` — entry point and main loop  
- `delay.asm` / `utils.asm` — delay routines and helper code  
- Additional modules (if any) for PWM, initialization, etc  
- `README.md` — this documentation

## 🧠 Usage Instructions  
1. Set up your MSP430 development board (e.g., MSP430G2553).  
2. Connect switches to P1.0–P1.3, LEDs to PORT2, PWM output to P1.7.  
3. Build and flash the code.  
4. Set the switches to one of the defined patterns and observe the behavior.  
5. Use an oscilloscope to verify PWM duty cycle and frequency when in the PWM mode.

## 📌 Notes  
- The RESET button must only be used for system start-up; subsequent resets during operation are disallowed per course requirements.  
- Timing is critical — ensure you measure delays and PWM parameters for correctness.  
- Documentation © Hanan Ribo (lab sheet origin).


# Project 2 – Interrupt-Driven FSM & Peripheral Control  
In this assembly project for the MSP430 microcontroller we implement a simple finite state machine (FSM) driven by interrupts from push-buttons (P2.0–P2.3). LEDs are connected to PORT1 and PWM/square-wave output to P2.7.

## ✅ Functionality  
- System starts in **idle/sleep** mode: LEDs off, MCU waiting for button interrupt.  
- On button 0 (state 1): binary count up on LEDs (PORT1) in 0.5 s steps, for a total of 10 seconds. Count resumes from where it left off on subsequent entries. This state is _atomic_ (does not exit until completion).  
- On button 1 (state 2): a single LED shifts left-to-right cyclically with 0.5 s delay, for 5 seconds. Also atomic.  
- On button 2 (state 3): generate a square-wave output on P2.7: 1 kHz for 5 s, then 2 kHz for 5 s, and repeat until any other button is pressed (non-atomic state).  
- On button 3 (state 4): execute an API routine `PrintArr(data, SIZE, dir)` on a data array, showing left- or right-printed bytes, with 1 second pause between prints.  
- If any other button press occurs (or state transitions), system returns to idle/sleep: turn off LEDs and wait.

## 🔧 Key Topics Covered  
- Interrupt setup and service routines (ISR) for button presses  
- FSM design and implementation in assembly: states, transitions, atomic vs non-atomic states  
- Layered code architecture: HAL (hardware abstraction layer), drivers, application logic  
- Delay routines, peripheral control, PWM/square-wave generation  
- Working with data arrays, API style routines (`PrintArr`), direction control  
- Use of cycle-accurate delays and real-hardware verification

## 📁 Files in this folder  
- `main.asm` — FSM initialization and main loop  
- `isr.asm` — interrupt vectors and handlers  
- `fsm.asm` / `drivers.asm` — FSM logic and hardware abstraction  
- `data.asm` — definition of `data[8] = { F764H, 5A83H, 79D2H, 6302H, 2394H, 2103H, 63CAH, 0345H }` and PrintArr routine  
- `README.md` — this documentation

## 🧠 Usage Instructions  
1. Connect buttons to P2.0–P2.3, LEDs to PORT1, and waveform output to P2.7.  
2. Build and flash the code to your MSP430 board.  
3. Press buttons to select states and observe LED behavior / waveform output on oscilloscope.  
4. Ensure correct timing (0.5 s delays, 5-10 s durations) and correct outputs.

## 📌 Notes  
- RESET button must only be used at start-up.  
- Please refer to the detailed FSM diagram in the lab report (drawn prior to code implementation).  
- Documentation © Hanan Ribo (lab sheet origin).
