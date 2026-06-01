[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.20493560.svg)](https://doi.org/10.5281/zenodo.20493560)

# Fault-Tolerance Scaffold Simulator

A compact educational simulator that introduces the systems logic behind
fault tolerance using a deliberately classical repetition-code model.

```text
unreliable physical components
+ redundancy
+ noisy measurement
+ software decoding
→ improved logical reliability, if the system operates below threshold
```

## Purpose

Fault-tolerant quantum computing is often introduced through specialized
concepts: logical qubits, syndrome measurements, stabilizer codes, decoders,
and thresholds. This simulator provides a simpler starting point.

A logical bit is encoded into an odd-length string of repeated physical bits.
Hardware errors flip physical bits with probability `p` each cycle. Measurement
errors corrupt the software-visible readout with probability `q` each cycle. A
majority-vote decoder attempts to recover the original logical bit. Running
multiple correction cycles models the sustained error–correction loop that fault
tolerance requires in practice.

The goal is not to simulate a full quantum error-correcting code. The goal is to
make the hardware–measurement–software structure of fault tolerance visible.

## Four Central Lessons

1. Redundancy combined with software decoding can suppress logical failure below
   any target rate, provided hardware errors are below threshold.
2. `p = 0.5` marks a total-randomness boundary where majority voting loses all
   information, but practical operation is governed by much stricter logical-error
   targets such as `1e-2`, `1e-3`, or lower.
3. Sustaining logical fidelity over many correction cycles requires the per-cycle
   error rate to stay below threshold for every cycle in the computation.
4. A resource-estimation curve directly answers how many physical bits are needed
   to reach a target logical error rate for a given hardware error `p` and cycle
   budget `k`.

## Core Model

For odd string length `N`:

```text
0 → 000...0
1 → 111...1
```

Each cycle:

```text
logical bit
→ hardware bit flips with probability p  (applied to current corrected state)
→ measurement/readout errors with probability q
→ majority-vote decoder
→ corrected physical state carried forward
```

Effective error probability seen by the decoder:

```text
r = p(1 − q) + (1 − p)q = p + q − 2pq
```

Logical failure occurs when more than half the measured bits are wrong.
Multi-cycle failure probability is approximated by:

```text
P_fail(k) = 1 − (1 − p_L)^k
```

where `p_L` is the single-cycle analytical failure rate.

## Simulator Panels

| Panel | What it shows |
|---|---|
| **(a) Majority-vote curves** | Logical failure rate vs hardware error `p` for several code lengths `N` at fixed `q` and `k`. The `p_L = p` diagonal and the `p = 0.5` boundary are shown for reference. |
| **(b) Operating region heatmap** | Joint `(p, q)` operating region for selected `N` and `k`. Color indicates logical failure probability. Red contours mark user-selected targets. |
| **(c) Resource estimation** | Logical failure rate vs code length `N` for several hardware error values `p`. Crossing points with target lines give the minimum `N` required to reach each target. |

## Controls

| Control | Meaning |
|---|---|
| `N bits` | Repetition string length; forced to odd values. |
| `p` | Hardware bit-flip probability per cycle. |
| `q` | Measurement/readout error probability per cycle. |
| `cycles k` | Number of sustained error–correction rounds. |
| `trials` | Number of Monte Carlo samples. |
| `target` | Input logical bit, `0` or `1`. |
| `use seed / seed` | Reproducible Monte Carlo sampling. |
| `p max / q max` | Heatmap axis zoom. |
| `logical target 1–3` | User-selected logical-error contour levels. |
| `N max (resource)` | Maximum code length shown in Panel (c). |
| `p values (resource)` | Hardware error values shown in Panel (c). |
| `save prefix` | Filename prefix for saved figure. |
| `save PNG / save PDF` | Export the three-panel figure. |

## Repository Contents

```text
fault_tolerance_scaffold.ipynb        main interactive notebook
fault_tolerance_technical_brief_v2.0.pdf    companion technical brief
fault_tolerance_exercises_v2.0.pdf         guided exercises with pre/post questions
README.md                             this file
requirements.txt                      Python dependencies
LICENSE                               MIT license
```

## Requirements

```text
jupyterlab>=4.3.4
notebook>=7.3.2
ipywidgets>=8.1.5
matplotlib>=3.10.0
ipython>=8.31.0
```

Install dependencies:

```bash
pip install -r requirements.txt
```

## Running the Notebook

Start JupyterLab:

```bash
jupyter lab
```

Open `fault_tolerance_scaffold.ipynb`, run Cell 1, then Cell 2.
Adjust the controls and click **Run simulator**.

## Companion Documents

**Technical brief** (`fault_tolerance_scaffold_brief.pdf`) covers the
analytical model, multi-cycle recurrence, resource estimation, and connection
to quantum error correction in detail.

**Guided exercises** (`fault_tolerance_exercises.pdf`) provide structured
activities organized around the four central lessons, with pre- and
post-exercise questions and answers.

## Connection to Quantum Error Correction

| Classical scaffold | Quantum error-correction analogue |
|---|---|
| Physical bit flip | Physical qubit error |
| Measurement/readout error `q` | Noisy syndrome measurement |
| Majority vote | Decoder / inference algorithm |
| Corrected logical bit | Logical qubit or Pauli-frame update |
| Logical failure probability `p_L` | Logical error per round or operation |
| Cycle count `k` | Circuit depth / number of QEC rounds |
| Resource curve `N` vs `p_L` | Physical-to-logical qubit overhead |

The same systems principle applies in both settings:

> Fault tolerance is a hardware–measurement–software co-design problem.

## Limitations

This is a scaffold, not a full quantum error-correction simulator. It does not
include phase errors, stabilizer measurements, syndrome extraction circuits,
correlated errors, leakage, erasures, decoder mismatch, surface-code geometry,
or real-time decoder latency.

## Acknowledgment

This material was developed and/or adapted with support from the National
Science Foundation through the QCAP-Pilot and QCAP-Design efforts under
NSF Award Nos. OSI-2410813 and OSI-2531569. Any opinions, findings,
conclusions, or recommendations expressed in this material are those of the
author(s) and do not necessarily reflect the views of the National Science
Foundation.

## License

Released under the MIT License. See `LICENSE` for details.

## Attribution

```text
Boris Kiefer,
Fault-Tolerance Scaffold Simulator:
Hardware Errors, Measurement Errors, and Software Decoding.
New Mexico State University.
```

## Author

Boris Kiefer
New Mexico State University

- GitHub: [boriskiefer](https://github.com/boriskiefer)
- LinkedIn: [boris-kiefer-85089831](https://www.linkedin.com/in/boris-kiefer-85089831/)

## How to cite
Boris Kiefer, *Fault-Tolerance Scaffold Simulator: Hardware Errors, Measurement Errors, and Software Decoding*, Zenodo,
https://doi.org/10.5281/zenodo.20493560
