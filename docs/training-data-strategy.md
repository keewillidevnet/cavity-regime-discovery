# Training Data Strategy

## Overview

AlphaCav requires training data that spans the physics of cavity dynamics, gas injection, and acoustic emission. This document outlines the strategy for acquiring, generating, and curating training data.

The strategy explicitly addresses **survivorship bias** by ensuring training data covers parameter space systematically rather than following historical research priorities.

### Self-Supervised Learning Paradigm

Following the DINOv3 approach, AlphaCav employs self-supervised learning that does not require labeled regime classifications. The model learns rich representations from unlabeled CFD data by:

1. **Contrastive learning** across simulation snapshots to identify physically meaningful structure
2. **Physics-constrained reconstruction** that forces encoders to capture governing dynamics
3. **Multi-fidelity consistency** that aligns representations across simulation fidelity levels

This eliminates the annotation bottleneck. Regime labels are expensive (requiring expert analysis of each simulation) and potentially biased (reflecting historical assumptions about what constitutes distinct regimes). Self-supervised learning discovers structure from the data itself.

---

## Data Sources

### High-Fidelity CFD Simulation

Primary source for training data generation.

| Simulation Type | Physics Coverage | Cost (core-hours/case) | Role |
|-----------------|------------------|------------------------|------|
| URANS | Mean flow, cavity shape | 1,000-5,000 | Broad parameter coverage |
| LES | Unsteady dynamics, turbulence | 50,000-200,000 | Critical regime transitions |
| Coupled acoustic | Far-field noise prediction | 100,000-500,000 | Acoustic source validation |

### Experimental Data

Calibration and validation anchors.

| Source | Data Type | Purpose |
|--------|-----------|---------|
| Water tunnel experiments | Cavity visualization, pressure measurements | Cavity dynamics validation |
| Acoustic measurements | Hydrophone arrays, spectral content | Acoustic model calibration |
| Published literature | Archival experimental results | Benchmark coverage |

### Synthetic Data Augmentation

Physics-informed data augmentation to extend coverage.

| Method | Application |
|--------|-------------|
| Parameter perturbation | Explore variations around simulated cases |
| Physics-constrained interpolation | Fill gaps between simulation points |
| Reduced-order model sampling | Rapid generation of approximate solutions |

---

## Parameter Space Coverage

### Systematic Sampling

To correct for survivorship bias, the training data strategy employs systematic sampling rather than intuition-guided selection.

**Latin Hypercube Sampling** across:

| Parameter | Range | Discretization |
|-----------|-------|----------------|
| Flow velocity | 10-50 m/s | 20 levels |
| Gas injection rate | 0-0.1 kg/s | 15 levels |
| Cavitator geometry | 5 families | Continuous variation within family |
| Downstream disturbance frequency | 0-500 Hz | 25 levels |
| Ambient pressure | 1-10 atm | 10 levels |

### Adaptive Refinement

After initial training, active learning identifies regions requiring additional data:

1. High uncertainty regions in regime map
2. Regime boundaries with sharp transitions
3. Novel regimes predicted but not validated

---

## Multi-Fidelity Framework

### Fidelity Hierarchy

```
┌─────────────────────────────────────────┐
│           Coupled Acoustic LES          │  ← Highest fidelity, sparse coverage
│              (50+ cases)                │
├─────────────────────────────────────────┤
│              LES Unsteady               │  ← High fidelity, moderate coverage
│             (100+ cases)                │
├─────────────────────────────────────────┤
│            URANS Baseline               │  ← Moderate fidelity, broad coverage
│             (500+ cases)                │
├─────────────────────────────────────────┤
│         Reduced-Order Models            │  ← Low fidelity, dense coverage
│           (10,000+ cases)               │
└─────────────────────────────────────────┘
```

### Fidelity Weighting

The model learns to weight information appropriately across fidelity levels:

- High-fidelity data anchors physical accuracy
- Low-fidelity data provides parameter space coverage
- Cross-fidelity consistency enforced through auxiliary loss terms

---

## Data Quality Assurance

### Validation Checks

| Check | Purpose |
|-------|---------|
| Conservation verification | Ensure simulations satisfy mass/momentum/energy balance |
| Grid convergence | Verify spatial resolution adequacy |
| Temporal convergence | Confirm statistical stationarity for unsteady cases |
| Benchmark comparison | Validate against published experimental data |

### Outlier Detection

Automated screening for:

- Non-physical solutions (negative pressures, supersonic liquid velocities)
- Numerical artifacts (checkerboarding, odd-even decoupling)
- Unconverged simulations

---

## Data Pipeline

### Workflow

```
┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│  Parameter   │───▶│  Simulation  │───▶│    Post-     │
│  Sampling    │    │  Execution   │    │  Processing  │
└──────────────┘    └──────────────┘    └──────────────┘
                                               │
                                               ▼
┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│   Training   │◀───│   Feature    │◀───│   Quality    │
│   Dataset    │    │  Extraction  │    │  Assurance   │
└──────────────┘    └──────────────┘    └──────────────┘
```

### Feature Extraction

From raw simulation data, extract:

| Feature Category | Examples |
|------------------|----------|
| Cavity geometry | Length, diameter, shape parameters |
| Flow statistics | Mean velocity, turbulent kinetic energy |
| Spectral content | Power spectral density of pressure, cavity boundary |
| Acoustic metrics | SPL, kurtosis, crest factor |

---

## Addressing Survivorship Bias

Traditional training data would inherit the selection bias of historical research. AlphaCav's data strategy mitigates this through:

1. **Systematic sampling**: Latin Hypercube covers parameter space uniformly
2. **Active learning**: Identifies and fills under-explored regions
3. **Physics constraints**: Enables extrapolation beyond training distribution
4. **Multi-fidelity coverage**: Low-fidelity models reach regions too expensive for high-fidelity simulation

The goal is to ensure the model sees parameter combinations that researchers never thought to simulate because they didn't look promising based on existing theory.

---

## Computational Resources

### Estimated Requirements

| Phase | Compute | Storage | Timeline |
|-------|---------|---------|----------|
| Initial URANS campaign | 500K core-hours | 5 TB | 2 months |
| LES refinement | 2M core-hours | 20 TB | 4 months |
| Coupled acoustic | 5M core-hours | 30 TB | 6 months |
| **Total Phase I** | **7.5M core-hours** | **55 TB** | **12 months** |

### Platform Options

- DoD HPCMP allocation (Navy DSRC: Nautilus, Narwhal)
- Additional DSRC resources as needed (AFRL, ARL, ERDC)
