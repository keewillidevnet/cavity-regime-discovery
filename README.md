# AlphaCav

**Physics-Native Generative AI for Acoustic Regime Discovery in Supercavitation**

## Overview

This repository contains the proposal and technical documentation for **AlphaCav**, a generative AI system designed to discover novel acoustic regimes in gas-supported supercavitation.

Analogous to how AlphaFold revolutionized protein structure prediction by learning underlying physics, AlphaCav encodes the multi-physics phenomena governing cavity dynamics, gas entrainment, and acoustic emission to discover regimes that human intuition and traditional CFD would never explore.

## The Survivorship Bias Problem

For 50 years, supercavitation research has explored parameter spaces that:

- Seemed promising based on existing theory
- Were adjacent to known working designs
- Could be reached incrementally from experimental data
- Matched researcher intuition

We have systematically avoided the regimes that "didn't survive" the selection filter of human intuition and incremental CFD refinement. The quiet regimes might be sitting in parameter space nobody ever visited because the path there looked unpromising from where we started.

**AlphaCav corrects for this bias.** The model doesn't know what's "supposed" to work. It only knows what's physically consistent.

## Repository Structure

```
alphacav/
├── README.md                              # This file
├── WHITEPAPER.md                          # Full technical white paper
├── LICENSE                                # BSD 3-Clause License
├── docs/                                  # Additional documentation
│   ├── executive-summary.md               # One-page executive summary
│   ├── technical-architecture.md          # Detailed architecture design
│   ├── training-data-strategy.md          # Data acquisition approach
│   └── stakeholder-analysis.md            # Target organizations and alignment
├── examples/                              # Use case examples
│   ├── regime-discovery.md                # Novel regime discovery workflow
│   ├── validation-workflow.md             # Experimental validation approach
│   └── survivorship-bias.md               # The case for unexplored parameter space
├── figures/                               # Architecture diagrams
│   └── README.md                          # Figure descriptions
├── implementation/                        # Implementation details
│   └── README.md                          # Prototype roadmap
└── .github/workflows/                     # CI/CD automation
    └── validate.yml                       # Markdown linting
```

## The Problem

Gas-supported supercavitation generates intense and highly variable acoustic emissions. The physical origins remain poorly understood because:

- **High-dimensional parameter space**: Flow velocity, gas injection rate, cavitator geometry, downstream disturbances, ambient pressure
- **Sparse experimental coverage**: Facility costs and time constraints limit parametric exploration
- **Expensive simulation**: Unsteady, multiphase, compressible CFD required for acoustic source mechanisms
- **Selection bias**: Researchers study what looks promising, missing potentially valuable unexplored regions

The result is a fragmented understanding of acoustic regime boundaries and the physical pathways connecting cavity dynamics to far-field noise.

## The Gap

| Current Approaches | AlphaCav |
|-------------------|----------|
| Empirical correlations with limited extrapolation | Physics-constrained generalization |
| High-fidelity CFD (expensive, sparse coverage) | Rapid exploration of parameter space |
| Reduced-order models (miss cross-domain coupling) | Multi-physics reasoning |
| Optimization within known design space | Discovery of unknown regimes |
| Inherits 50 years of selection bias | Corrects for survivorship bias |

## Proposed Solution

AlphaCav is a **physics-native generative foundation model** trained to:

1. **Encode multi-physics relationships** spanning hydrodynamics, gas dynamics, and acoustics
2. **Generalize across parameter regimes** beyond explicit training coverage
3. **Identify non-obvious coupling pathways** invisible to single-physics analysis
4. **Generate hypotheses** for experimental validation
5. **Explore parameter space systematically** without human selection bias

### Core Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│    Physics-Native Generative Foundation Model for Acoustic      │
│         Regime Discovery in Gas-Supported Supercavitation       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌───────────┐ ┌───────────┐ ┌───────────┐ ┌───────────┐        │
│  │  Cavity   │ │    Gas    │ │  Acoustic │ │ Downstream│        │
│  │ Dynamics  │ │ Injection │ │   Source  │ │Disturbance│        │
│  │  Encoder  │ │  Encoder  │ │  Encoder  │ │  Encoder  │        │
│  └─────┬─────┘ └─────┬─────┘ └─────┬─────┘ └─────┬─────┘        │
│        │             │             │             │              │
│        └─────────────┴──────┬──────┴─────────────┘              │
│                             │                                   │
│                      ┌──────▼──────┐                            │
│                      │ Cross-Domain│                            │
│                      │  Attention  │                            │
│                      └──────┬──────┘                            │
│                             │                                   │
│                      ┌──────▼──────┐                            │
│                      │   Coupled   │                            │
│                      │   Physics   │                            │
│                      │ Transformer │                            │
│                      └──────┬──────┘                            │
│                             │                                   │
│           ┌─────────────────┼─────────────────┐                 │
│           │                 │                 │                 │
│    ┌──────▼──────┐   ┌──────▼──────┐   ┌──────▼──────┐          │
│    │   Regime    │   │  Mechanism  │   │  Hypothesis │          │
│    │     Map     │   │ Identifier  │   │  Generator  │          │
│    │  Generator  │   │             │   │             │          │
│    └─────────────┘   └─────────────┘   └─────────────┘          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

