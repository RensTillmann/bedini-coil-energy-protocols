
# Open-System Energy Test Protocol — Detecting Unexplained Energy Inflow

> **Purpose:** Provide a rigorous, reproducible procedure to test whether a pulsed-coil / flyback capture system operates as an **open energy system**, meaning it delivers more usable energy than can be explained by its **initial stored energy** and measured **external inputs**.

This protocol is designed to **either support or falsify** the claim that such systems can draw extra energy from an external source (e.g., “vacuum energy,” “radiant energy,” “ambient environment”) without presuming the origin of any detected anomaly.

---

## Key Difference from Standard Energy Balance Protocol

The **standard protocol** compares **input vs output** during operation.

The **open-system protocol** additionally:
- **Quantifies all initial stored energy** before start (electrical, mechanical, chemical, thermal).
- **Runs the system to depletion** or a defined cutoff to see if total output exceeds the sum of measured inputs **plus** initial stored energy.
- **Controls the environment** to detect or rule out ambient energy coupling.

---

## Scope

Applies to:
- Bedini-style pulse chargers (BJT or MOSFET/SiC).
- Flyback capture systems into batteries or capacitors.
- Rotating-mass systems (flywheels, magnet rotors) where kinetic energy may store/release energy.
- High-voltage pulse-charging configurations.

---

## What This Protocol Can Prove

- Whether total delivered energy exceeds the sum of **initial stored energy** + **external inputs** within measurement uncertainty.
- Whether the excess is consistent across environments and operating conditions.

**Cannot prove:**
- The physical source of any detected excess (e.g., “vacuum energy” vs unrecognized coupling or storage effect).
- Any specific theoretical model.

---

## Step 0 — Characterize Initial Stored Energy

**Purpose:** Ensure we know exactly how much energy is in the system before starting.

1. **Electrical storage** — Measure `E_cap = 0.5 * C * V²` for all capacitors in circuit (including hidden snubbers, filters).  
2. **Battery chemical energy** — Fully charge each battery, rest 2 h, then perform a **C/20 discharge** to cutoff to get Wh capacity. Recharge fully for the test. This Wh is the **full initial chemical stock**.  
3. **Mechanical storage** — If rotor/flywheel starts with motion, compute `E_rotor = 0.5 * J * ω²` (measure RPM; estimate inertia).  
4. **Thermal storage** (if relevant) — Measure temperature of large masses if heating is later used as “output work.”

Record these in a **baseline energy table**.

---

## Step 1 — Isolate and Control Environment

- **EM shielding:** If possible, run in a Faraday cage or grounded shielded space to block ambient RF/EM coupling.
- **Grounding:** Ensure a single-point ground; log any unexpected current paths.
- **Ambient logging:** Record temperature, humidity, background EM noise before and during run.
- **Physical isolation:** No physical tethers to mains-powered equipment unless measured.

---

## Step 2 — Measure All External Inputs

- Use bandwidth-correct instrumentation (≥100 MHz oscilloscope with V/I probes) to measure **all electrical power** entering the system.  
- If mechanical drive is used (e.g., motor-assist), measure torque × RPM for mechanical input power.
- If thermal or fluid input exists, measure via appropriate sensors.

Integrate over time to compute total **E_input** for the run.

---

## Step 3 — Define Cutoff Condition

Choose a clear, reproducible endpoint, such as:
- All batteries discharged to agreed cutoff voltage.
- Rotor at rest.
- Capacitors discharged to safe low voltage.

---

## Step 4 — Measure All Outputs

**During and/or after run:**
- **Electrical**: Wh delivered to loads or captured in capacitors (`0.5 * C * (V2² - V1²)`).
- **Chemical**: Remaining Wh in batteries at cutoff (discharge on controlled load after rest).
- **Mechanical**: Final rotor kinetic energy (if any).
- **Thermal**: Measured calorimetrically if heating is a load.

---

## Step 5 — Energy Balance

Total available energy = **Initial stored energy** + **E_input**

Compare to:
**Total delivered energy** = All measured outputs (electrical + chemical + mechanical + thermal).

**Pass condition for anomaly:**  
`Total delivered energy > Total available energy + uncertainty margin`

If the above is met, the system exhibits **unexplained energy inflow** under these test conditions.

---

## Step 6 — Cross-Environment Tests

To investigate possible ambient sources:
- Repeat in shielded vs unshielded environments.
- Repeat at different geographic locations (if possible).
- Repeat with varying background EM noise (e.g., near/far from strong RF sources).
- Compare results for correlation with environmental variables.

---

## Measurement Rules

- **Simultaneous V/I capture** to avoid phase errors.
- **Wide bandwidth** to accurately capture fast pulses (≥100 MHz, preferably more for SiC systems).
- **Kelvin sensing** for shunts; short leads; coax or twisted pair.
- **Temperature logging** for all major components.
- **Rotor start/stop from rest** unless mechanical storage is accounted for.
- **Uncertainty accounting**: Include instrument accuracy, shunt tolerance, probe calibration.

---

## Controls

- **Dummy load run:** Replace output battery with resistive load to verify no “surplus” without chemical storage effects.
- **Capacitor-only run:** Replace batteries with capacitors; compare capture efficiency.
- **Swap run:** Swap source and capture batteries to eliminate asymmetry bias.

---

## Data to Publish

- Full baseline energy table (initial stored energies).  
- Full run data: scope traces, integrated power curves, battery logs, RPM logs, temperature logs.  
- Environment logs (shielding, EM noise, location).  
- Uncertainty budget.

---

## Safety

- High-voltage collapse spikes can exceed hundreds of volts — ensure probe and component ratings have margin.
- Enclose high-energy circuits.
- Use fuses/current limits on first power-up.
