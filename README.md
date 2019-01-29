# USB-PD-Lipo-Charger-Hardware

https://hackaday.io/project/161771-usb-power-delivery-lipo-battery-charger

This project uses USB Type C with Power Delivery to charge Lithium Polymer batteries. It supports charging 1s-4s batteries and supports balancing for 2s-4s packs. The device supports charging up to 100W (16.8V at 6A).

I got the idea for this project while on a vacation where I only took a single 65W USB C power supply from my Thinkpad to charge the laptop, Nintendo Switch, and cell phone. Having worked with hobby style lithium polymer batteries and bulky chargers, I thought it would be nice to have a small device that could charge LiPo batteries from a USB C power supply.

Input is a USB Type C Port. Charging is done through an XT60 connector and has JST connectors for balancing 2s-4s packs.

The device will be pre-programmed with a fixed max current limit up to 6A. Everything runs automatically and will charge up to the max capability of the connected USB PD power supply if the max current output limit exceeds the input power supply.

The only user feedback is through an RGB LED.

A STM32G0 microcontroller controls runs the system and handles USB PD communication. TI makes the perfect programmable buck boost regulator for this project with the BQ25703A. It accepts input voltage from 3.5V to 24V and the output is meant to charge lithium batteries up to 4s with a current limit of 6.35A. It is programmed through I2C.

A 3.3V linear regulator was needed to provide power for the electronics. The hardest part is that it needs to accept up to 20V on the input due to the power delivery specifications. The IFX25001 from Infineon accepts up to 45V and is a good fit for the project.

I chose to have balance ports for 2s-4s packs for balancing with charging done through an XT60 connector. Originally I was going to support charging through the balance ports, but the typical gauge of wire used for those connections would severely limit the charging current. I also had to design a circuit to automatically route the output from the regulator to the top cell depending on what type of pack was connected. I designed the circuit and thought it was pretty clever, but ultimately, made the decision to reduce complexity and support higher charging current by going with an XT60 connector.

Balancing is controlled by the STM32G0. It monitors the cell voltages using its ADC through voltage dividers to scale the voltage to safe levels. Open drain level shifters were needed to control the PFETs used for discharging. Because of this, it create a large VGS on the PFETs, up to the max of a 4s pack ~16.8V. I needed to find PFETs that could handle 20V VGS. 
