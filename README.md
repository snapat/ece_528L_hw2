1. A hardware timer is a timer peripheral that is used similar to a stopwatch, in an MCU. It is used to an able to keep track of time, independent of the CPU.

2.  A clock signal is used to synchronize the timing of different components. It can also be used to increment a timer to keep track of time.
LFXTCLK: Low-frequency oscillator clock that operates in the 32-kHz range or less.
HFXTCLK: High-frequency oscillator clock that operates in the 1-MHz to 48-MHz range.
DCOCLK: Internal digitally controlled oscillator with programmable frequencies.
VLOCLK: Internal very-low-power low-frequency oscillator with 9.4-kHz typical frequency
REFOCLK: Internal very-low-power low-frequency oscillator with selectable 32.768-kHz or 128-kHz typical frequencies
MODCLK: Internal low-power oscillator with 25-MHz typical frequency
SYSOSC: Internal oscillator with 5-MHz typical frequency

3. 2^16 (possible values) - 1 (accounting for 0) = 65535

4. Period = Seconds / (Wrap-around Value)
Wrap-around Value = (2^24 - 1)
Period = 1 / Frequency
Overflow Value = Seconds * Frequency
Seconds = (2^24) / (10^7) = 1.6777216

5. A 24-bit counter that counts down, and resets to the highest value after reaching zero, that resets to zero if any write accesses it. It can be used as a countdown timer for something to happen, like a routine that happens every 100Hz or as an alarm.
The SysTick timer can only be clocked with respect to a reference clock, which is the CPU free running clock in the MSP432P4xx.(2.2.1 Notes Section). It can be enabled via CLKSOURCE bit in STCSR register.

6. The timer consists of three registers:
Control and Status register configures clock, enables counter, enables SysTick interrupt,  and determines counter status.
Reload Value register provides the counter's wrap value.  
Current Value register contains the current value of the counter.

7. Timer_A Operating Modes:
00 Stop: Timer halted
01 Up: Counts from 0 to TAxCCR0
10 Continuous: 0 to 0FFFFh
11 Up/Down: Counts from 0 to TAxCCR0 and back down to 0

8. TAxCTL: Clock source select, input divider, mode control, interrupt control, clear
  TAxCCTLn: Capture/Compare Control Settings Registers 0 through 6
  TAxR: Count register of Timer A
  TAxCCRn: Set to capture or control mode
  TAxIV: Interrupt Vector, shows interrupt source and flag   
  TAxEx0: Select divider for input clock

9. PWM is pulse width modulation, a way to modulate square wave that has a set amplitude and frequency. It is augmented by a duty cycle, which controls the amount of time a square wave is on per period; at 100% the square wave becomes its constant maximum amplitude. It can be used as a clock, as a way to control average power, and control LEDs and motors.

10. There are 7 modules available. Each output mode has 8 operating modes,and each capture/compare block has it's own output.

11.
#define OUTMOD_BITS 5
#define PERIOD_CONSTANT 60000

void Timer_A0_PWM_Init () 
{
  P2->SEL0 |= 0x10;
  P2->SEL1 &= ~(0x10); //Set to secondary mode

  P2->DIR |= 0x10; //Configured to output

  TIMER_A0->CCR[1] = PERIOD_CONSTANT; // set Period = (2*60000) / (12 MHz / 2) = 20 ms

  TIMER_A0->EX0 = 0b00; //clear all bits of expnasion register

  TIMER_A0->CCTL[1] |= (0b010 << OUTMOD_BITS); // toggle/reset mode

  TIMER_A0 -> CTL |= 0x0270; //settings

  TIMER_A0 -> CCR[1] = PERIOD_CONSTANT/2; //change duty cycle


}