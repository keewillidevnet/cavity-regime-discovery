# Example: Experimental Validation Workflow

## From AI-Discovered Regime to Confirmed Physics

### Overview

This example walks through the complete validation workflow for an AlphaCav-discovered acoustic regime, from model output to experimental confirmation.

---

### Phase 1: Model Discovery

AlphaCav identifies a candidate regime with predicted low acoustic impulsivity:

```
Discovered Regime: DR-2024-0147
================================

Parameters:
  Reynolds number:     Re = 2.8 × 10^6
  Cavitation number:   σ = 0.07
  Gas injection rate:  Q_inj/Q∞ = 0.035
  Angle of attack:     α = -1.5°
  Geometry:            40° cone nose, L/D = 8

Model Predictions:
  Acoustic kurtosis:   2.2 ± 0.4 (95% CI)
  Spectral peak:       320 Hz ± 80 Hz
  Cavity coverage:     97% ± 2%
  Stability index:     0.84 ± 0.08

Confidence Level: HIGH (0.86)

Mechanism Attribution:
  Primary: Stable twin-vortex closure (42% contribution)
  Secondary: Matched injection-to-ambient pressure (31%)
  Tertiary: Favorable α shifting closure aft (27%)
```

---

### Phase 2: Pre-Test Analysis

Before committing tunnel time, conduct preliminary verification:

#### 2.1 Physics Plausibility Check

| Parameter | Value | Physical Bounds | Status |
|-----------|-------|-----------------|--------|
| Re | 2.8e6 | 10^5 - 10^8 | ✅ Valid |
| σ | 0.07 | 0.01 - 0.5 | ✅ Valid |
| Q_inj | 0.035 | 0 - 0.15 | ✅ Valid |
| α | -1.5° | -10° - +10° | ✅ Valid |

#### 2.2 Facility Compatibility

```
Facility: Penn State Garfield Thomas Water Tunnel

Required Conditions:
  Flow velocity: 15.2 m/s (for Re = 2.8e6 with L = 0.3m)
  Pressure: Adjustable for σ = 0.07
  Gas injection: Available, controllable to 0.035 Q∞

Compatibility: ✅ FEASIBLE
```

#### 2.3 Instrumentation Plan

| Measurement | Instrument | Purpose |
|-------------|------------|---------|
| Cavity boundary | High-speed camera (10 kHz) | Verify coverage and stability |
| Acoustic emission | Hydrophone array (8 elements) | Measure kurtosis and spectrum |
| Flow field | PIV (optional) | Validate closure mechanism |
| Gas flow | Mass flow meter | Confirm injection rate |

---

### Phase 3: Test Execution

#### 3.1 Test Matrix

| Run | Re | σ | Q_inj | α | Purpose |
|-----|-----|-----|-------|-----|---------|
| 1 | 2.8e6 | 0.07 | 0.035 | -1.5° | Primary test point |
| 2 | 2.8e6 | 0.07 | 0.035 | 0° | α sensitivity |
| 3 | 2.8e6 | 0.07 | 0.020 | -1.5° | Q_inj sensitivity |
| 4 | 2.8e6 | 0.10 | 0.035 | -1.5° | σ sensitivity |
| 5 | 2.0e6 | 0.07 | 0.035 | -1.5° | Re sensitivity |
| 6 | Baseline | — | — | — | Literature comparison |

#### 3.2 Data Collection Protocol

For each run:
1. Establish steady flow conditions (5 min stabilization)
2. Record 60 seconds of synchronized data
3. High-speed imaging: 10,000 fps, 6 seconds burst
4. Hydrophone: 100 kHz sample rate, continuous
5. Log all facility parameters

---

### Phase 4: Data Analysis

#### 4.1 Acoustic Metrics

```python
import numpy as np
from scipy import signal

# Load hydrophone data
pressure = load_hydrophone_data("run_001.h5")

# Compute kurtosis (impulsivity metric)
kurtosis = scipy.stats.kurtosis(pressure, fisher=True)

# Compute power spectral density
f, psd = signal.welch(pressure, fs=100000, nperseg=8192)

# Find spectral peak
peak_freq = f[np.argmax(psd)]

print(f"Measured kurtosis: {kurtosis:.2f}")
print(f"Spectral peak: {peak_freq:.0f} Hz")
```

**Results:**

