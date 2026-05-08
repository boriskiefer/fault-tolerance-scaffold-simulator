# Fault-Tolerance Scaffold Simulator

A compact educational simulator that introduces the systems logic behind fault tolerance using a deliberately classical repetition-code model.

The simulator is designed to demystify quantum error correction by first showing the core idea in a setting that requires no quantum mechanics:

> unreliable physical components + redundancy + noisy measurement + software decoding  
> → improved logical reliability, if the system operates in a useful error regime.

## Purpose

Fault-tolerant quantum computing is often introduced through specialized concepts such as logical qubits, syndrome measurements, stabilizer codes, decoders, and thresholds. This simulator provides a simpler starting point.

It models a logical bit encoded into an odd-length string of repeated physical bits. Hardware errors flip physical bits with probability `p`. Measurement errors corrupt the software-visible readout with probability `q`. A majority-vote decoder then attempts to recover the original logical bit.

The goal is not to simulate a full quantum error-correcting code. The goal is to make the hardware–measurement–software structure of fault tolerance visible.

## Core idea

For an odd string length `N`, a logical bit is encoded as

```text
0 → 000...0
1 → 111...1