# Example: Novel Regime Discovery Workflow

## Discovering Acoustically Favorable Operating Conditions

### Scenario

A researcher wants to identify operating conditions that minimize impulsive acoustic emissions from a gas-supported supercavitating body without sacrificing cavity stability.

Traditional approach: Run parametric CFD sweeps across known promising regions, guided by intuition and prior experiments.

AlphaCav approach: Query the model for regime boundaries, then validate AI-identified candidates.

---

### Workflow

#### Step 1: Define the Query

```python
from alphacav import RegimeQuery

query = RegimeQuery(
    objective="minimize_impulsivity",
    constraints={
        "cavity_coverage": ">0.95",      # Nearly full envelopment
        "stability_index": ">0.8",        # Temporally stable
        "reynolds_number": (1e6, 5e6),    # Operating range
    },
    geometry="ogive_nose",
    return_top_k=10
)
```

#### Step 2: Generate Candidate Regimes

```python
from alphacav import AlphaCavModel

model = AlphaCavModel.load("alphacav-v1.0")
candidates = model.discover_regimes(query)

# Output: List of parameter combinations ranked by predicted acoustic benefit
```

**Example Output:**

| Rank | Re | σ | Q_inj | α | Predicted Kurtosis | Confidence |
|------|-----|-----|-------|-----|-------------------|------------|
| 1 | 2.3e6 | 0.08 | 0.04 | -2° | 2.1 | 0.87 |
| 2 | 1.8e6 | 0.12 | 0.06 | 0° | 2.4 | 0.82 |
| 3 | 3.1e6 | 0.05 | 0.03 | -1° | 2.6 | 0.79 |

#### Step 3: Interpret Mechanisms

```python
mechanisms = model.explain_regime(candidates[0])

# Output: Physical interpretation of why this regime is favorable
```

**Example Mechanism Report:**

```
Regime #1 Mechanism Analysis
============================

Dominant Factors:
  1. Gas injection rate (Q_inj = 0.04) provides sufficient ventilation
     to prevent re-entrant jet penetration without cavity over-inflation
  
  2. Negative angle of attack (α = -2°) shifts cavity closure away from
     control surface interaction zone
  
  3. Moderate cavitation number (σ = 0.08) maintains stable cavity length
     without high-frequency oscillations

Predicted Acoustic Character:
  - Broadband dominated (no strong tonal components)
  - Low impulsivity (kurtosis < 3)
  - Spectral peak below 500 Hz

Coupling Pathways:
  - Gas injection → Cavity pressure → Closure dynamics → Acoustic emission
  - Angle of attack → Asymmetric cavity → Reduced tail slap
```

#### Step 4: Prioritize Validation

```python
validation_plan = model.plan_experiments(
    candidates[:3],
    facility="penn_state_garfield_thomas",
    budget_hours=40
)
```

**Example Validation Plan:**

```
Experimental Validation Plan
============================

Facility: Garfield Thomas Water Tunnel
Total Tunnel Hours: 40

Test Matrix:
  Condition 1: Re=2.3e6, σ=0.08, Q_inj=0.04, α=-2°
    - Duration: 8 hours
    - Measurements: High-speed imaging, hydrophone array, PIV
    - Key Validation: Confirm kurtosis < 3

  Condition 2: Re=1.8e6, σ=0.12, Q_inj=0.06, α=0°
    - Duration: 6 hours
    - Measurements: High-speed imaging, hydrophone array
    - Key Validation: Verify spectral content prediction

  Condition 3: Re=3.1e6, σ=0.05, Q_inj=0.03, α=-1°
    - Duration: 6 hours
    - Measurements: High-speed imaging, hydrophone array
    - Key Validation: Check cavity stability prediction

  Baseline Conditions: (for comparison)
    - Duration: 10 hours
    - Known configurations from literature

  Contingency:
    - Duration: 10 hours
    - Explore model-identified boundary regions
```

---

### Key Insight

The first candidate (Rank 1) exists in a region of parameter space that traditional research would likely not explore:

- **Negative angle of attack** seems counterintuitive for a body meant to travel straight
- **Lower gas injection** than typical "safe" operating points
- **Moderate Reynolds number** rather than pushing to maximum speed

This combination emerged from the model's physics-informed exploration, not from incremental refinement of known designs. The model doesn't know what's "supposed" to work. It only knows what's physically consistent.

---

### Validation Outcome (Hypothetical)

After tunnel testing:

| Candidate | Predicted Kurtosis | Measured Kurtosis | Status |
|-----------|-------------------|-------------------|--------|
| 1 | 2.1 | 2.3 | ✅ Confirmed |
| 2 | 2.4 | 2.9 | ✅ Confirmed |
| 3 | 2.6 | 4.1 | ❌ Model error |

The model successfully identified two previously unknown favorable regimes. Candidate 3's failure provides training data for model refinement.

---

### Next Steps

1. **Add validation data** to training set for model improvement
2. **Explore boundary** around confirmed regimes for optimization
3. **Investigate Candidate 3 failure mode** to improve physics encoders
4. **Publish findings** (with appropriate framing for public release)
