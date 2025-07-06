STM32 Bare-Metal PWM LED Dimming (TIM3 on PA6)

This project demonstrates how to generate a PWM signal using TIM3 on the STM32F401RE to control LED brightness with a fade-in/fade-out effect. The LED is connected to PA6 (TIM3_CH1), and brightness is adjusted by varying the PWM duty cycle.

No HAL libraries are used — this is a pure bare-metal (CMSIS) implementation.

What I Built:

- Configured TIM3 in PWM Mode 1 on Channel 1 (PA6)
- Set up GPIOA pin 6 as Alternate Function 2
- Used a timer to generate a 1 kHz PWM signal
- Gradually updated the CCR1 (compare value) to control brightness
- LED fades in and out smoothly and quickly
- Fully hardware-driven — no blocking delays

What I Learned:

- How to set up TIM3 for PWM generation using ARR, PSC, CCR
- How duty cycle controls LED brightness
- How to configure GPIO for alternate function (AF2 for PA6)
- Timing behavior of different step sizes and delay values
- How to fade an LED efficiently using bare-metal programming

Key Concepts Implemented:

- PWM Mode 1 using TIM3
- GPIO Alternate Function configuration
- Duty cycle control via TIM3->CCR1
- Hardware-level LED dimming
- Fast fade effect using:
  - Step size: ±10
  - Delay loop: ~2000 cycles

Hardware and Tools:

Board: STM32 Nucleo-F401RE  
Pin: PA6 (TIM3 Channel 1)  
LED: Any standard LED + 220Ω resistor  
IDE: Keil uVision5 or STM32CubeIDE  
Debugger: On-board ST-Link  
Language: C (CMSIS only, no HAL)

Project Structure:

main.c – Core PWM and GPIO setup  
stm32f4xx.h – CMSIS header  
startup_stm32f401xe.s – Default startup (no interrupts used here)

How It Works:

PWM Setup: 1 kHz signal
TIM3->PSC = 16 - 1        // Timer input clock: 1 MHz  
TIM3->ARR = 1000 - 1      // Auto-reload = 1000 → 1 kHz PWM

Fade logic
uint16_t duty = 0  
int8_t step = 10

while (1) {
    TIM3->CCR1 = duty;
    for (volatile int i = 0; i < 2000; i++);
    duty += step;
    if (duty >= 1000 || duty <= 0) step = -step;
}

Output:

- LED brightness increases and decreases smoothly on PA6
- Complete fade cycle takes ~200 ms
- Fast response with clean transitions — no flicker
