# buck_converter
This project simulates a basic buck converter circuit in LTspice to validate its performance against design specifications.

1 | Background
The specifications were based on the needs of a microcontroller or any 5V device. The reason I constructed the circuit was to demonstrate that I can calculate, verify, and design a functional DC-DC buck converter in under 3 days. Please note that anything related to power electronics is self-taught.  

 
Input voltage,  Vin   = 12 V
Target output,  Vout  = 5 V
Nominal load,   Iout  = 0.50 A
Switching freq, fs    = 100 kHz
Chosen ripple,  ΔIL   = 30 % of Iout  ≈ 0.15 A
Allowable ΔVout = 1 % of 5 V         ≈ 0.05 V
------------------------------------------------

(1) Duty‑cycle  (ideal CCM buck)
    D = Vout / Vin
      = 5 V / 12 V
      ≈ 0.4167  (≈ 41.7 %)

(2) Inductor value for the chosen ripple
    L = (Vin − Vout) · D / (fs · ΔIL)
      = (12 V − 5 V) · 0.4167 / (100 kHz · 0.15 A)
      ≈ 1.94 × 10⁻⁴ H  →  **≈ 200 µH**

(3) Output‑capacitor value for ≤ 50 mV ripple  
    (small‑signal approximation, triangular IL)
    C ≥ ΔIL / (8 · fs · ΔVout)
      = 0.15 A / (8 · 100 kHz · 0.05 V)
      ≈ 3.75 × 10⁻⁶ F  →  **≈ 4 µF**

    In the final LTspice run I over‑sized to **47 µF**
    and added 0.05 Ω ESR to reflect a real aluminium
    electrolytic.  This cut the simulated Vout ripple
    to ≈ 29 mV (p‑p).

(4) Expected efficiency (first‑order)  
    • Diode loss  ≈ Vf · Iout  ≈ 0.5 V · 0.5 A  = 0.25 W  
    • Switch loss ≈ (I² · RDS) + tsw · fs etc. ≈ 0.05 W  
    ⇒ Pout ≈ 5 V · 0.5 A = 2.50 W  
    ⇒ η  ≈ 2.50 W / (2.50 + 0.30) W ≈ **89 %**

    LTspice returned 86 – 89 %, matching the back‑of‑
    envelope estimate once parasitics were included.
