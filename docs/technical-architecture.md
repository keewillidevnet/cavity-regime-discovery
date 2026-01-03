# Technical Architecture

## AlphaCav Model Design

### Design Philosophy

AlphaCav is designed for **mechanism discovery**, structured around the premise that distinct acoustic regimes emerge from identifiable physical mechanisms. The model learns to recognize and predict these regimes by encoding the underlying physics rather than fitting input-output mappings.

This approach corrects for the survivorship bias inherent in traditional research: the model explores parameter space based on physical constraints, not historical research priorities.

### Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│    Physics-Native Generative Foundation Model for Acoustic      │
│         Regime Discovery in Gas-Supported Supercavitation       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌───────────┐ ┌───────────┐ ┌───────────┐ ┌───────────┐       │
│  │  Cavity   │ │    Gas    │ │  Acoustic │ │ Downstream│       │
│  │ Dynamics  │ │ Injection │ │   Source  │ │Disturbance│       │
│  │  Encoder  │ │  Encoder  │ │  Encoder  │ │  Encoder  │       │
│  └─────┬─────┘ └─────┬─────┘ └─────┬─────┘ └─────┬─────┘       │
│        │             │             │             │              │
│        └─────────────┴──────┬──────┴─────────────┘              │
│                             │                                   │
│                      ┌──────▼──────┐                            │
│                      │ Cross-Domain │                            │
│                      │  Attention   │                            │
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
│    ┌──────▼──────┐   ┌──────▼──────┐   ┌──────▼──────┐         │
│    │   Regime    │   │  Mechanism  │   │  Hypothesis │         │
│    │     Map     │   │ Identifier  │   │  Generator  │         │
│    │  Generator  │   │             │   │             │         │
│    └─────────────┘   └─────────────┘   └─────────────┘         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Architecture Rationale

The architecture separates physics encoding from cross-domain reasoning:

- **Domain-specific encoders** learn representations of individual physical processes
- **Cross-domain attention** identifies coupling relationships between domains
- **Coupled physics transformer** reasons about multi-physics interactions
- **Task-specific heads** generate outputs tailored to discovery objectives

This separation allows the model to leverage domain-specific simulation data while learning emergent behaviors that arise from physics coupling.

### Foundation Model Precedent: DINOv3

The AlphaCav architecture draws inspiration from Meta's DINOv3 (2025), which demonstrates that self-supervised foundation models can achieve state-of-the-art performance without labeled training data.

**Key DINOv3 principles adopted:**

| DINOv3 Principle | AlphaCav Implementation |
|------------------|------------------------|
| Frozen backbone + lightweight adapters | Physics encoders serve multiple tasks without retraining |
| Self-supervised learning without labels | Learn from CFD data without regime annotations |
| Dense feature extraction | Dense field representations capture physics structure |
| Single forward pass serves multiple tasks | Regime mapping, mechanism ID, hypothesis generation share backbone |

DINOv3's success in satellite imagery, where manual annotation is impractical at scale, validates the approach for cavitation physics, where experimental regime labels are equally scarce. The model learns structure from data, not from human-provided labels.

---

## Physics Encoders

### Cavity Dynamics Encoder

Encodes the physics governing cavity formation, shape, and temporal evolution.

| Physics Phenomenon | Representation |
|--------------------|----------------|
| Cavity detachment and formation | Pressure coefficient fields, detachment location |
| Shape and length scaling | Non-dimensional cavity geometry parameters |
| Stability regimes | Temporal variance metrics, spectral content of cavity boundary |
| Re-entrant jet dynamics | Closure behavior classification, jet penetration depth |

**Governing relationships:** Modified Rayleigh-Plesset dynamics, potential flow cavity models, empirical closure correlations.

### Gas Injection Encoder

Encodes ventilation gas behavior and its influence on cavity properties.

| Physics Phenomenon | Representation |
|--------------------|----------------|
| Injection rate effects | Non-dimensional gas flow coefficient |
| Spatial distribution | Injection location encoding, multi-port configurations |
| Temporal modulation | Spectral representation of injection waveform |
| Gas entrainment | Entrainment efficiency metrics |

