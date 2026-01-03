# Executive Summary

## AlphaCav: Physics-Native Generative AI for Acoustic Regime Discovery

### The Problem

Gas-supported supercavitation generates intense acoustic emissions whose physical origins remain poorly understood. Current experimental and computational approaches cannot systematically explore the high-dimensional parameter space governing cavity dynamics and acoustic behavior.

Worse, 50 years of research has been subject to **survivorship bias**. We have only explored parameter spaces that seemed promising, were adjacent to known designs, or matched researcher intuition. The quiet regimes might exist in parameter space nobody ever visited.

### The Opportunity

Recent advances in physics-informed machine learning demonstrate that generative foundation models can encode complex multi-physics dynamics and generalize beyond training distributions. AlphaCav applies these architectural principles to acoustic regime discovery in cavitating flows.

### Proposed Solution

AlphaCav is a physics-native generative foundation model that:

1. Encodes the coupled physics of cavity dynamics, gas injection, and acoustic emission
2. Discovers novel acoustic regimes invisible to traditional approaches
3. Generates testable hypotheses for experimental validation
4. Corrects for selection bias by exploring systematically, not intuitively

### Key Differentiator

| Traditional Approach | AlphaCav |
|---------------------|----------|
| Optimize within known design space | Discover unknown regimes |
| Human-guided parameter selection | Physics-constrained exploration |
| Inherit 50 years of selection bias | Systematic bias correction |

### Target Stakeholders

- **ONR Code 33** (Ocean Acoustics): Primary funding alignment
- **NSWC Carderock / NUWC Newport**: Experimental validation
- **Penn State ARL**: Garfield Thomas Water Tunnel access
- **Sandia / LLNL / PNNL**: Multi-physics simulation and AI expertise

### Expected Outcomes

| Deliverable | Value |
|-------------|-------|
| Acoustic regime map | First systematic characterization of achievable behaviors |
| Mechanism identification | Physical understanding of regime transitions |
| Experimental guidance | Prioritized hypotheses for validation |
| Unexplored parameter space | Regimes missed by decades of selection bias |

### Risk Mitigation

**Strongest critique:** "Discovered regimes may only exist in the numerical model."

**Response:** The model is a discovery instrument, not a final answer. It reduces the search space from infinite to tractable. Experimental facilities validate what matters.

### ONR Alignment

This work treats acoustic regime discovery as a physics problem. The approach is mechanism-oriented, discovery-focused, and non-operational.

**Primary:** ONR Code 33, Ocean Acoustics  
**Secondary:** ONR Code 32, Hydrodynamics and Cavitation Physics

---

### Contact

**Author:** Keenan Williams  
**Email:** telesis001@icloud.com
