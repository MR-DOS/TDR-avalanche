# TDR-avalanche
----
## Description
Simple flat-top avalanche pulser with integrated power supply for the avalanche transistor circuit and trigger circuitry. 

----
## Overview
This pulser was made in hope to use it to measure reflections on cables. However, using the transistor 2SC5773, it was not possible to obtain faster risetime than about 700 ps. This allows to measure reflections, but the resolution is quite limited due to the shape of the pulse (slow risetime and a backlash after the falling edge). Maybe using some other transistor and changing the compensation circuitry, it could be made better. Since there are many PECL buffers with rise and fall times faster than 200 ps (and reproducible), the next one will use some IC to generate the pulses. Also, generating short rectangular pulse is not very good if you want to postprocess the data, because you would need to run a decorrelation to find out what is happening on the cable. Using a long rectangular pulse (with infinitesimally fast edges) where all of the reflected energy comes back before the falling edge allows you to derive the signal which in turn gives you impulse response.

----
## Features
### Power supplies
The pulser contains a 40V power supply followed by denoising network and 6V - 35V regulated linear supply to set the avalanche voltage. The whole board is powered from 5V using Mini USB-B, consuming less than 100 mA.

### Avalanche pulser
The avalanche pulser uses 2SC5773 transistor (6V NPN, fT=10.8 GHz @ 50 mA). The compensation circuitry makes the output pulse a nice clean rectangular pulse with a flat top with no visible ringing (invisible with 1 GHz oscilloscope, to be precise), however there is a not very nice backlash after the falling edge (see pictures). The transistor does not self-avalanche when set properly, it has to be triggered by external source. I used 8-10 kHz triggering during the development and use. There are 2 SMA connectors, one for coaxial cable where energy for the pulse is stored (length of the cable determines length of the pulse) and one where the pulses are output.

### Trigger circuitry
The trigger circuitry triggers avalanche on rising edge of the input trigger signal. There is RC circuit which sets the time between the trigger and avalanche. However, it seems to have quite large jitter. There is also trigger output which you can connect to scilloscope (the avalanche happens on falling edge of the TRIG OUT). The trigger circuitry can be inhibited using the jumper JP1 when tuning the avalanche voltage.

### Attenuators
There are 2 attenuator sections giving 11.3 and 7.9 dB attenuation. The first one is mandatory as it is an impedance matching circuit. The second one is just a normal attenuator and can be omitted.

----
## How to use it as a simple standalone time-domain reflectometer
Connect square wave generator to the SYNC\_IN port. Connect an oscilloscope to the SYNC\_OUT port and to the PULSE\_OUT port.
Connect the coaxial cable to be measured to the STORAGE_COAX port. The cable MUST NOT have any shorts in it or any low impedance termination, because it is connected to the collector network and it would load the 200k resistor and the transistor would not avalanche. This severely limits the useability, so this mode is not recommended.

----
## How to use it as a pulser for general use
Connect square wave generator to the SYNC\_IN port.
Connect storage coaxial cable to the STORAGE_COAX port. You should use some 50 Ohm cable with low losses, ideally some rigid or semirigid cable. I used RG-174 because I had a lot of it on hand and the circuit is tweaked for use with the RG-174 (I used about one meter of the cable which gave me about 8 ns long pulse). Connect the PULSE\_OUT port to anything you want (50 Ohm terminated).

----
## Images
Oscillogram of the whole pulse:
![Oscillogram of the whole pulse](/images/whole_pulse.jpg)
Oscillogram of the rising edge:
![Oscillogram of the rising edge](/images/zoomed_in_rising_edge.jpg)
Oscillogram of the zoomed in pulse:
![Oscillogram of the zoomed in pulse](/images/zoomed_in_pulse.jpg)
Picture of the made and assembled PCB:
![Picture of the made and assembled PCB](/images/made.jpg)
Picture of the PCB connected to oscilloscope and generator:
![Picture of the made and assembled PCB](/images/connected.jpg)

----
## Reference
[AN-94: Slew Rate Verification for Wideband Amplifiers](https://www.analog.com/media/en/technical-documentation/application-notes/an94f.pdf)

[AN-72: A Seven-Nanosecond Comparator for Single Supply Operation](https://www.analog.com/media/en/technical-documentation/application-notes/an72f.pdf)

[Tweet with photo of the etched PCB](https://twitter.com/polasek_petr/status/1063875099940532225)

[Tweet with photos of the etched, drilled and assembled PCB](https://twitter.com/polasek_petr/status/1064328075725426688)

[Tweet with measured output of the circuit](https://twitter.com/polasek_petr/status/1064698462539866112)