
# Coil-Collapse Energy Audit Protocol — Reproducible, Bandwidth-Correct, Battery-Neutral

> **Purpose:** Provide a rigorous, instrumented procedure to determine whether a pulsed-coil, rotor-assisted system with flyback capture (a.k.a. Bedini/SG-style rigs, MOSFET/SiC upgrades, optical/Hall timing) delivers **net energy output exceeding net energy input**.

This version adds: **high-voltage & SiC switching guidance**, **“work done” definitions**, **battery pulse-effects controls**, and an explicit section on **what this protocol can and cannot prove**.

---

## What This Protocol Can Prove (and What It Cannot)

**Can prove (if followed exactly):**
- Whether the system’s **net electrical/chemical/mechanical output energy** exceeds the **net input energy** by more than the combined measurement uncertainty.
- Whether energy recovered from coil collapse is consistent with **standard flyback/boost behavior** or indicates an anomaly.

**Cannot prove:**
- The **origin** of any observed surplus (e.g., “vacuum energy,” “radiant/scalar energy”). Physics attribution requires separate theoretical and experimental work.
- Superiority of one narrative over another. This protocol **only** establishes a **reproducible energy balance**.

> If the device is over-unity, it must pass **every test** below (caps-only, batteries, and hybrid) by a margin **greater than the stated uncertainty**. If any single test fails, the over-unity claim is **not supported**.

---

## Claims Under Test (Neutral Wording)
1. **Pulse charging / high-voltage spikes** may improve battery behavior (e.g., desulfation) and **appear** to increase delivered energy.
2. **Fast switching** (e.g., SiC MOSFETs, SiC diodes) improves capture of the coil-collapse energy.
3. **Higher DC bus voltage** (e.g., 120 V vs 12 V) increases available pulse energy and capture efficiency.
4. **System-level “work done”** might exceed the known initial energy stock if the system is an **open** energy system.

This protocol accepts points 1–3 as **plausible within standard engineering** and designs tests to **eliminate false positives** that can arise from them. Point 4 is the **hypothesis** under test.

---

## Pass/Fail Criteria

A claim of “excess energy” is accepted only if **all** of the following are true:

- **Test 1 (Capacitors Only):** `E_cap > E_in` by > combined uncertainty.
- **Test 2 (Batteries):** `E_out > E_in` **after swap** (A↔B) by > combined uncertainty.
- **Test 3 (Hybrid, Cap → Known Load):** `E_load > E_in` by > combined uncertainty.
- **Rotor energy neutrality:** Start and end with rotor stopped (or account for ΔE_kinetic explicitly).
- **Temperature control:** Battery and ambient within specified bounds; corrections applied where applicable.
- **Bandwidth & simultaneity:** V(t) and I(t) measured concurrently with sufficient bandwidth.

Expectation under standard electromagnetism: `E_cap`, `E_out`, and `E_load` will be **less** than `E_in` due to resistive, switching, core, and EMI losses.

---

## Required Instruments

- **Power source**: Programmable DC supply (preferred) *or* source battery with a **4-terminal shunt**.
- **Oscilloscope** ≥ 100 MHz (preferably 200–400 MHz for fast SiC edges) with simultaneous channels for **V(t)** and **I(t)**.
  - **Differential HV probe** (≥ the highest expected drain/transient voltage).
  - **Current probe** (Pearson/Rogowski) or **low-inductance 4-terminal shunt** + coax.
- **Precision power analyzer / watt-hour meter** (optional but recommended) on input and/or output.
- **Electronic DC load** with logging (for C/20 discharges) and programmable profiles.
- **Thermocouples** or RTDs for ambient and battery temps.
- **Capacitor bank**: Polypropylene, known `C`, low ESR, rated for expected pulses.
- **Tachometer/strobe** (optional) for rotor RPM to compute/verify ΔE_kinetic.
- **EMI snubbers** (RCD or RC) for controlled ringing; layout with low loop area.

---

## Schematic Concept (Measurement Nodes)

```
            +Vin (DC supply or Source Batt A)
                   │
                [ Shunt ]  ← 4‑terminal (Kelvin sense to scope)
                   │
              +----┴----+
              | Driver  |  (SiC gate driver)
              +----┬----+
                   │
                Coil L
                   │
                  ├─────>|───── Output Node ────> (Cap bank and/or Batt B)
                  │      D (SiC diode)
                  │
                 GND

Measure input V(t) & I(t) at the driver board terminals (post-cable).
Measure output energy at the storage element (cap voltage), or via logged discharge.
```

---

## Pre-Conditioning (Batteries)

**Reasoning:** Pulse charging and rest effects alter apparent capacity; standardize or results are ambiguous.

1. Select two **identical** healthy lead-acid batteries **A** and **B**.
2. Equalize temperature (±0.5 °C). Rest **12 h**.
3. Fully charge (CC/CV), rest **2 h**.
4. Perform a **single** C/20 discharge on each to characterize baseline Wh at test temperature. Record.
5. Recharge both to identical SoC, rest **2 h** before experiments.

---

## Test 1 — Pure Electrical (Capacitors Only)

**Purpose:** Remove battery chemistry. Determine if collapse capture exceeds input.

1. Source: DC supply (or battery A). Output: **capacitor bank** only.
2. Start with **rotor stopped**.
3. Scope: capture `V_in(t)` and `I_in(t)` at the driver board. Compute `P_in(t)=V_in*I_in`; integrate over run:  
   `E_in = ∑ P_in * Δt`
