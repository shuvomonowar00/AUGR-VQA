# BTUMQA-225K Benchmark Design

## Design Intent

BTUMQA-225K is not just a larger VQA dataset. It is an uncertainty-sensitive benchmark designed so that:

- uncertainty is part of the answer logic
- ambiguity is explicitly represented
- reliability-aware regional reasoning matters
- trivial classification-style questions do not dominate the final release

## Benchmark Layers

### Foundational

- `highest_risk_region`
- `lowest_risk_region`
- `confidence_qualified_presence`
- `confidence_qualified_extent`

### Comparative

- `more_uncertain_region`
- `more_reliable_region`
- `uncertainty_gap_bucket`
- `reliability_gap_bucket`
- `safe_region_for_reasoning`

### Ambiguity-Sensitive

- `dominant_region_under_uncertainty`
- `ambiguous_subregion_pair`
- `uncertainty_consistency_check`

## Why This Matters

The benchmark is designed to reward models that use uncertainty meaningfully rather than treating uncertainty as a side-channel. This makes it better aligned with uncertainty-guided token modulation and future uncertainty-aware medical VQA research.
