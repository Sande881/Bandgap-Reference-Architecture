# Bandgap Reference Architecture

## 1.) Project Overview
### 1.1 Description
An Analog-to-Digital Converter (ADC) is dependent upon the accuracy of it's reference voltage. A shift in temperature, or process, affects the reference voltage, resulting in the decrease of precision in the ADC. This project hopes to achieve the creation of a Bandgap Reference Circuit that will reasonably resist this shift and provide a constant refernce voltage.

### 1.2) Objectives
This BGR shall Utilize parasitic vertical BJTs to sum Proportional-to-Absolute-Temperature (PTAT) and Complementary-to-Absolute-Temperature (CTAT) current profiles to produce a stable nominal output of ≈ 1.2V. It will implement an active startup network to guarantee state transition out of the zero-current operating point upon supply power-up, and wide-swing cascode current mirrors to translate and distribute precise bias currents across external system loads. 

### 1.3) Key Features

* Designed using the open-source SkyWater 130nm PDK, as it integrates seamlessly with free, open-source EDA toolchains.
  
* A dedicated circuit designed to kick the reference out of its "zero-current" state during supply power-up, guaranteeing self-initialization.

* Device modelling by moving away from simplistic square-law approximations by explicitly accounting for channel-length variations ($\lambda$ or $r_o$) during the initial analytical design phase.

* Achieves a Temperature Coefficient (TC) of $\le$ 20 ppm/°C.

* Features a Power Supply Rejection Ratio (PSRR) targeting $\ge$ 40 dB of attenuation at low frequencies.

## 2.) Requirements
### 2.1) Tools:
* IIC-OSIC-TOOLS (Integrated Infrastructure for Collaborative Open Source IC Tools), which contains various softwares and the Sky130 PDK
### 2.2) Software:
* VMWare, running Ubuntu 26.04 LTS.
### 2.3) Constraints & Limitations:
* Stacking transistors in the wide-swing cascode always eats into your voltage headroom. Since the target output is $\approx 1.2\text{V}$ and the standard SkyWater 130nm core supply is typically 1.8V, you only have about 600mV of headroom left for the biasing network to operate.

* Because we take parasitic vertical BJTs (formed naturally inside the CMOS wells) rather than dedicated BJTs, they suffer from high substrate resistance, lower current gain, and higher susceptibility to process variations.

* At 130nm, global process corners (Fast-Fast, Slow-Slow) can heavily skew the PTAT/CTAT balance.

* If the start up network is not designed carefully, the startup circuit might not turn off completely after initialization. This can introduce leakage currents, degrading the final PSRR, and increasing static power consumption.

* PSRR may degrade due to internal node capacitances acting as low-impedance paths for high frequency noise, degrading the protective isolation of the current mirrors.

## 3.) Development Logs
Rough: 
28/05/26 2:25 AM - Refined Documentation

### 3.1)  Research Links:
| Date        | Link          | Note |
| ------------- |:-------------:| -----:|
| 28/05/26     | http://opencircuitdesign.com/analog_flow/    |  Open-Source Analog IC Design Flow  |
| 28/05/26     | https://github.com/iic-jku/IIC-OSIC-TOOLS#4-quick-launch-for-designers      |   IIC-OSIC-TOOLS Docker Image  |
|      |      |      |

### 3.2) Key Decisions & Changes:

### 3.3) Issues & Solutions:

## 4) Development Process
With the complete installation of IIC-OSIC-TOOLS onto Ubuntu, drafting of the core BGR Architecture is done by taking reference from Behzad Razavi's Design of Analog CMOS Integrated Circuits.

   





