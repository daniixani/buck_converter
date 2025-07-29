# buck_converter
This project simulates a basic buck converter circuit in LTspice to validate its performance against design specifications.

1 | Background
The specifications were based on the needs of a microcontroller or any 5V device. The reason I constructed the circuit was to demonstrate that I can calculate, verify, and design a functional DC-DC buck converter in under 3 days. Please note that anything related to power electronics is self-taught but I was guidance the past 9 months under a research position.  

 
2 | Plan

Iterate in LTspice

Start ideal → add Schottky model (D1N5819) and ESR.

Sweep L & C around the math pick to hit ripple ≤ 1 %.

Add .meas statements to auto‑report Pin, Pout, efficiency, fsw, Iout, Vout.

Reality checks

Efficiency drops (switch & diode losses).

Ripple shifts when ESR is included.

Simulation makes it obvious when CCM is accidentally broken (Iₗ hits zero).

3 | Simulation Results (20 ms run, steady‑state slice)
Metric	Simulated	Target	Pass?
Vout (avg)	5.09 V	4.90–5.10 V	✅
Iout (avg)	0.509 A	0.50 A	✅
Ripple (pk‑pk)	29 mV (0.57 %)	≤ 1 %	✅
Efficiency	86.4 %	≥ 85 % (no synchronous MOSFET)	✅
ƒsw (measured)	100.3 kHz	100 kHz	✅

Expectation vs Reality

Aspect:	Expected (paper):	Reality (sim):	Commentary
Inductor value	195 µH:	Fine‑tuned to 50 µH for smaller size while staying CCM:	Smaller L ⭢ higher ripple, but bigger C fixed it
Capacitor value	50 µF: 100 µF with 0.05 Ω ESR:	Extra C cut ripple in half
Efficiency:	90 % (ideal switch):	86 % (with diode & ESR): Matches loss estimates (3 % diode, 1 % switching)
Ripple	< 50 mV:	29 mV: Better than spec due to larger C

4 | Files in This Repo
File	Purpose
buck_conv_proj1.asc	LTspice schematic
buck_conv_proj1.plt	Plot settings (Vout & Iout)
buck_converter_results.pdf	Polished report (metrics, math, ripple calc)
README.md	You’re reading it
