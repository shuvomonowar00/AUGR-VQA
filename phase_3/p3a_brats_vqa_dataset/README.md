# BrainTumorVQA Phase 3A QA Dataset

This folder contains the structured Phase 3A QA dataset releases generated from
RSNA-BraTS 2021 2D metadata plus the completed Phase 2A, Phase 2B, and Phase 2C
notebook manifests.

## Structured Releases

### `brats_vqa_core_12k/`

This is `BrainTumorVQA-Core`, the compact balanced benchmark release.

- `brats_vqa_core_qa_pairs.csv`
- `brats_vqa_core_train.csv`
- `brats_vqa_core_val.csv`
- `brats_vqa_core_test.csv`
- `logs/dataset_generation_report.json`
- `logs/question_template_inventory.json`

### `brats_vqa_full_100k/`

This is `BrainTumorVQA-Full`, the large-scale release for stronger training and
broader evaluation coverage.

- `brats_vqa_full_qa_pairs.csv`
- `brats_vqa_full_train.csv`
- `brats_vqa_full_val.csv`
- `brats_vqa_full_test.csv`
- `logs/dataset_generation_report.json`
- `logs/question_template_inventory.json`

The Full release uses the same 8 question families, the same 15 answer classes,
and the same fixed 6-region ordering as the Core release.

## Generation Commands

Generate the Core release:

```powershell
python Dataset\generate_brats_vqa_qa_dataset_12k_core_data.py
```

Generate the Full release:

```powershell
python Dataset\generate_brats_vqa_qa_dataset_100k_full_data.py
```

## Scientific Guardrail

All QA labels are deterministic and provenance-based. Subregion and uncertainty
labels must come from `target.csv` together with Phase 2A, Phase 2B, and Phase
2C manifest metadata. The generator does not fabricate unsupported clinical
labels.
