# Physics-Native Generative Foundation Model for Acoustic Regime Discovery in Gas-Supported Supercavitation

**A Proposal for Accelerating Mechanism-Oriented Research in Cavitation Acoustics**

---

**ONR Code Alignment**

Primary: ONR Code 33, Ocean Acoustics  
Secondary: ONR Code 32, Hydrodynamics and Cavitation Physics

---

## Executive Summary

Gas-supported supercavitation generates intense and highly variable acoustic emissions whose physical origins remain poorly understood. Current experimental approaches are constrained by the high dimensionality of the parameter space governing cavity dynamics, gas injection, geometry, and downstream flow conditions. Exhaustive parametric exploration is impractical, leaving significant regions of acoustic regime space unexplored.

This white paper proposes the development of a **physics-native generative foundation model** trained on high-fidelity simulation data to encode the coupled dynamics governing cavity formation, stability, and acoustic emission. Foundation model architectures enable generalization across parameter regimes and discovery of non-obvious physical coupling pathways that single-physics models cannot capture.

The proposed model would serve as a **discovery instrument**, generating testable hypotheses about acoustic regime boundaries, identifying dominant physical mechanisms, and guiding experimental resource allocation toward high-value regions of parameter space.

Recent advances in physical AI demonstrate that generative foundation models trained on simulation data can encode complex multi-physics dynamics and generalize beyond training distributions. This work applies analogous architectural principles to the problem of acoustic regime discovery in cavitating flows.

---

## Table of Contents