## Target Stakeholders

This problem sits at the intersection of multi-physics simulation, AI-driven scientific discovery, ocean acoustics, and undersea systems engineering. No single organization owns all of that capability.

### Navy Research Organizations
- Office of Naval Research
- NSWC Carderock
- NUWC Newport
- Naval Research Laboratory

### University-Affiliated Research Centers (UARCs)
- Penn State ARL (Garfield Thomas Water Tunnel)
- APL-UW (ocean acoustics expertise)
- JHU APL (undersea systems)

### DOE National Laboratories
- Sandia National Laboratories (multi-physics simulation)
- Lawrence Livermore National Laboratory (HPC, physics modeling)
- Pacific Northwest National Laboratory (AI for scientific discovery)
- Argonne National Laboratory (AI/ML, maritime focus)

## Anticipated Critiques

| Critique | Response |
|----------|----------|
| Data scarcity | Train on physics (PDEs), not data alone. Synthetic data from high-fidelity CFD. |
| Extrapolation limits | Physics-constrained latent space. Model generates only physically consistent states. |
| Validation gap | Discovery instrument feeds experimental validation. Reduces search space from infinite to tractable. |
| Inherited CFD errors | Multi-fidelity approach. Calibration against experimental benchmarks. |
| Black box concerns | Mechanism identification outputs. Attention weight analysis maps to physical parameters. |
| Just surrogate modeling | Different objective: discovery vs. acceleration. Explores unknown parameter space. |

**Strongest critique:** "You'll discover regimes that only exist in your numerical model, not in reality."

**Strongest response:** "That's why we validate experimentally. The model reduces the search space from infinite to tractable. Tunnel time validates what matters."

## Value Proposition

| Outcome | Impact |
|---------|--------|
| Acoustic regime map | First systematic characterization of achievable acoustic behaviors |
| Mechanism identification | Physical understanding of regime transitions |
| Experimental guidance | Prioritized hypotheses for validation campaigns |
| Unexplored parameter space | Discovery of regimes missed by 50 years of selection bias |

## Quick Links

- [Full White Paper](WHITEPAPER.md)
- [Executive Summary](docs/executive-summary.md)
- [Technical Architecture](docs/technical-architecture.md)
- [Survivorship Bias Case](examples/survivorship-bias.md)
- [Stakeholder Analysis](docs/stakeholder-analysis.md)

## Related Work

### Physics-Informed Machine Learning
- [Machine Learning in Acoustics (npj Acoustics 2025)](https://www.nature.com/articles/s44384-025-00021-w)
- [GenCFD: Generative AI for Fluids (2024)](https://arxiv.org/abs/2409.18359)
- [Physics-Informed Neural Networks Review](https://www.nature.com/articles/s42254-021-00314-5)

### NVIDIA Physics AI
- [PhysicsNeMo](https://developer.nvidia.com/physicsnemo) - Framework for physics AI models
- [Cosmos](https://www.nvidia.com/en-us/ai/cosmos/) - World foundation models for physical AI

### ONR Programs
- [Ocean Acoustics Program](https://www.onr.navy.mil/organization/departments/code-32/division-322/ocean-acoustics)
- [Ship Signatures Program](https://www.onr.navy.mil/organization/departments/code-33/division-331/ship-signatures)

## ONR Code Alignment

**Primary:** ONR Code 33, Ocean Acoustics

**Secondary:** ONR Code 32, Hydrodynamics and Cavitation Physics

This work treats acoustic regime discovery as a physics problem, using a foundation model architecture to reveal how cavity dynamics govern noise generation in gas-supported supercavitation. The approach is mechanism-oriented, discovery-focused, and non-operational.

## License

This proposal and associated documentation are provided under the [BSD 3-Clause License](LICENSE).

## Contact

**Author:** Keenan Williams  
**Email:** telesis001@icloud.com

---

**Star this repo if you find it valuable!**