**Governing relationships:** Compressible gas dynamics, mixing layer physics, entrainment correlations.

### Acoustic Source Encoder

Encodes mechanisms that generate acoustic emissions.

| Physics Phenomenon | Representation |
|--------------------|----------------|
| Cavity collapse events | Impulsivity metrics, collapse energy estimates |
| Interface oscillations | Spectral content of cavity boundary motion |
| Turbulent sources | Turbulent kinetic energy fields, integral length scales |
| Bubble dynamics | Bubble size distributions, collective oscillation modes |

**Governing relationships:** Ffowcs Williams-Hawkings acoustic analogy, Lighthill sources, bubble acoustics.

### Downstream Disturbance Encoder

Encodes flow perturbations aft of cavity closure.

| Physics Phenomenon | Representation |
|--------------------|----------------|
| Disturbance frequency | Spectral encoding of perturbation content |
| Amplitude | Normalized momentum flux |
| Spatial structure | Modal decomposition of disturbance field |
| Interaction with closure | Closure sensitivity metrics |

**Governing relationships:** Wake dynamics, vortex shedding, flow instability mechanisms.

---

## Cross-Domain Attention

The cross-domain attention layer identifies coupling relationships between physics domains that would be invisible to single-physics analysis.

### Attention Mechanism

For each domain pair (i, j), compute attention weights:

```
A_ij = softmax(Q_i · K_j^T / sqrt(d_k))
```

Where:
- Q_i: Query vectors from domain i encoder
- K_j: Key vectors from domain j encoder
- d_k: Key dimension for scaling

### Coupling Pathways

The attention mechanism learns to identify:

| Coupling | Physical Interpretation |
|----------|------------------------|
| Cavity → Acoustic | How cavity dynamics drive noise generation |
| Gas → Cavity | How injection modifies cavity behavior |
| Disturbance → Closure | How downstream perturbations affect cavity stability |
| Closure → Acoustic | How closure dynamics generate impulsive events |

---

## Generative Outputs

### Regime Map Generator

Produces continuous maps of acoustic behavior across parameter space.

**Output format:** Multi-dimensional field mapping parameter combinations to acoustic regime classifications with confidence bounds.

**Use case:** Identify regime boundaries, locate unexplored regions, guide experimental planning.

### Mechanism Identifier

Ranks physical mechanisms by contribution to observed acoustic behavior.

**Output format:** Ordered list of mechanisms with relative importance weights and physical interpretability mappings.

**Use case:** Understand why specific regimes exhibit particular acoustic characteristics.

### Hypothesis Generator

Produces testable predictions for experimental validation.

**Output format:** Specific parameter combinations predicted to exhibit novel acoustic behavior, with confidence intervals and suggested validation approaches.

**Use case:** Direct experimental resources toward high-value regions of parameter space.

---

## Physics Constraints

The model enforces physical consistency through:

1. **Conservation constraints**: Mass, momentum, energy conservation embedded in loss function
2. **Thermodynamic constraints**: Entropy production, equation of state consistency
3. **Scaling relationships**: Known non-dimensional scaling preserved across regimes
4. **Causality**: Temporal ordering of cause-effect relationships

These constraints ensure the model cannot generate physically impossible regimes, addressing the critique that discoveries might be numerical artifacts.

---

## Computational Requirements

### Training

| Component | Requirement |
|-----------|-------------|
| Platform | Navy DSRC Nautilus (DoD HPCMP) |
| GPU resources | 8-32 A100 GPUs (Nautilus AI/ML nodes) |
| Training duration | 1-4 weeks |
| Storage | 10-50 TB for training data |

### Inference

Trained model inference is lightweight, enabling rapid exploration of parameter space once training is complete. Single-GPU inference supports interactive regime exploration.

---

## Comparison to Existing Approaches

| Aspect | Surrogate Models | AlphaCav |
|--------|------------------|----------|
| Objective | Accelerate known simulations | Discover unknown regimes |
| Training | Interpolate within data | Generalize via physics |
| Output | Point predictions | Regime maps with uncertainty |
| Bias | Inherits training data bias | Corrects for survivorship bias |
