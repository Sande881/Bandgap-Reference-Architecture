* Drafted a core BGR Cell and peformed required temp sweep.
  
* In stability analysis, the bode plot's gain curve moves upward. This is caused by the ideal amplifier's infinite bandwidth letting parasitic capacitances turn into low-impedance paths, feeding the AC signal directly into the inputs. To fix this, a dominant RC pole was added (10k ohm, 1 pF) at the output to force the curve to roll-off normally.
  
* Added a startup circuit, during the testing of this circuit, I realized that the gain wasn't registering properly in the SPICE environment, causing the Bandgap Reference Voltage to sit at 1.3 V. I had to make the gain 100, and had to change the dominant pole capacitor to 1 nF to prevent excess amplified voltage shooting into the current sources.
  
* When performing Ramping of Supply voltage VDD to check that the startup circuit safely brings the BGR upto 1.2 V and detaches once it reaches 1.2V, the Bandgap Reference Voltage spikes to -120 V in the transient analysis when a Fast ramp of 100 ns is performed, acting like a voltage spike. This is because the SPICE environment uses Trapezoid Integration by default for simulation, which doesn't fare well against sharp edges (a lack of "L-Stability"). For this, I had to switch to gear integration to accurately simulate the nature of the circuit against fast and slow ramping of VDD.

* 
