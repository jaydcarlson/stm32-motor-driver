# stm32-motor-driver
Altium PCB files and STM32CubeIDE project to implement a basic brushed DC motor controller.

## Demo Usage
The STM32 will respond to commands sent on the UART pins at a baud of 115,200. You can use [PuTTY](https://www.putty.org/) or some other serial terminal to communicate with the board using a USB-to-serial converter.

The board will announce itself on boot-up; if you don't see `STM32 Motor Driver, v1.0` printed on your console, check your TX/RX polarity, your baud, and that you are connected to the correct COM port.

The only command the interface currently supports is the "goto" command. Invoke it as `g [setpoint]`. The board will respond by announcing the setpoint and announcing when it is reached.

Examples:
```
g 1000
GO 1000...Reached

g -500
GO -500...Reached
```

## Pin Mapping
* PA10 and PA8 are wired as separate PWM channels (TIM1_CH3 and TIM1_CH1) for the INA and INB signals, respectively.
* PA9 is routed to the "PWM" pin — if you want to recirculate your current, keep this pin high and instead apply your PWM to the INA or INB signals. Otherwise, enable TIM1_CH2 and apply your PWM duty cycle appropriately.
* PA15 and PB3 correspond to the A and B quadrature encoder inputs, which routes them to TIM2_CH1 and TIM2_CH2 running in Encoder mode.
* PA0 (ADC1_IN0) can be used to monitor the current. The board uses a 1k CS resistor, which translates to roughly 3.696 amp at full-scale 3.3V (0.8928 V/A).
* SEL0 is routed to PA11; this can be used to determine which path inside the H-Bridge to measure current from. Consult the VNH7100 datasheet for more information.
