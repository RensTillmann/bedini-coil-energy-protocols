
# Bedini Coil Energy Protocols

⚠**Disclaimer:** these are open-source documents generated with the help of AI, based partly on shared knowledge publicly available. You might find them useful as a template or as a source of ideas for refining your test runs.

---

Two open-source, reproducible test protocols for Bedini-style pulsed-coil systems — one for **standard energy audits**, one for **open-system anomaly testing**.

These protocols are **neutral** and focus on **rigorous measurement** so results are credible to both experimenters and skeptics.

---

## Included Protocols

### 1. [`standard_coil_collapse_flyback.md`](standard_coil_collapse_flyback.md)
**Purpose:** Standard coil-collapse / flyback **energy balance** testing.

- Designed for Bedini / SSG / pulsed-coil / flyback capture systems.
- Proves whether **net output energy** exceeds **net input energy** under standard electrical engineering principles.
- Controls for:
  - Battery chemistry effects (pulse desulfation, surface charge).
  - Rotor inertia and mechanical storage.
  - Measurement bandwidth and phase errors.
- Uses three complementary tests: **capacitors-only**, **battery-swap**, **hybrid (cap → known load)**.

### 2. [`open_system_external_energy.md`](open_system_external_energy.md)
**Purpose:** Testing for possible **open-system / external energy inflow**.

- Measures **all initial stored energy** before the run (electrical, chemical, mechanical, thermal).
- Runs to **depletion** or defined cutoff to see if total output exceeds all known sources.
- Includes **environmental controls**:
  - EM shielding / grounding checks.
  - Ambient logging (temperature, humidity, background EM noise).
- Adds **cross-environment** tests to check for correlation with environmental variables.

---

## Why Use These Protocols?

Bedini-style and other pulsed-coil energy claims often run into **false positives** due to:
- Battery recovery after rest.
- Changes in capacity from pulse charging.
- Unmeasured rotor kinetic energy.
- Limited-bandwidth or phase-shifted measurements.
- Ambient energy coupling.

These protocols are written to **remove ambiguity**:
- If there’s **no anomaly**, the data will show it clearly.
- If there **is** an anomaly, the result will be reproducible and defensible.
  
