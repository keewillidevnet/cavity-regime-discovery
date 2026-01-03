# The Case for Unexplored Parameter Space

## Survivorship Bias in Supercavitation Research

### The Classic Example

During World War II, the U.S. military analyzed bullet holes on aircraft returning from combat. The initial instinct was to add armor where the holes were concentrated.

Statistician Abraham Wald pointed out the flaw: **they were only seeing the planes that survived**. The missing holes represented fatal damage, the areas that needed armor most.

The military had been studying the survivors, not the full picture.

---

### The Parallel in Supercavitation

For 50 years, supercavitation research has followed a similar pattern:

```
┌─────────────────────────────────────────────────────────────────┐
│                    PARAMETER SPACE                               │
│                                                                  │
│     ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░    │
│     ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░    │
│     ░░░░░░░░░░░░░████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░    │
│     ░░░░░░░░░░░██████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░    │
│     ░░░░░░░░░░████████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░    │
│     ░░░░░░░░░░██████████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░    │
│     ░░░░░░░░░░░░████████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░    │
│     ░░░░░░░░░░░░░░████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░    │
│     ░░░░░░░░░░░░░░░░░░████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░    │
│     ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░    │
│     ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░    │
│                                                                  │
│     ░░░ = Unexplored          ████ = Explored                   │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

We've only explored a small region, specifically the configurations that:
- Seemed promising based on existing theory
- Were adjacent to known working designs
- Could be reached incrementally from previous experiments
- Matched researcher intuition about what should work

### Why This Happens

#### 1. Theory-Guided Selection

Researchers start with theoretical predictions about which regimes should be interesting. Parameters outside these predictions rarely get simulated or tested.

> "We don't run CFD for configurations we expect to fail."

#### 2. Incremental Exploration

Each new study builds on previous work. If Study A tested σ = 0.1-0.2, Study B might test σ = 0.15-0.25. The unexplored regions at σ = 0.5 or σ = 0.02 remain untouched.

#### 3. Resource Constraints

Water tunnel time costs ~$10,000/hour. Researchers optimize their test matrices for known-promising regions, not exploratory sweeps into "unlikely" territory.

#### 4. Publication Bias

Studies showing "this didn't work" rarely get published. The literature over-represents successful configurations, reinforcing the perception that explored regions are the only viable ones.

---

### What We Might Be Missing

Consider these hypothetical scenarios:

#### Scenario A: The Counterintuitive Injection Rate

```
Conventional wisdom: Higher gas injection → more stable cavity
Explored range: Q_inj = 0.05 - 0.15

What if:
  At Q_inj = 0.025, a different cavity closure mode emerges
  that happens to produce lower acoustic emission?
  
  Nobody tested it because "too little gas" seemed wrong.
```

#### Scenario B: The Unexpected Geometry

```
Conventional wisdom: Smooth, streamlined noses are best
Explored: Cones, ogives, hemispheres

What if:
  A specific non-axisymmetric perturbation disrupts
  the re-entrant jet in a way that reduces impulsive collapse?
  
  Nobody tested it because it "looked wrong."
```

#### Scenario C: The Unusual Operating Point

```
Conventional wisdom: Run as fast as possible for full supercavity
Explored: High Reynolds number, low σ

What if:
  A moderate-speed regime exists where cavity oscillation
  frequency doesn't couple with acoustic propagation?
  
  Nobody tested it because "slower = less interesting."
```

---

### How AlphaCav Addresses This

A physics-native generative model doesn't have human biases:

| Human Researcher | AlphaCav |
|------------------|----------|
| "This looks unpromising" | Evaluates physics, not intuition |
| "We already know that region" | Has no prior assumptions |
| "Let's refine what works" | Explores uniformly across constraints |
| "That can't be right" | Only rejects physics violations |

The model generates candidates across the entire physically-valid parameter space, not just the regions humans have historically favored.

---

### The Critical Question

**How do we know favorable regimes exist in unexplored regions?**

We don't know yet. That's the point.

But consider:
- The physics governing acoustic emission is nonlinear and coupled
- Nonlinear systems often have multiple stable operating modes
- Some of those modes may have been "filtered out" by our exploration strategy
- A systematic search could find them

The cost of checking is a tractable AI training problem plus targeted experiments.

The cost of not checking is leaving potential discoveries on the table forever.

---

### Analogy: Drug Discovery

Pharmaceutical companies faced a similar problem:
- Chemists had intuitions about which molecular structures would work
- They synthesized and tested compounds in "promising" regions
- Vast chemical space remained unexplored

AI-driven approaches (like AlphaFold for proteins, generative chemistry for small molecules) now explore beyond human intuition and routinely find candidates that chemists wouldn't have proposed.

AlphaCav applies the same philosophy to fluid physics.

---

### Conclusion

We've been studying the bullets holes on the planes that came back.

The quiet regimes, if they exist, might be in parameter space we never visited because the path there looked unpromising from where we started.

A physics-native AI can explore without those biases.

That's the opportunity.
