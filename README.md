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
| 16/06/26     | https://patents.google.com/patent/US20170012609A1/en     |  Reference design for the startup circuit    |

### 3.2) Key Decisions & Changes:

### 3.3) Issues & Solutions:

## 4) Development Process
Week 1:
First, we obtain the process and device characteristics of the NMOS and PMOS we are to use. The transistor models, from which we shall extract characteristics, are:
- sky130_fd_pr/nfet_01v8_nf.sym
- sky130_fd_pr/pfet_01v8_nf.sym

The circuit is set up in the following manner:

<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/90e0ad4e-3831-47af-ae01-5371e866b50f" />

Performing the required ngspice simulation, we get the follow parameters:

* Threshold Voltage (V<sub>th</sub>) = 368.4887 mV
* &micro;C<sub>ox</sub>= 58.00941 &mu;A/V²
* Output Resistance (r<sub>o</sub>) = 164.92 k&Omega;
* Channel Length Modulation Parameter (&lambda;) = 0.052 V<sup>-1</sup>

doing the same for the pfet model, we get the following parameters:

* Threshold Voltage (V<sub>th</sub>) = -390.54 mV
* &micro;C<sub>ox</sub>= 75.3546 &mu;A/V²
* Output Resistance (r<sub>o</sub>) = 2.33 M&Omega;
* Channel Length Modulation Parameter (&lambda;) = 0.0115 V<sup>-1</sup>

The core BGR Cell will be drafted from Behzad Razavi's Design of Analog CMOS Integrated Circuits,

Equations:

$I_{Q1} = I_{Q2}$ if $R_2 = R_3$. We also have $\vert{}V_{BE1}\vert{} = \vert{}V_{BE2}\vert{} + I_{C2}R_1$ and hence $I_{C2} = V_T \ln n / R_1$.

$$\vert{}I_{D4}\vert{} =  \frac{1}{R_2} \left( \vert{}V_{BE1}\vert{} + \frac{R_2}{R_1} V_T \ln n \right) \tag{12.68}$$

$$V_{BG} = \frac{R_4}{R_2} \left( \vert{}V_{BE1}\vert{} + \frac{R_2}{R_1} V_T \ln n \right) \tag{12.69}$$

$$\frac{R_2}{R_1} \ln(n) = 17.2$$   (Zero Temperature-Coefficient Condition)

Calculations:
n = 8 and choosing I<sub>D4</sub> as 10 &mu;A, we get

$$\frac{R_2}{R_1}  \approx \mathbf{8.272}$$

$$\vert{}I_{D4}\vert{} = \frac{1}{R_2} \left( \vert{}V_{BE1}\vert{} + 17.2 \cdot V_T \right) \implies R_2  = \mathbf{109.55 \text{ k}\Omega}$$

$R_2 = R_3 = \mathbf{109.55 \text{ k}\Omega}$

$R_1 = 13.24 kΩ$

$$V_{BG} = \frac{R_4}{R_2} \left( \vert{}V_{BE1}\vert{} + \frac{R_2}{R_1} V_T \ln(n) \right) \implies \frac{R_4}{R_2} \approx \mathbf{1.0954}$$

$R_4 \approx \mathbf{120.0 \text{ k}\Omega}$

Circuit:

<img width="900" height="600" alt="image" src="https://github.com/user-attachments/assets/10706b4b-4606-4ca5-b36d-3a2fbee61dae" />






  
  


   





