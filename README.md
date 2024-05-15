# ADCModule
This project included Code Composer Studio, a MSP430 Microcontroller, a Multimeter, a Potentiometer, a RGB LED, and a 16x2 LCD Display. The main purpose of this project was to understand the how the potentiometer sends it reading to the ADC module, and how that relates to what is shown on the Multimeter. 

![image](https://github.com/k-parikh/ADCModule/assets/105128826/82b4d081-f2b1-41fc-a64a-2e58c04e34ae)

The figure above shows the connections that are used to control all the parts in this project. 

  * P3.0 - 3.7 and 8.0 - 8.2: LCD Display
  * P6.0 - 6.2: RGB LED Light
  * P4.1: Potentiometer

The first step is to enable all the inputs and outputs. 

```
P3DIR |= 0xFF;
P3OUT &= ~0xFF;
P8DIR |= 0x0E;
P8OUT &= ~0x0E;
P6DIR |= 0x07;
P4SEL1 |= BIT1;
P4SEL0 |= BIT1;
```

The second step is to initialize the ADC module.

```
ADC12CTL0 = ADC12SHT0_6 | ADC12ON;
ADC12CTL1 = ADC12SHP;
ADC12CTL2 = ADC12RES_2;
ADC12MCTL0 = ADC12INCH_9;
ADC12IER0 |= ADC12IE0;
```

`unsigned long voltage = ((((unsigned long)adc_raw) * 3.300) / 4095) * 1000;`

We have to declare and initialize the line able so that we can get the voltage value in Volts. It is also so that we can show the voltage value on the LCD display. 

![image](https://github.com/k-parikh/ADCModule/assets/105128826/b4abee98-2ff2-4b65-bb03-4f8c183dd2f8)

Lastly, set the conditions to turn on the correct LED color based on the voltage value. 

```
if(voltage < 1000){
  P6OUT = ~0x00;
}
if((voltage >= 1000) && (voltage < 2000)){
  P6OUT = ~0x04;
}
if((voltage >= 2000) && (voltage < 3000)){
  P6OUT = ~0x02;
}
if(voltage >= 3000){
  P6OUT = ~0x01;
}
```

The numbers in the conditional statements represent the voltage values as they relate to the the voltage variable. 
