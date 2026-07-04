*Volare system compatibility
.options scale=1u

*PROCESS CORNER LIBRARY INCLUSION
.lib /foss/pdks/ciel/sky130/versions/7b70722e33c03fcb5dabcf4d479fb0822d9251c9/sky130A/libs.tech/combined/sky130.lib.spice tt

*HARDWARE DEVICE UNDER TEST (DUT)
.param W_val=10 L_val=2 nf_val=1 mult_val=1

XM1 drain gate 0 0 sky130_fd_pr__nfet_01v8 
+ W=W_val L=L_val nf=nf_val mult=mult_val
+ ad={int((nf_val+1)/2) * W_val/nf_val * 0.29} 
+ pd={2*int((nf_val+1)/2) * (W_val/nf_val + 0.29)}
+ as={int((nf_val+2)/2) * W_val/nf_val * 0.29} 
+ ps={2*int((nf_val+2)/2) * (W_val/nf_val + 0.29)}
+ nrd={0.29 / W_val} nrs={0.29 / W_val}
+ sa=0 sb=0 sd=0

*IDEAL VOLTAGE STIMULUS SOURCES
VDS drain 0 DC=0.05
VGS gate  0 DC=0.9

*POST-PROCESSING 
.control

*GATE SWEEP (For Vth and Mu_Cox Extraction)
  alter VDS DC = 0.05
  dc VGS 0 1.8 0.01
  
  let gm = deriv(i(VDS))
  let max_gm = vecmax(gm)
  
  *Math weighting filter to catch peak position natively
  let gm_shifted = gm - vecmin(gm)
  let gm_peak_weight = ln(gm_shifted + 1)
  let weight_norm = gm_peak_weight / vecmax(gm_peak_weight)
  let pure_peak_mask = weight_norm * weight_norm * weight_norm * weight_norm * weight_norm
  let clean_mask = pure_peak_mask / vecmax(pure_peak_mask)
  
  let vgs_at_max = vecmax(clean_mask * v(gate))
  let ids_at_max = vecmax(clean_mask * i(VDS))
  
  let vth_extracted = vgs_at_max - (ids_at_max / max_gm) - 0.025
  let mu_cox = abs(max_gm / (0.05 * (10/2)))
  
  *Save run 1 data vector plot frame out of active calculation memory
  set plot_gate = $curplot

*DRAIN SWEEP (For Ro and Lambda Extraction)
  alter VGS DC = 1.0
  dc VDS 0.8 1.8 0.01
  
  let gds = deriv(i(VDS))
  let ro_vector = 1.0 / gds
  let lambda_vector = gds / (i(VDS) - (gds * v(drain)))
  
  *Isolate values at VDS = 1.2V
  let target_vds = 1.2
  let diff = abs(v(drain) - target_vds)
  let min_diff = vecmin(diff)
  let sample_mask = (diff - min_diff + 1e-12) >= 0
  
  let ro_at_sample = vecmax(sample_mask * ro_vector)
  let lambda_at_sample = vecmax(sample_mask * lambda_vector)
  
  set plot_drain = $curplot


  echo "=========================================================="
  echo "      EXTRACTED SKYWATER 130NM NFET PROCESS PARAMETERS    "
  echo "=========================================================="
  echo "--- LINEAR TRANSCONDUCTANCE METRICS (VDS = 0.05V) ---"
  print {$plot_gate}.vth_extracted
  print {$plot_gate}.mu_cox
  echo ""
  echo "--- SATURATION PARAMETERS (VGS = 1.0V, VDS = 1.2V) ---"
  print {$plot_drain}.ro_at_sample
  print {$plot_drain}.lambda_at_sample
  echo "=========================================================="
.endc
.end
