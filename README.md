# Fault-Tolerance Scaffold Simulator

A compact educational simulator that introduces the systems logic behind fault tolerance using a deliberately classical repetition-code model.

The simulator is designed to demystify quantum error correction before introducing quantum mechanics:

```text
unreliable physical components
+ redundancy
+ noisy measurement
+ software decoding
-> improved logical reliability, if the system operates in a useful error regime
```

## Purpose

Fault-tolerant quantum computing is often introduced through specialized concepts such as logical qubits, syndrome measurements, stabilizer codes, decoders, and thresholds. This simulator provides a simpler starting point.

It models a logical bit encoded into an odd-length string of repeated physical bits. Hardware errors flip physical bits with probability `p`. Measurement errors corrupt the software-visible readout with probability `q`. A majority-vote decoder attempts to recover the original logical bit.

The goal is not to simulate a full quantum error-correcting code. The goal is to make the hardware–measurement–software structure of fault tolerance visible.

## Core Model

For odd string length `N`:

```text
0 -> 000...0
1 -> 111...1
```

Simulator sequence:

```text
logical bit
-> hardware bit flips with probability p
-> measurement/readout errors with probability q
-> majority-vote decoder
-> logical success or failure
```

The effective error probability seen by the decoder is:

```text
r = p(1 - q) + (1 - p)q
  = p + q - 2pq
```

Logical failure occurs when more than half of the measured bits are wrong.

## Notebook Controls

| Control | Meaning |
|---|---|
| `N bits` | Repetition string length; forced to odd values. |
| `p` | Hardware bit-flip probability. |
| `q` | Measurement/readout error probability. |
| `trials` | Number of Monte Carlo samples. |
| `target` | Input logical bit, `0` or `1`. |
| `p max` | Maximum hardware error shown on the heatmap. |
| `q max` | Maximum measurement error shown on the heatmap. |
| `logical error target 1–3` | User-selected heatmap contour levels. |
| `save prefix` | Filename prefix for saved figure. |
| `save PNG / save PDF` | Export the side-by-side figure. |

## Repository Contents

```text
fault_tolerance_scaffold.ipynb       main interactive notebook
fault_tolerance_scaffold_brief.pdf   companion technical brief
README.md                            project overview
requirements.txt                     Python dependencies
LICENSE                              MIT license
```

## Requirements

A minimal `requirements.txt` is:

```text
jupyterlab>=4.0
notebook>=7.0
ipywidgets>=8.1
matplotlib>=3.8
ipython>=8.0
```

## Running the Notebook

Start Jupyter:

```bash
jupyter lab
```

Open:

```text
fault_tolerance_scaffold.ipynb
```

Run Cell 1, then Cell 2. Adjust the controls and click **Run simulator**.

## Connection to Quantum Error Correction

This simulator is intentionally classical. It provides a scaffold for the quantum version:

| Classical scaffold | Quantum error-correction analogue |
|---|---|
| Physical bit flip | Physical qubit error |
| Measurement/readout error | Noisy syndrome measurement |
| Majority vote | Decoder / inference algorithm |
| Corrected logical bit | Logical qubit or Pauli-frame update |
| Logical failure probability | Logical error per round or operation |

The same systems principle remains:

> Fault tolerance is a hardware–measurement–software co-design problem.

## Limitations

This is a scaffold, not a full quantum error-correction simulator. It does not include phase errors, stabilizer measurements, syndrome extraction circuits, correlated errors, leakage, erasures, decoder mismatch, surface-code geometry, or real-time decoder latency.

## License

This project is released under the MIT License. See `LICENSE` for details.

## Attribution

Suggested attribution:

```text
Boris Kiefer,
Fault-Tolerance Scaffold Simulator:
Hardware Errors, Measurement Errors, and Software Decoding.
```

## Author

Boris Kiefer  
New Mexico State University  

GitHub: https://github.com/boriskiefer  
LinkedIn: https://www.linkedin.com/in/boris-kiefer-85089831/

## Acknowledgement

This material was developed and/or adapted with support from the National
Science Foundation through the QCAP-NQVL-Pilot and QCAP-NQVL-Design efforts
under NSF Award Nos. OSI-2410813 and OSI-2531569.
Any opinions, findings, conclusions, or recommendations expressed in this
material are those of the author(s) and do not necessarily reflect the views
of the National Science Foundation.