4. Track capacitor voltage from **V1** to **V2**:  
   `E_cap = 0.5 * C * (V2² - V1²)`
5. Stop with **rotor stopped**; verify ΔE_kinetic≈0 (or compute from RPM & inertia).

**Pass/Fail:** `E_cap > E_in` beyond uncertainty would indicate anomaly.  
**Expected:** `E_cap < E_in`.

**Notes for high‑voltage/SiC builds:**  
- Ensure probe ratings exceed worst-case flyback.  
- Keep switch-node loops tight; add RCD clamp if necessary to prevent probe damage and measurement corruption.

---

## Test 2 — Battery Energy Accounting (with Swap)

**Purpose:** Measure *chemical* energy stored, avoiding surface-charge illusions.

1. Use **A** as source, **B** as receiver (collapse→B).  
2. Measure `E_in` as in Test 1 (scope integration at the board).  
3. After charging, let **B** rest **2 h**.  
4. Discharge **B** at **C/20** to standard cutoff (e.g., 10.5 V for 12 V LA) on the electronic load; log Wh → `E_out`.  
5. **Swap roles** and repeat with **B** as source, **A** as receiver.

**Pass/Fail:** Must show `E_out > E_in` for **both** directions by > uncertainty.  
**Expected:** `E_out < E_in`.

**Controls addressing pulse‑effects:**  
- If pulse desulfation increases capacity, the **baseline** (Step 4 in Pre‑Conditioning) reveals it in both batteries; the **swap** cancels asymmetry.  
- Temperature-compensate cutoff voltages and record temps throughout.

---

## Test 3 — Hybrid (Cap Output → Known Load)

**Purpose:** Preserve collapse-capture dynamics but eliminate battery measurement uncertainty.

1. Source: battery or DC supply. Output: **capacitor bank**.  
2. When cap reaches threshold, **dump** into a precision resistor (or small water calorimeter) with high-rate logging of V/I/t.  
3. Sum Wh across N cycles: `E_load`. Compare to `E_in` from the scope integration.

**Pass/Fail:** `E_load > E_in` beyond uncertainty.  
**Expected:** `E_load < E_in`.

---

## “Work Done” Definition (To Avoid Ambiguity)

Claims based on “work done” must quantify **all** relevant energy terms:

- **Electrical:** Wh delivered to external loads or stored in capacitors (`0.5*C*(V2² - V1²)`), logged with bandwidth‑correct measurements.  
- **Chemical:** Wh delivered by a battery on a **standardized** discharge profile after rest.  
- **Mechanical:** Change in rotor kinetic energy `ΔE_kinetic = 0.5 * J * (ω₂² − ω₁²)`; measure RPM precisely, estimate inertia `J` or start/stop from rest.  
- **Thermal:** If heating is a load, measure calorimetrically.

Any “work done” proof **must** show that the **sum** of outputs exceeds `E_in` by > uncertainty **and** that transient stores (caps, rotor, batteries) have not simply been drawn down.

---

## Measurement Rules That Prevent False Positives

- **Simultaneous V & I** (same scope timebase) to avoid phase errors in `P(t)`.  
- **Bandwidth**: ≥100 MHz (preferably 200–400 MHz for SiC). Nanosecond edges alias on slow meters.  
- **4‑terminal shunts**: Kelvin connections; short leads; coax; minimal loop area.  
- **Measure at the board**: Include cable drops and switching losses.  
- **Temperature control**: Log ambient + battery temps; apply standard compensation.  
- **EMI hygiene**: Twisted pairs; snubbers/RCD clamps; shielded probes; good ground strategy.  
- **Uncertainty margin**: Claim “excess” only if margin ≥ 5–10% above combined uncertainty (RSS).  
- **Clamp‑meter disclaimer**: DC clamp meters are **not** valid for spike-rich waveforms; use scope‑integrated V·I.

---

## Controls & Cross‑Checks

- **Flyback reference**: Drive coil with a function generator + MOSFET (no rotor). Expect behavior consistent with a standard flyback/boost converter.  
- **Closed‑loop trial**: After charging B, attempt to run the rig from B while charging A (and powering a known load). Sustained operation with surplus is required; in practice systems decay.  
- **Battery‑neutral test**: Repeat Test 1 at multiple duty cycles and bus voltages (e.g., 12 V, 36 V, 120 V) to map capture efficiency vs switching and verify no threshold anomaly.

---

## Data to Publish (For Reproducibility)

- Raw scope traces (`V_in(t)`, `I_in(t)`, switch node, diode current if available).  
- CSV logs for all integrations (input power, cap voltages, battery discharges, load dumps).  
- Temperatures, rotor RPM logs (if applicable), and photos of probe placements.  
- BOM, PCB Gerbers/EDA files, coil parameters (L, R, core), diode/MOSFET part numbers, timing wheel geometry.  
- Uncertainty analysis and final energy balance table for each test.

---

## Safety Notes

- Collapse spikes can exceed **hundreds of volts**. Ensure probe, MOSFET `Vds`, and diode `Vr` margins.  
- Observe creepage/clearance; enclose high‑energy paths.  
- Use fuses/current limits on first power‑up; be prepared for switch-node overshoot.
