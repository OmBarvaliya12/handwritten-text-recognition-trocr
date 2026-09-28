# Methodology

## Problem

Handwritten Text Recognition (HTR) maps an image containing handwriting to a text sequence.

## Model

The project starts from `microsoft/trocr-base-handwritten`, a pretrained TrOCR vision encoder-decoder model. The encoder processes the image and the decoder generates a text sequence.

## Image pipeline

The notebook validates local images with PIL, converts them to RGB, applies the TrOCR processor, and optionally applies training-time augmentation.

Training augmentation includes small rotations, Gaussian blur, brightness/contrast variation, and small translations.

## Pseudo-label generation

Because the local dataset files used in the experiment did not include transcription labels, the pretrained TrOCR model produces initial text predictions. These predictions are then used as pseudo-labels during the experimental fine-tuning stage.

This is the key methodological limitation of the current version.

## Dataset construction

The experiment builds a dataframe containing image paths and generated labels, then adds label-length and word-count statistics. The data is shuffled with a fixed random seed and split 90/10 into training and validation sets.

## Training configuration

- 6 epochs
- Learning rate: 3e-5
- Cosine learning-rate schedule
- Warm-up ratio: 0.05
- Weight decay: 0.01
- Gradient accumulation: 2
- Checkpoint every 500 steps
- Best-checkpoint selection using CER
- Gradient checkpointing
- FP16 when CUDA is available

## Evaluation

The project computes:

- **CER:** character-level edit distance
- **WER:** word-level edit distance
- **Exact Match:** percentage of examples where the complete predicted string matches the target after trimming whitespace

## Inference

The inference helper accepts a single image path and generates a text sequence using beam search.

## Post-processing

An optional dictionary-based spell-correction function uses Python's `difflib.get_close_matches` to replace short words with close dictionary entries.

This post-processing step is separate from the core TrOCR model.

## Evaluation caveat

Because pseudo-labels originate from the pretrained model being fine-tuned, the current metrics should not be used as a claim of independently validated IAM benchmark performance.

The next methodological milestone is evaluation against verified IAM transcriptions on a held-out test set.
