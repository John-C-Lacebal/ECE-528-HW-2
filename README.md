# ECE-528-HW-2
Submission for HW 2 for ECE 528

1. A hardware timer is a peripheral in embedded systems that is designed to measure time intervals without CPU involvement. Hardware timers are commonly used to generate PWM signals for motors or LEDs. They are also used to generate interrupts, timeouts, and delays.

2. The clock signal in embedded systems is used to synchronize the operations of digital circuits. The Main Clock (MCLK) is the only clock source used for the CPU core and core peripherals. The High-Speed System Clock (HSMCLK) drives the CPU (via MCLK), as well as other high-speed peripherals. The Sub-Main Clock (SMCLK) powers many peripherals including the UART, SPI, I2C, and timers. The Auxiliary Clock (ACLK) is used for low-power peripherals and is mainly used for wake-up and low-power operations. The Boot Clock (BCLK) is used for the boot ROM and early system initialization before the main clock is configured.

3. If Timer_A is configured in up mode, the timer counts up to the value in the compare register (TAxCCR0) before rolling over. Since the timer is 16-bits wide, the maximum value it can take is 0xFFFF.

4. 
Counter width = 24<br>
Clock frequency = 10 MHz = 10 x 10^6 Hz<br>
Max count = 10^24 - 1 = 16,777,215<br>
Clock cycle period = 1/(10 x 10^6 Hz) = 0.1 μs<br>
Overflow period = 16777215 x (0.1 x 10^-6) ≈ 1.68 s

The overflow period is about 1.68 seconds long.

5. The SysTick timer is a 24-bit down counter that can generate delays and periodic interrupts. The timer can be used for periodic LED blinking and PWM signal generation. The source for the timer can come from either the Main System Clock (MCLK) or the 32 kHz internal RC oscillator. In the SysTick Control and Status Register (STCTRL), the CLKSRC bit (bit 0) selects the clock source (0 for MCLK, 1 for oscillator). LATL

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
````c
    // Configure pin P2.4 (PM_TA0.1)to use peripheral function mode
    // set Bit 4 of the SEL0 register for P2
    // clear Bit 4 of the SEL1 register for P2
    P2->SEL0 |= 0x10;
    P2->SEL1 &= ~0x10;

    // Configure pin P2.4 as an output GPIO pin to drive the PWM signal
    // Set Bit 4 of the DIR register for P2
    P2->DIR |= 0x10;

    // Set the Timer A0 Capture/Compare register to a specified period_constant
    // CCR[0] is primarily used as the "period" register
    // General formula: Period = (2 * period_constant) / (12 MHz / Prescale Value)
    // In this case: Period = (2*60000) / (12 MHz / 2) = 20 ms
    int period_constant = 60000
    TIMER_A2->CCR[0] = period_constant;

    // Configure the Timer A0 expansion register to divide the clock frequency by 1
    // Clear all bits of the EX0 register
    TIMER_A2->EX0 = 0x0000;

    // Configure the output mode as Toggle / Reset for CCR[1]
    // Set the bits of the OUTMOD field of the CCTL[1] register to 010b
    TIMER_A0->CCTL[1] |= 0x0040;

    // Assign the value of duty_cycle_1 to the CCR[1] register
    // Duty Cycle %: duty_cycle_1 / period_constant
    // Since period_constant is 60000, to get a duty cycle of 50%, duty_cycle_1 = 30000
    int duty_cycle_1 = 30000;
    TIMER_A2->CCR[1] = duty_cycle_1;

    // Modify the following bits in the CTL register
    // Select SMCLK = 12 MHz as timer clock source
    // Set ID = 1 (Divide timer clock by 2)
    // Set MC = 3 (Up/Down Mode)
    TIMER_A0->CTL |= 0x0270;
````

12.
````c
    // Store the user-defined task function for use during interrupt handling
    Timer_A0_Task = task;

    // Halt Timer A0 by clearing the MC bits in the CTL register
    TIMER_A0->CTL &= ~0x0030;

    // In the CTL register, set the TASSEL and ID bits
    // Choose SMCLK as timer clock source (TASSEL = 10b)
    // Choose prescale value of 1 (ID = 0)
    TIMER_A0->CTL |= 0x0200;

    // Enable interrupt request of the
    // corresponding Capture/Compare interrupt flag
    // in the CCTL[0] register using the CCIE bit
    TIMER_A0->CCTL[0] |= 0x0010;

    // Store the period (2 ms) in the CCR0 register
    // Note: Timer starts counting from 0
    TIMER_A0->CCR[0] = 2;

    // Divide the SMCLK frequency by 1 by setting the
    // TAIDEX bits of the EX0 register
    TIMER_A0->EX0 = 0x0000;

    // Set interrupt priority level to 2 using the IPR2 register of NVIC
    // Timer A0 has an IRQ number of 8
    NVIC->IP[2] = (NVIC->IP[2] & 0xFFFFF00F) | 0x00000060;

    // Enable Interrupt 8 in NVIC by setting Bit 8 of the ISER register
    NVIC->ISER[0] |= 0x00000100;

    // Set the TACLR bit and enable Timer A0 in up mode using the MC bits in the CTL register
    TIMER_A0->CTL |= 0x0014;
````