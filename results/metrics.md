# Experiment Results

## Recorded validation results

| Metric | Value |
|---|---:|
| Character Error Rate (CER) | **3.01%** |
| Word Error Rate (WER) | **7.33%** |
| Exact Match | **67.55%** |

## Dataset

- Local image subset: 4,899
- Train/validation split: 90/10
- Recorded split: approximately 4,409 training / 490 validation examples

## Interpretation

These metrics were produced by the current pseudo-labeling experiment.

They should **not** be presented as independently validated IAM benchmark results because the target transcriptions were generated using the pretrained TrOCR model.

## Training configuration

- Base model: `microsoft/trocr-base-handwritten`
- Epochs: 6
- Learning rate: 3e-5
- Scheduler: cosine
- Warm-up ratio: 0.05
- Weight decay: 0.01
- Gradient accumulation: 2
- Checkpoint interval: 500 steps
- Best-model metric: CER
- GPU mixed precision: FP16 when CUDA is available
