# BTUMQA-225K

This folder contains the deterministic uncertainty-first BTUMQA-225K release.

## Release Summary

- Release name: `BTUMQA-225K`
- Generation mode: `uncertainty_first_225k`
- Target size: `225,000`
- Split sizes: train `157,500`, val `33,750`, test `33,750`
- Unique patients: `1,251`
- Unique slices: `72,025`

## Files

- `btumqa_225k_qa_pairs.csv`
- `btumqa_225k_train.csv`
- `btumqa_225k_val.csv`
- `btumqa_225k_test.csv`
- `README.md`
- `BENCHMARK_DESIGN.md`
- `FUTURE_UPDATES.md`
- `logs/dataset_generation_report.json`
- `logs/question_template_inventory.json`
- `logs/question_family_inventory.json`
- `logs/decision_rule_inventory.json`
- `logs/threshold_inventory.json`

## Scientific Note

BTUMQA-225K is a deterministic uncertainty-first brain tumor MRI VQA benchmark. It is designed so that correct answers depend more strongly on reliability-sensitive, ambiguity-sensitive, and uncertainty-aware regional reasoning.
