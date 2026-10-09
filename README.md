# ECE-528-HW-2
Submission for HW 2 for ECE 528

1. A hardware timer is a peripheral in embedded systems that is designed to measure time intervals without CPU involvement. Hardware timers are commonly used to generate PWM signals for motors or LEDs. They are also used to generate interrupts, timeouts, and delays.

2. The clock signal in embedded systems is used to synchronize the operations of digital circuits. The Main Clock (MCLK) is the only clock source used for the CPU core and core peripherals. The High-Speed System Clock (HSMCLK) drives the CPU (via MCLK), as well as other high-speed peripherals. The Sub-Main Clock (SMCLK) powers many peripherals including the UART, SPI, I2C, and timers. The Auxiliary Clock (ACLK) is used for low-power peripherals and is mainly used for wake-up and low-power operations. The Boot Clock (BCLK) is used for the boot ROM and early system initialization before the main clock is configured.

3. If Timer_A is configured in up mode, the timer counts up to the value in the compare register (TAxCCR0) before rolling over.

4. 
Counter width = 24<br>
Clock frequency = 10 MHz = 10 x 10^6 Hz<br>
Max count = 10^24 - 1 = 16,777,215<br>
Clock cycle period = 1/(10 x 10^6 Hz) = 0.1 μs<br>
Overflow period = 16777215 x (0.1 x 10^-6) ≈ 1.68 s

The overflow period is about 1.68 seconds long.

5. The SysTick timer is 

6. The SysTick timer on the MSP432P401R
microcontroller has four 32-bit registers. The SysTick Control and Status Register (STCSR) enables the SysTick timer and other features like the SysTick exception requests. The SysTick Reload Value Register (STRVR) sets the starting count value for the SysTick timer to count down from. The SysTick Current Value Register (STCVR) reads the current count value of the Systick timer, and can be written into to clear the current value. Finally, the SysTick Clock Source Register (STCSR) selects the clock source for the SysTick timer.

7. The Timer_A peripheral is a 16-bit counter with up to 7 capture/compare registers. The Stop Mode halts the timer. The Up Mode repeatedly counts from 0 up to the value in the TAxCCR0 register. The Continuous Mode repeatedly counts from 0 to 0xFFFF. The Up/Down Mode repeatedly counts from 0 up to the value in the TAxCCR0 register and back down to 0.

8.

* __Timer_Ax Control Register (TAxCTL):__ Used to configure general operation of the Timer_A peripheral

* __Timer_Ax Capture/Compare Control N Register (TAxCCTLn):__ Controls the behavior of each individual capture/compare channel block in Timer_A (Input capture, Output compare, Interrupt generation)

* __Timer_Ax Counter Register (TAxR):__ Holds current count value of Timer_A

* __Timer_Ax Capture/Compare N Register (TAxCCRn):__ Used for compare and capture modes. In compare mode, the value within TAxR is compared against this register's value. In capture mode, the value within TAxR is copied to this register.

* __Timer_Ax Interrupt Vector Register (TAxIV):__ Used for Timer_A interrupts, and notifies the user what specific interrupt went off.

* __Timer_Ax Expansion 0 Register (TAxEX0):__ Allows for further division of the selected clock source's frequency.

9. Pulse Wave Modulation (PWM) is a technique that uses pulse widths to encode or modulate a signal. It generates a square wave with a variable duty cycle by rapidly switching between HIGH and LOW inputs, which regulates how much power is delivered to a load. PWM is used to control LED brightness, DC motor speed, and servo motor position.

10. There are 4 Timer_A modules available (TA0-TA3), and each module provides up to 4 PWM outputs.

11.

12.