1. [Motivation and Problem Statement](#1-motivation-and-problem-statement)
2. [Research Gap and Opportunity](#2-research-gap-and-opportunity)
3. [Technical Approach](#3-technical-approach)
4. [Model Architecture](#4-model-architecture)
5. [Physics Encoders](#5-physics-encoders)
6. [Generative Outputs](#6-generative-outputs)
7. [Training Data Strategy](#7-training-data-strategy)
8. [Downstream Disturbance Abstraction](#8-downstream-disturbance-abstraction)
9. [Hybrid Physics-Statistical Framework](#9-hybrid-physics-statistical-framework)
10. [Expected Outcomes](#10-expected-outcomes)
11. [Anticipated Critiques and Responses](#11-anticipated-critiques-and-responses)
12. [Relevance to ONR](#12-relevance-to-onr)
13. [Phase II Transition Path](#13-phase-ii-transition-path)
14. [Terminology Framework](#14-terminology-framework)

---

## 1. Motivation and Problem Statement

Supercavitating flows produce acoustic signatures dominated by impulsive events associated with cavity instability and collapse. These signatures exhibit high variability across operating conditions, yet the physical mechanisms governing transitions between acoustically quiescent and impulsive regimes remain poorly characterized.

The parameter space governing acoustic emission in gas-supported supercavitation includes:

- Flow velocity and Reynolds number
- Gas injection rate, location, and temporal modulation
- Cavitator geometry and closure configuration
- Downstream flow disturbance characteristics
- Depth and ambient pressure conditions

Traditional experimental campaigns sample this space sparsely due to facility costs and time constraints. Computational fluid dynamics (CFD) can extend coverage but remains expensive for the unsteady, multiphase, compressible simulations required to capture acoustic source mechanisms.

The result is a fragmented understanding of acoustic regime boundaries and the physical pathways that connect cavity dynamics to far-field noise.

---

## 2. Research Gap and Opportunity

### 2.1 Current Limitations

Existing approaches to cavitation acoustics fall into three categories:

| Approach | Capability | Limitation |
|----------|------------|------------|
| Empirical correlations | Fast evaluation | Limited extrapolation beyond training data |
| High-fidelity CFD | Detailed physics | Computationally prohibitive for parametric exploration |
| Reduced-order models | Efficient prediction | Encode specific physics; miss cross-domain coupling |

None of these approaches are well-suited to **discovery** of acoustic regimes across high-dimensional parameter spaces.

### 2.2 The Foundation Model Opportunity

Foundation models trained on diverse simulation data can:

1. **Encode multi-physics relationships** spanning hydrodynamics, gas dynamics, and acoustics
2. **Generalize across parameter regimes** beyond explicit training coverage
3. **Identify non-obvious coupling pathways** invisible to single-physics analysis
4. **Generate hypotheses** for experimental validation

The key insight is that a model trained on sufficient simulation diversity can internalize physical constraints and extrapolate to unexplored regions of parameter space in ways that purely data-driven surrogate models cannot.

### 2.3 Architectural Precedent: Self-Supervised Vision Foundation Models

Meta's DINOv3 (2025) demonstrates that self-supervised foundation models can achieve state-of-the-art performance across diverse tasks without labeled training data. Key findings relevant to AlphaCav:

| DINOv3 Finding | AlphaCav Implication |
|----------------|---------------------|
| Frozen backbone + lightweight adapters outperform fine-tuned specialists | Physics encoders can serve multiple downstream tasks (regime mapping, mechanism ID, hypothesis generation) |
| Self-supervised learning scales to 1.7B images without labels | Scales to massive synthetic CFD datasets where regime labels don't exist |
| Dense pixel-level features capture scene structure | Dense field-level features can capture flow physics structure |
| Satellite imagery backbone enables applications where annotation is impractical | Cavitation physics backbone enables discovery where experimental data is sparse |

DINOv3's success in domains where labeling is prohibitively expensive (satellite imagery, medical imaging) validates the core AlphaCav premise: foundation models can learn powerful representations from unlabeled physics data, enabling discovery in parameter spaces that human intuition never explored.

### 2.4 Prior Work and Novel Contribution

#### What Exists

Machine learning has been applied to cavitation and underwater acoustics in several contexts:

| Domain | Existing Approaches | Objective |
|--------|---------------------|-----------|
| Cavitation intensity recognition | CNNs, autoencoders, hierarchical networks | Classify known intensity levels in pumps, valves, turbines |
| Cavitation regime classification | Physics-informed neural networks | Binary classification (stable vs. transient) |
| Propeller cavitation noise | Generative adversarial networks | Synthesize realistic acoustic signatures |
| Underwater target recognition | CNNs, RNNs, autoencoders | Classify ships and marine sources |
| Machine condition monitoring | Self-supervised anomaly detection (DCASE) | Detect deviations from normal operation |

These approaches have demonstrated value for their respective tasks. However, they share common limitations: they classify within predefined categories, require labeled training data, and operate on single-physics representations.

#### What Does Not Exist

No prior work has addressed:

1. **Foundation model architecture for physical acoustics**: Existing models are task-specific classifiers, not generalizable backbones with frozen encoders and lightweight adapters for multiple downstream tasks
2. **Self-supervised pre-training on multi-physics CFD data**: Current approaches rely on labeled experimental data or single-physics simulations
3. **Regime discovery as an objective**: All existing work classifies known categories; none explores parameter space to identify previously unknown acoustic regimes
4. **Multi-physics integration**: No model jointly encodes cavity dynamics, gas injection physics, and acoustic emission within a unified architecture

#### Novel Contribution

AlphaCav represents the **first physics-native foundation model for acoustic regime discovery in fluid dynamics**. The novelty arises from the combination of:

| Contribution | Distinction from Prior Work |
|--------------|----------------------------|
| Foundation model architecture | Frozen physics encoders + task-specific adapters, not single-purpose classifiers |
| Self-supervised learning on CFD | Learns from unlabeled simulation data; no regime annotations required |
| Discovery objective | Explores unknown parameter space to find regimes human intuition never visited |
| Multi-physics integration | Jointly encodes cavity dynamics, gas injection, acoustic sources, and downstream disturbances |
| Physics-constrained latent space | Representations satisfy governing equations; cannot generate physically inconsistent states |

This combination enables a fundamentally different capability: systematic exploration of acoustic regime boundaries across high-dimensional parameter spaces, correcting for 50 years of survivorship bias in supercavitation research.

---

## 3. Technical Approach

### 3.1 Core Philosophy

The proposed model is designed for **mechanism discovery**, structured around the premise that distinct acoustic regimes emerge from identifiable physical mechanisms. The model learns to recognize and predict these regimes by encoding the underlying physics rather than fitting input-output mappings.

### 3.2 Organizing Principles

The technical approach treats the following as first-order drivers of acoustic emission:

1. Cavity stability and persistence
2. Impulsivity associated with cavity collapse and reformation
3. Geometry-driven cavity closure behavior
4. Gas injection rate and temporal structure
5. Sensitivity of cavity closure to downstream flow disturbances

Each mechanism maps to specific encoder modules within the model architecture.

---

## 4. Model Architecture

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

### 4.1 Architecture Rationale

The architecture separates physics encoding from cross-domain reasoning:

- **Domain-specific encoders** learn representations of individual physical processes
- **Cross-domain attention** identifies coupling relationships between domains
- **Coupled physics transformer** reasons about multi-physics interactions
- **Task-specific heads** generate outputs tailored to discovery objectives

This separation allows the model to leverage domain-specific simulation data while learning emergent behaviors that arise from physics coupling.

Following the DINOv3 paradigm, the physics encoders are designed to produce rich, dense representations that can serve multiple downstream tasks through lightweight adapters. This enables a single forward pass through the backbone to support regime mapping, mechanism identification, and hypothesis generation simultaneously, reducing inference cost for deployment scenarios requiring multiple predictions.

---

## 5. Physics Encoders

### 5.1 Cavity Dynamics Encoder

Encodes the physics governing cavity formation, shape, and temporal evolution.

| Physics Phenomenon | Representation |
|--------------------|----------------|
| Cavity detachment and formation | Pressure coefficient fields, detachment location |
| Shape and length scaling | Non-dimensional cavity geometry parameters |
| Stability regimes | Temporal variance metrics, spectral content of cavity boundary |
| Closure dynamics | Re-entrant jet characteristics, twin-vortex shedding |
| Collapse and reformation | Impulsive event timing, cavity volume time series |

Training data: Unsteady multiphase CFD capturing cavity evolution across parameter sweeps.

### 5.2 Gas Injection Encoder

Encodes the influence of gas supply on cavity sustainment and stability.

| Physics Phenomenon | Representation |
|--------------------|----------------|
| Injection rate effects | Non-dimensional gas entrainment coefficient |
| Injection location | Axial and circumferential distribution |
| Temporal modulation | Frequency content and duty cycle of gas supply |
| Gas dissolution and entrainment | Mass transfer coefficients, void fraction fields |
| Pressure-flow coupling | Supply system impedance characteristics |

Training data: Parametric simulations varying injection configuration and temporal structure.

### 5.3 Acoustic Source Encoder

Encodes the mechanisms by which cavity dynamics generate acoustic radiation.

| Source Type | Physical Mechanism |
|-------------|-------------------|
| Monopole | Cavity volume oscillation, collapse events |
| Dipole | Unsteady forces on cavitator and closure region |
| Quadrupole | Turbulent fluctuations in shear layers and wake |

| Acoustic Observable | Representation |
|--------------------|----------------|
| Broadband sound pressure level | Relative SPL integrated across frequency bands |
| Spectral content | Power spectral density, spectral slope |
| Impulsivity | Kurtosis, crest factor, peak-to-RMS ratio |
| Temporal structure | Inter-event timing, burst statistics |

Training data: Coupled CFD-acoustic simulations with Ffowcs Williams-Hawkings or direct acoustic computation.

### 5.4 Downstream Disturbance Encoder

Encodes the influence of downstream flow perturbations on cavity stability and acoustic emission.

| Disturbance Characteristic | Representation |
|---------------------------|----------------|
| Frequency content | Normalized frequency (Strouhal number based on cavity length) |
| Spectral structure | Narrowband vs. broadband, harmonic content |
| Spatial pattern | Axisymmetric, helical modes, azimuthal wave number |
| Amplitude | Normalized momentum flux perturbation |
| Temporal modulation | Amplitude envelope, intermittency |

Training data: Simulations with parametrically varied downstream boundary conditions representing abstract disturbance sources.

---

## 6. Generative Outputs

The model produces three categories of output designed to accelerate discovery:

### 6.1 Regime Map Generator

Generates maps of acoustic emission regimes across parameter space.

**Outputs:**
- Boundaries between acoustically quiescent and impulsive regimes
- Sensitivity gradients indicating which parameters most strongly influence regime transitions
- Operating corridors with favorable acoustic characteristics
- Uncertainty quantification for regime boundary locations

**Use case:** Guide experimental campaigns toward regime boundaries where the highest-value measurements can be obtained.

### 6.2 Mechanism Identifier

Isolates dominant physical pathways linking cavity dynamics to acoustic emission.

**Outputs:**
- Ranked importance of physical mechanisms across parameter space
- Coupling pathway identification (e.g., gas injection modulation driving closure instability driving impulsive collapse)
- Regions where single-physics models are sufficient vs. where coupling effects dominate

**Use case:** Focus modeling and instrumentation efforts on the mechanisms that matter most in each regime.

### 6.3 Hypothesis Generator

Produces physics-grounded conjectures for experimental validation.

**Outputs:**
- Testable predictions about acoustic behavior in unexplored parameter regions
- Suggested parameter combinations for regime boundary exploration
- Anomaly flags indicating where model uncertainty is high and experimental data would be most valuable

**Use case:** Transform the model from a passive predictor into an active participant in the scientific discovery process.

---

## 7. Training Data Strategy

### 7.1 Data Sources

| Source | Content | Role |
|--------|---------|------|
| High-fidelity CFD (RANS, URANS) | Mean cavity shape, steady-state behavior | Baseline physics encoding |
| Large Eddy Simulation (LES) | Unsteady cavity dynamics, turbulent fluctuations | Instability and spectral content |
| Direct Numerical Simulation (DNS) | Resolved acoustic sources (limited cases) | Ground truth for acoustic mechanisms |
| Coupled CFD-Acoustic (FW-H) | Far-field acoustic predictions | Source-to-signature mapping |
| Experimental validation data | Water tunnel and tow tank measurements | Model calibration and uncertainty reduction |

### 7.2 Simulation Campaign Design

Training data generation follows a structured campaign:

1. **Baseline sweeps:** Systematic variation of single parameters (velocity, gas rate, geometry) with others fixed
2. **Interaction studies:** Factorial designs exploring parameter combinations
3. **Regime boundary refinement:** Adaptive sampling near identified transitions
4. **Disturbance sensitivity:** Parametric downstream perturbation studies

### 7.3 Physics-Constrained Data Augmentation

Synthetic data augmentation respects physical constraints:

- Conservation laws (mass, momentum, energy)
- Scaling relationships (cavitation number, Froude number, Reynolds number)
- Causality (downstream disturbances cannot affect upstream cavity detachment)

This allows the model to learn from augmented data without violating fundamental physics.

---

## 8. Downstream Disturbance Abstraction

A critical element of the research framing is the treatment of downstream disturbances as abstract parameters rather than specific hardware configurations.

### 8.1 Abstraction Rationale

Cavity closure regions are sensitive to downstream flow perturbations. These perturbations can arise from many sources, but the acoustic response depends on disturbance characteristics rather than source identity.

By parameterizing disturbances abstractly, the model can:

- Investigate disturbance sensitivity without reference to specific hardware
- Identify universal relationships between disturbance characteristics and acoustic response
- Generate insights applicable across multiple physical configurations

### 8.2 Disturbance Parameter Space

| Parameter | Range | Physical Interpretation |
|-----------|-------|------------------------|
| Normalized frequency | 0.1 to 10 (St based on cavity length) | Ratio of disturbance timescale to cavity transit time |
| Spectral bandwidth | Narrowband to broadband | Coherence of disturbance structure |
| Azimuthal mode | m = 0, 1, 2, ... | Spatial symmetry of disturbance pattern |
| Amplitude | 0 to 20% of mean momentum flux | Strength of perturbation |
| Intermittency | Continuous to highly intermittent | Temporal structure of disturbance |

### 8.3 Example Abstraction

A periodic downstream disturbance at a specific normalized frequency represents any physical source that produces periodic perturbations at that frequency. The model learns how such disturbances affect cavity stability and acoustic emission without encoding assumptions about what generates them.

---

## 9. Hybrid Physics-Statistical Framework

### 9.1 Integration with Reduced-Order Models

The foundation model operates in conjunction with reduced-order physical models that capture established scaling relationships.

**Reduced-order model role:**
- Predict nominal cavity behavior (mean length, closure location)
- Provide physics-based expectations against which observations are compared
- Define the baseline from which residuals are computed

**Foundation model role:**
- Learn systematic deviations from reduced-order predictions
- Identify unmodeled physics revealed by structured residuals
- Generalize across parameter space where reduced-order models lose validity

### 9.2 Bayesian Residual Inference

A Bayesian framework quantifies uncertainty and identifies structured residual behavior:

- **Posterior distributions** over acoustic observables given input parameters
- **Uncertainty bounds** that reflect both data sparsity and model limitations
- **Anomaly detection** flagging observations inconsistent with learned physics

Persistent residual structure indicates:
- Acoustic regime transitions
- Previously uncharacterized instability pathways
- Regions where additional physics must be incorporated

### 9.3 Model-Experiment Feedback Loop

The hybrid framework enables an iterative discovery process:

1. Model predicts acoustic behavior and uncertainty across parameter space
2. Experiments target high-uncertainty regions and predicted regime boundaries
3. New data updates model, refining predictions and reducing uncertainty
4. Cycle repeats, progressively mapping acoustic regime space

---

## 10. Expected Outcomes

### 10.1 Phase I Deliverables

Phase I emphasizes knowledge generation:

| Deliverable | Description |
|-------------|-------------|
| Acoustic regime map | Initial mapping of regime boundaries across primary parameter dimensions |
| Mechanism ranking | Quantitative assessment of mechanism importance by regime |
| Coupling pathway catalog | Identified cross-domain interactions governing acoustic emission |
| Hypothesis set | Experimentally testable predictions for Phase II validation |
| Model architecture | Documented foundation model design and training methodology |
| Uncertainty characterization | Quantified confidence bounds on regime predictions |

### 10.2 Scientific Contributions

- First systematic application of foundation model architectures to cavitation acoustics
- Quantitative linkage between cavity stability metrics and acoustic observables
- Statistical characterization of precursors to impulsive cavity collapse
- Discovery of non-obvious coupling pathways through cross-domain attention analysis

### 10.3 Explicit Non-Goals

Phase I does not pursue:

- Performance optimization for any specific configuration
- System integration or operational assessment
- Design guidance for hardware development
- Prediction of absolute acoustic levels (focus remains on relative regime behavior)

---

## 11. Anticipated Critiques and Responses

This section addresses foreseeable methodological critiques and articulates the principled responses embedded in the proposed approach.

### 11.1 Data Scarcity

**Critique:** Generative models require massive training datasets. High-fidelity supercavitation acoustic data barely exists in sufficient quantity.

**Response:** The proposed approach trains on physics, not data alone. Physics-informed neural network architectures constrain the learned representations to satisfy governing equations. Synthetic training data generated from high-fidelity CFD provides coverage across parameter space. The model learns physical relationships, not input-output mappings, enabling generalization from limited empirical data.

Meta's DINOv3 (2025) provides direct precedent: self-supervised foundation models achieve state-of-the-art performance in domains where labeled data is scarce or nonexistent. DINOv3's satellite imagery backbone, trained without manual annotations, achieves exceptional performance on downstream tasks like canopy height estimation. AlphaCav applies the same paradigm to physics data, where regime labels are equally unavailable but physical constraints provide analogous structure for self-supervised learning.

### 11.2 Extrapolation Beyond Training Distribution

**Critique:** Neural networks fail when extrapolating outside their training distribution. Novel regimes are, by definition, outside the distribution of known configurations.

**Response:** The physics-native architecture constrains the latent space to physically consistent states. The model cannot generate regimes that violate conservation laws or thermodynamic constraints. This is fundamentally different from unconstrained data-driven models. The generative process explores within the manifold of physical possibility, not arbitrary feature space.

### 11.3 Validation Gap

**Critique:** If the model discovers a novel regime, how can it be validated without expensive experimental campaigns? The approach risks chasing computational phantoms.

**Response:** This critique identifies precisely why the model is framed as a *discovery instrument* rather than a predictive tool. The output is not a final answer but a prioritized set of hypotheses for experimental validation. By narrowing the search space from infinite to tractable, the model makes experimental campaigns feasible. Partnership with facilities such as Penn State ARL's Garfield Thomas Water Tunnel and NSWC Carderock's acoustic test facilities provides the validation pathway.

### 11.4 Inherited CFD Errors

**Critique:** Training on CFD means inheriting all modeling errors from turbulence closures, multiphase treatments, and acoustic analogies. Garbage in, garbage out.

**Response:** The training data strategy employs a multi-fidelity approach. High-fidelity LES and direct acoustic simulation are used for critical regions and validation anchors. URANS provides broader parameter coverage with known limitations. The model learns to weight information appropriately across fidelity levels. Additionally, calibration against existing experimental benchmarks identifies and corrects systematic biases.

### 11.5 Interpretability and Scientific Value

**Critique:** Neural networks are black boxes. Even if the model identifies a novel regime, the inability to explain *why* undermines scientific value. Is this actually science?

**Response:** The architecture includes explicit mechanism identification outputs. Discovered regimes are mapped back to governing physical parameters through attention weight analysis and sensitivity studies. The model does not replace physical understanding; it accelerates the discovery of phenomena that merit detailed investigation. Discovery first, mechanism elucidation second. This mirrors historical scientific practice where phenomena were often observed before being explained.

### 11.6 Novelty Relative to Surrogate Modeling

**Critique:** This is neural operator surrogate modeling with different marketing. The approach offers nothing fundamentally new.

**Response:** Surrogate models accelerate evaluation of *known* simulation configurations. They interpolate within established parameter spaces. The proposed model explores *unknown* parameter space to discover regime boundaries that have never been simulated or observed. The objective function is discovery, not acceleration. This represents a fundamentally different use of machine learning in scientific computing.

### 11.7 Multi-Scale Challenge

**Critique:** Supercavitation acoustics spans bubble dynamics (microscale), cavity behavior (mesoscale), and acoustic propagation (macroscale). No single model architecture can faithfully represent all scales simultaneously.

**Response:** The hierarchical encoder architecture explicitly separates scale-specific physics. Cross-scale coupling is learned through attention mechanisms rather than brute-force resolution of all scales. Additionally, the model is designed to identify *candidate* regimes warranting detailed multi-scale follow-up, not to replace high-fidelity multi-scale simulation entirely.

### 11.8 Survivorship Bias in Training Data

**Critique:** Training data comes from simulations of configurations that researchers chose to study. The model inherits the same selection bias that limits current understanding.

**Response:** This critique is valid for purely data-driven approaches. The physics-native architecture mitigates this risk by learning governing relationships rather than correlations in observed data. When the model generates novel configurations, it extrapolates based on physical constraints, not historical research priorities. Active learning strategies in Phase II would further address this by systematically exploring under-sampled regions of parameter space.

### 11.9 Summary

| Critique | Core Concern | Mitigation Strategy |
|----------|--------------|---------------------|
| Data scarcity | Insufficient training examples | Physics-informed architecture, synthetic data |
| Extrapolation limits | Failure outside training distribution | Physics-constrained latent space |
| Validation gap | Unverifiable predictions | Discovery instrument framing, experimental partnerships |
| CFD inheritance | Modeling errors propagate | Multi-fidelity training, benchmark calibration |
| Interpretability | Black box concerns | Mechanism identification outputs, attention analysis |
| Novelty claims | Just surrogate modeling | Different objective: discovery vs. acceleration |
| Multi-scale challenge | Scale separation | Hierarchical encoders, candidate identification |
| Survivorship bias | Biased training data | Physics-native extrapolation, active learning |

The strongest critique remains: *"You will discover regimes that exist only in your numerical model, not in physical reality."*

The strongest response: *"That is why we validate experimentally. The model reduces the search space from infinite to tractable. Experimental facilities validate what matters."*

---

## 12. Relevance to ONR

### 12.1 Code 33 Relevance

This effort aligns directly with Code 33 interests in:

| Interest Area | Alignment |
|---------------|-----------|
| Hydrodynamic noise source characterization | Primary focus on acoustic source mechanisms |
| Regime mapping | Systematic identification of acoustic emission regimes |
| Physics-based interpretation | Foundation model encodes physical relationships |
| Uncertainty-aware analysis | Bayesian framework with explicit confidence bounds |
| Discovery science | Hypothesis generation for experimental validation |

The emphasis on acoustic observables as primary dependent variables and mechanism-level understanding supports Code 33 priorities in undersea acoustics research.

### 12.2 Code 32 Relevance

Secondary relevance is provided through:

- Investigation of cavitation physics and cavity closure dynamics
- Geometry effects on cavity stability
- Gas injection influence on cavity behavior

These hydrodynamic elements support physical understanding without driving the research objectives.

### 12.3 Framing Statement

This work treats acoustic regime discovery as a physics problem, using a foundation model architecture to reveal how cavity dynamics govern noise generation in gas-supported supercavitation. The approach is mechanism-oriented, discovery-focused, and non-operational.

---

## 13. Phase II Transition Path

### 13.1 Extended Scope

Phase II would expand the acoustic regime map through:

- Broader parameter coverage including additional geometry families
- Refined downstream disturbance characterization
- Higher-fidelity training data in regions identified as high-value by Phase I
- Experimental validation campaigns guided by model predictions

### 13.2 Model Enhancement

Phase II model development would include:

- Increased encoder depth for finer physics resolution
- Additional physics domains (e.g., structural acoustics, bubble dynamics)
- Active learning for efficient experimental guidance
- Transfer learning to related cavitating flow configurations

### 13.3 Sustained Discovery Focus

Phase II maintains emphasis on mechanism discovery and acoustic interpretation. The foundation model remains a scientific instrument for understanding physical relationships rather than an engineering tool for system development.

---

## 14. Terminology Framework

This research employs mechanism-oriented terminology to maintain focus on acoustic physics rather than system integration.

### 14.1 Terminology Mapping

| Mechanism-Oriented Term | Physical Meaning |
|------------------------|------------------|
| Axisymmetric test body | Cavitating body used in experiments |
| Prescribed velocity | Controlled flow speed in experimental configuration |
| Downstream disturbance source | Any source of flow perturbation aft of cavity closure |
| Periodic downstream disturbance | Disturbance with discrete frequency content |
| Normalized momentum flux | Non-dimensional measure of disturbance amplitude |
| Acoustically quiescent regime | Operating condition with low impulsive acoustic emission |
| Regime boundary | Parameter combination where acoustic character transitions |
| Cavity closure sensitivity | Response of closure dynamics to perturbations |

### 14.2 Framing Principles

- **Parameters over hardware:** Describe disturbances by physical characteristics, not sources
- **Regimes over performance:** Characterize acoustic behavior by mechanism, not merit
- **Discovery over design:** Generate hypotheses for validation, not specifications for implementation
- **Physics over application:** Understand relationships, not optimize outcomes

---

## Appendix A: Architectural Comparison

| Aspect | Physical AI Foundation Models | Proposed Cavitation Acoustics Model |
|--------|-------------------------------|-------------------------------------|
| Training data | Video, simulation of physical interaction | CFD, LES, coupled acoustic simulation |
| Physics domains | Rigid body dynamics, contact, friction | Multiphase flow, compressibility, acoustics |
| Generative output | Action sequences, world state predictions | Regime maps, mechanism rankings, hypotheses |
| Validation | Task completion, physical plausibility | Experimental acoustic measurements |
| Application | Control, planning, prediction | Discovery, hypothesis generation, experiment guidance |

---

## Appendix B: Acoustic Observables and Instrumentation

### B.1 Primary Acoustic Metrics

| Metric | Definition | Physical Interpretation |
|--------|------------|------------------------|
| Relative broadband SPL | Integrated sound pressure level across frequency band | Overall acoustic intensity |
| Power spectral density | Sound pressure per unit frequency | Frequency distribution of acoustic energy |
| Spectral slope | Rate of PSD decay with frequency | Source mechanism indicator |
| Kurtosis | Fourth standardized moment of pressure time series | Impulsivity measure |
| Crest factor | Peak-to-RMS ratio | Transient event prominence |

### B.2 Instrumentation Requirements

| Measurement | Instrumentation | Purpose |
|-------------|-----------------|---------|
| Acoustic pressure | Hydrophone array | Far-field signature characterization |
| Cavity boundary | High-speed imaging | Cavity dynamics and stability |
| Cavity closure | Synchronized imaging | Closure behavior and impulsive events |
| Gas flow | Mass flow measurement | Injection parameter documentation |
| Flow velocity | Reference measurement | Operating condition verification |

---

## Appendix C: Computational Requirements

### C.1 Training Data Generation

| Simulation Type | Estimated Cost (core-hours per case) | Cases Required |
|-----------------|--------------------------------------|----------------|
| URANS baseline | 1,000 to 5,000 | 500+ |
| LES unsteady | 50,000 to 200,000 | 100+ |
| Coupled acoustic | 100,000 to 500,000 | 50+ |

### C.2 Model Training

| Component | Estimated Requirement |
|-----------|----------------------|
| GPU resources | 8 to 32 A100 or equivalent |
| Training duration | 1 to 4 weeks |
| Storage | 10 to 50 TB for training data |

### C.3 Inference

Trained model inference is lightweight, enabling rapid exploration of parameter space once training is complete.

---

## References

### Self-Supervised Foundation Models

1. Meta AI. "DINOv3: Self-supervised learning for vision at unprecedented scale." August 2025. https://ai.meta.com/blog/dinov3-self-supervised-vision-model/

### Physics-Informed Machine Learning

2. Bianco, M.J., et al. "Machine learning in acoustics: a review." npj Acoustics (2025). https://www.nature.com/articles/s44384-025-00021-w

3. Karniadakis, G.E., et al. "Physics-informed machine learning." Nature Reviews Physics 3, 422–440 (2021). https://www.nature.com/articles/s42254-021-00314-5

4. Lienen, M., et al. "GenCFD: High-resolution surrogate modeling for general-purpose CFD simulations." arXiv:2409.18359 (2024). https://arxiv.org/abs/2409.18359

### Protein Structure Prediction (Conceptual Precedent)

5. Jumper, J., et al. "Highly accurate protein structure prediction with AlphaFold." Nature 596, 583–589 (2021). https://doi.org/10.1038/s41586-021-03819-2

### Cavitation and Supercavitation

6. Franc, J.P. and Michel, J.M. "Fundamentals of Cavitation." Springer (2004).

7. Savchenko, Y.N. "Supercavitation: Problems and Perspectives." CAV2001, Fourth International Symposium on Cavitation (2001).

### Cavitation Intensity Recognition (Prior Work)

8. Sha, Y., et al. "A multi-task learning for cavitation detection and cavitation intensity recognition of valve acoustic signals." arXiv:2203.01118 (2022). https://arxiv.org/abs/2203.01118

9. Zhu, Y., et al. "A Review of Pump Cavitation Fault Detection Methods Based on Different Signals." Processes 11(7), 2007 (2023). https://www.mdpi.com/2227-9717/11/7/2007

10. Chen, Q., et al. "Cavitation intensity recognition for high-speed axial piston pumps using 1-D convolutional neural networks with multi-channel inputs of vibration signals." Alexandria Engineering Journal 59(4), 4207-4218 (2020). https://www.sciencedirect.com/science/article/pii/S1110016820303768

### Propeller Cavitation Noise (Prior Work)

11. Miglianti, L., et al. "Predicting the cavitating marine propeller noise at design stage: A deep learning based approach." Ocean Engineering 209, 107481 (2020). https://doi.org/10.1016/j.oceaneng.2020.107481

12. Lee, S., et al. "Data-based modeling of propeller tip-vortex cavitation noise for realistic acoustic ship signature." Applied Acoustics (2025). https://www.sciencedirect.com/science/article/pii/S0003682X25004761

### Underwater Acoustic Target Recognition (Prior Work)

13. Santos-Domínguez, D., et al. "ShipsEar: An underwater vessel noise database." Applied Acoustics 113, 64-69 (2016).

14. Irfan, M., et al. "DeepShip: An underwater acoustic benchmark dataset and a separable convolution based autoencoder for classification." Expert Systems with Applications 183, 115270 (2021).

15. Liu, D., et al. "Underwater Acoustic Target Recognition Based on Depthwise Separable Convolution Neural Networks." Sensors 21(4), 1429 (2021). https://pmc.ncbi.nlm.nih.gov/articles/PMC7922821/

16. Domingos, L.C.F., et al. "A Survey of Underwater Acoustic Data Classification Methods Using Deep Learning for Shoreline Surveillance." Sensors 22(6), 2165 (2022). https://pmc.ncbi.nlm.nih.gov/articles/PMC8954367/

### Machine Condition Monitoring (Prior Work)

17. Nishida, T., et al. "Description and Discussion on DCASE 2024 Challenge Task 2: First-Shot Unsupervised Anomalous Sound Detection for Machine Condition Monitoring." arXiv:2406.07250 (2024). https://arxiv.org/abs/2406.07250

18. Wilkinghoff, K. "Design choices for learning embeddings from auxiliary tasks for domain generalization in anomalous sound detection." DCASE Workshop (2023).

### ONR Program Documentation

19. Office of Naval Research Code 33 (Sea Warfare and Weapons). Program descriptions and funding opportunity announcements. https://www.nre.navy.mil/

20. Navy DSRC (DoD Supercomputing Resource Center). https://www.navydsrc.hpc.mil/

---

## Contact

*Author information to be added.*

---

**Document Version:** 1.0  
**Date:** December 2024  
**Classification:** Unclassified / Public Release
