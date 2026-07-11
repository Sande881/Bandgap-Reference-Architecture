<img width="1000" height="500" alt="Screenshot2026-07-08212321" src="https://github.com/user-attachments/assets/973e4ed2-5c3c-44f2-922a-fe517d122ad7" />



code:

name=zap

only_toplevel=true

type=text

value="
.options scale=1u
.save all
.lib "/foss/pdks/ciel/sky130/versions/7b70722e33c03fcb5dabcf4d479fb0822d9251c9/sky130A/libs.tech/combined/sky130.lib.spice" tt

 V_VDD VDD 0 DC=1.8

.control
 *Sweep frequency from 1 Hz up to 10 GHz (20 points per decade)
  ac dec 20 1 10g
  
  let loop_gain = v(vin)/v(vgate)
  let db_gain = db(loop_gain)
  let phase_gain = phase(loop_gain) * 180 / PI

  plot db_gain phase_gain
.endc
"




