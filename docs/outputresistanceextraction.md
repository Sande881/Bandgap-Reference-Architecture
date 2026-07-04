*SkyWater 130nm PMOS Characterization Testbench
*Error-Free Windowed Ro and Lambda Extraction

.options scale=1u
.lib /foss/pdks/ciel/sky130/versions/7b70722e33c03fcb5dabcf4d479fb0822d9251c9/sky130A/libs.tech/combined/sky130.lib.spice tt

.param W_val=10 L_val=2 nf_val=1 mult_val=1

XM1 drain gate 0 0 sky130_fd_pr__pfet_01v8 
+ W=W_val L=L_val nf=nf_val mult=mult_val
+ ad={int((nf_val+1)/2) * W_val/nf_val * 0.29} 
+ pd={2*int((nf_val+1)/2) * (W_val/nf_val + 0.29)}
+ as={int((nf_val+2)/2) * W_val/nf_val * 0.29} 
+ ps={2*int((nf_val+2)/2) * (W_val/nf_val + 0.29)}
+ nrd={0.29 / W_val} nrs={0.29 / W_val}
+ sa=0 sb=0 sd=0

VDS drain 0 DC=-1.2
VGS gate  0 DC=-1.5

.control
*Sweep VDS strictly from -1.0V to -1.4V with a 0.2V step
*This creates an array containing exactly 3 data rows: [-1.0V, -1.2V, -1.4V]
  dc VDS -1.0 -1.4 -0.2
  
*Extract currents using precise array index mapping
  let id_point1 = abs(i(VDS)[0])
  let id_nominal = abs(i(VDS)[1])
  let id_point2 = abs(i(VDS)[2])
  
  * --- TWO-POINT MACROSCOPIC EXTRACTION MATH ---
  let delta_v = abs(-1.4 - (-1.0))
  let delta_i = abs(id_point2 - id_point1)
  let ro_realistic = delta_v / delta_i
  let lambda_realistic = 1.0 / (ro_realistic * id_nominal)

  echo "=========================================================="
  echo "      REALISTIC SKYWATER 130NM PFET PARAMETERS           "
  echo "=========================================================="
  echo "Measured macroscopically between VDS = -1.0V and -1.4V:"
  print ro_realistic
  print lambda_realistic
  echo "Nominal Drain Current (at VDS = -1.2V, VGS = -1.5V):"
  print id_nominal
  echo "=========================================================="
.endc
.end
