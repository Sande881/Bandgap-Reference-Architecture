cd ~/.xschem/simulations/

* Xschem:  

nano /headless/.xschem/simulations/threshold.spice

echo 'export PDK_ROOT=/foss/pdks/ciel/sky130/versions/7b70722e33c03fcb5dabcf4d479fb0822d9251c9' >> ~/.bashrc

echo 'export PDK=sky130A' >> ~/.bashrc

source ~/.bashrc

* Ngspice:

ngspice /headless/.xschem/simulations/threshold.spice

ngspice -b /headless/.xschem/simulations/threshold.spice (batch mode)

* PDK:
  
find / -name "sky130.lib.spice" 2>/dev/null (finding pdk)

* Library:
  
.lib /foss/pdks/ciel/sky130/versions/7b70722e33c03fcb5dabcf4d479fb0822d9251c9/sky130A/libs.tech/combined/sky130.lib.spice tt  (library inclusion)

