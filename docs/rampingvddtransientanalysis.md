.param TRF=10u

VDD VDD 0 pulse(0 1.8 0 'TRF' 'TRF' 500u 1m)

.control

  tran 10n 150u
  
  plot v(VDD) v(vbg) v(vgate) v(vstartup)