| Metric | Predicted | Measured | Within CI? |
|--------|-----------|----------|------------|
| Kurtosis | 2.2 ± 0.4 | 2.4 | ✅ Yes |
| Spectral peak | 320 ± 80 Hz | 290 Hz | ✅ Yes |

#### 4.2 Cavity Characterization

```python
# Process high-speed imagery
cavity_coverage = analyze_cavity_coverage(images)
stability_index = compute_temporal_variance(cavity_coverage)

print(f"Cavity coverage: {np.mean(cavity_coverage)*100:.1f}%")
print(f"Stability index: {stability_index:.2f}")
```

**Results:**

| Metric | Predicted | Measured | Within CI? |
|--------|-----------|----------|------------|
| Coverage | 97 ± 2% | 96.2% | ✅ Yes |
| Stability | 0.84 ± 0.08 | 0.81 | ✅ Yes |

#### 4.3 Mechanism Verification

High-speed imagery analysis confirms:
- ✅ Twin-vortex closure structure observed
- ✅ No re-entrant jet penetration
- ✅ Cavity closure located aft of control surface zone

---

### Phase 5: Model Update

Feed validation results back to AlphaCav:

```python
from alphacav import ValidationResult

result = ValidationResult(
    regime_id="DR-2024-0147",
    predicted={
        "kurtosis": 2.2,
        "spectral_peak": 320,
        "cavity_coverage": 0.97,
        "stability_index": 0.84
    },
    measured={
        "kurtosis": 2.4,
        "spectral_peak": 290,
        "cavity_coverage": 0.962,
        "stability_index": 0.81
    },
    validated=True,
    notes="Twin-vortex closure confirmed via high-speed imaging"
)

model.incorporate_validation(result)
```

---

### Phase 6: Documentation

#### 6.1 Internal Report

Document findings for project records:
- Test conditions and instrumentation
- Raw data archive location
- Analysis scripts and parameters
- Comparison to model predictions
- Recommendations for follow-on testing

#### 6.2 Publication Pathway

For public release (with appropriate review):
- Frame as "regime characterization" not optimization
- Emphasize discovery methodology
- Avoid specific application context
- Focus on physics mechanisms

---

### Lessons Learned

1. **Model predictions were accurate** within stated confidence intervals
2. **Mechanism attribution was correct**: twin-vortex closure is key
3. **Sensitivity runs** confirmed regime robustness to small parameter variations
4. **Validation data** improves model for future discoveries

### Next Steps

1. Explore regime boundary to map transition conditions
2. Test additional geometries to assess transferability
3. Investigate off-design performance
4. Update training data with new measurements

---

## Appendix: DINOv3 for Automated Image Analysis

### Leveraging Vision Foundation Models

High-speed cavity imaging generates thousands of frames per test run. Manual analysis is impractical. Meta's DINOv3 provides a path to automated feature extraction without labeled training data.

### Application to Water Tunnel Footage

```python
from dinov3 import DINOv3Backbone
import torch

# Load pre-trained DINOv3 backbone (frozen)
backbone = DINOv3Backbone.load("dinov3-vit-l")

# Process high-speed cavity images
for frame in tunnel_footage:
    # Extract dense features without labels
    features = backbone.extract_features(frame)
    
    # Features capture:
    # - Cavity boundary location
    # - Re-entrant jet structure
    # - Closure mode characteristics
    # - Bubble cloud density
```

### Capabilities

| Task | Traditional Approach | DINOv3 Approach |
|------|---------------------|-----------------|
| Cavity segmentation | Manual annotation or threshold-based | Zero-shot from dense features |
| Closure mode classification | Expert labeling required | Cluster in feature space |
| Re-entrant jet tracking | Frame-by-frame manual tracking | Temporal feature consistency |
| Anomaly detection | Visual inspection | Distance in feature space |

### Integration with AlphaCav

DINOv3-extracted features from experimental imagery can be fused with AlphaCav physics predictions:

1. **Sim-to-real validation**: Compare feature distributions between CFD visualizations and tunnel footage
2. **Regime confirmation**: Cluster experimental frames in feature space to confirm predicted regime boundaries
3. **Failure mode detection**: Identify when experimental conditions diverge from predicted regimes

This creates a bridge between physics-native AI (AlphaCav) and vision foundation models (DINOv3) for comprehensive experimental validation.
