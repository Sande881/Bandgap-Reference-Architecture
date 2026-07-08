name=zap

only_toplevel=true

type=text

value="
.options scale=1u
.save all
.lib "/foss/pdks/ciel/sky130/versions/7b70722e33c03fcb5dabcf4d479fb0822d9251c9/sky130A/libs.tech/combined/sky130.lib.spice" tt

V_VDD VDD 0 DC=1.8

.control
  dc temp -40 125 1
  
  let vbg_room = v(vbg)[67]
  let max_vbg = vecmax(v(vbg))
  let min_vbg = vecmin(v(vbg))
  let delta_vbg = max_vbg - min_vbg
  let temp_co = (delta_vbg / (vbg_room * 165)) * 1e6

  echo ==========================================================
  echo          EXTRACTED SKY130 BGR CELL METRICS                
  echo ==========================================================
  print vbg_room
  print temp_co
  echo ==========================================================
  
  plot v(vbg)
.endc
"
