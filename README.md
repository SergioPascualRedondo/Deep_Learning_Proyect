# Deep Learning Jigsaw Reconstruction

[![Open final notebook in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/SergioPascualRedondo/DL_PROYECT_CHATY/blob/main/notebooks/DL_Sergio_Pascual_06-06-2026_CORREGIDO_GDOWN_RELOAD.ipynb)

Deep-learning project by **Sergio Pascual Redondo**. The objective is to reconstruct a `96 x 96 x 3` RGB image from nine shuffled `28 x 28 x 3` patches. The patches come from a `3 x 3` grid, but their order is hidden and their borders have been removed.

The repository contains the final corrected notebook submitted for the project. It is prepared for Google Colab and downloads the trained weights with `gdown`, so no personal Google Drive mount is required.

## Final notebook

[`notebooks/DL_Sergio_Pascual_06-06-2026_CORREGIDO_GDOWN_RELOAD.ipynb`](notebooks/DL_Sergio_Pascual_06-06-2026_CORREGIDO_GDOWN_RELOAD.ipynb)

Use the Colab badge above, or open the notebook on GitHub and select **Open in Colab**. For the submitted evaluation path, leave `TRAIN_MODEL = False`; the notebook downloads and reloads the final weights before evaluating the test split.

## Approach

The reconstruction pipeline is fully neural:

1. A shared CNN encodes each of the nine image patches.
2. Self-attention models relationships between patch tokens.
3. Learned position queries and cross-attention estimate the grid location of each patch.
4. A differentiable soft-reassembly layer builds an initial canvas.
5. A compact U-Net refiner fills missing seams and improves spatial coherence.

The model does not receive the true permutation, uses no pretrained network and stays below the six-million-parameter limit.

## Reported result

| Metric | Value |
|---|---:|
| Mean Absolute Error (MAE) | `0.039678782` |
| MAE standard deviation | `0.0006582933` |
| Total model parameters | `2,671,844` |
| Trainable parameters | `2,668,611` |

Pixels are normalized to `[0, 1]`. The official evaluation metric is MAE on the held-out test generator.

## Reproduce in Colab

1. Open the final notebook using the badge at the top of this page.
2. Select a GPU runtime in Colab: **Runtime > Change runtime type > T4 GPU**.
3. Run the notebook from top to bottom.
4. Keep `TRAIN_MODEL = False` to reproduce the submitted evaluation without retraining.

The notebook downloads STL-10, creates the shuffled-patch generators, builds the model, downloads the public weight file through `gdown`, reloads it into a fresh model and performs the evaluation.

## Repository structure

```text
DL_PROYECT_CHATY/
├── notebooks/
│   └── DL_Sergio_Pascual_06-06-2026_CORREGIDO_GDOWN_RELOAD.ipynb
├── archive/
│   ├── README.md
│   └── patches_to_image_spec_initial.ipynb
├── .gitignore
├── README.md
└── requirements.txt
```

The notebook in `archive/` is an earlier development version and is not the final submission.

## Local environment

Google Colab is the recommended environment. For a local installation:

```bash
python -m venv .venv
python -m pip install -r requirements.txt
jupyter lab
```

Large datasets, downloaded weights and generated outputs are intentionally excluded from version control.

