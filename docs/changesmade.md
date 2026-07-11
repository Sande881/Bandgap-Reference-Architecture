* Drafted a core BGR Cell and peformed required temp sweep.
* 
* In stability analysis, the bode plot's gain curve moves upward. This is caused by the ideal amplifier's infinite bandwidth letting parasitic capacitances turn into low-impedance paths, feeding the AC signal directly into the inputs. To fix this, a dominant RC pole was added (10k ohm, 1 pF) at the output to force the curve to roll-off normally.
  
* Added a startup circuit, during the testing of this circuit, I realized that the gain wasn't registering properly in the SPICE environment, causing the Bandgap Reference Voltage to sit at 1.3 V. I had to make the gain 100, and had to change the dominant pole capacitor to 1 nF to prevent excess amplified voltage shooting into the current sources.
  
*   
