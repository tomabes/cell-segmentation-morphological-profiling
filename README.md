# Cell Segmentation and Morphological Profiling

End-to-end microscopy workflow for segmenting drug-treated MCF-7 breast cancer cells, extracting morphometric features and testing whether cell morphology can distinguish treatment conditions. The project uses the public [BBBC021 dataset](https://bbbc.broadinstitute.org/BBBC021).

[Open the notebook with outputs in Google Colab](https://colab.research.google.com/drive/1EOPU8AgNrdOFhd61035fZIoDfvqkdfvW) · [Read the full project report](REPORT.md)

![Project pipeline](assets/pipeline.png)

## What this project does

- compares classical watershed, StarDist-guided watershed and Cellpose `cyto3` for whole-cell segmentation;
- preprocessing for three-channel fluorescence microscopy;
- tunes Cellpose parameters (diameter, cell probability threshold, flow threshold, channels)
- extracts 11 interpretable size, shape and intensity features from individual cells;
- applies quality control and retains 3,179 cells for the main analysis;
- evaluates Random Forest classifiers.

## Results

The main classifier reached **0.67 accuracy** and **0.63 macro F1** on held-out fields. A drug-only experiment, excluding the DMSO control, reached **0.72 accuracy** and **0.67 macro F1** on 507 cells.

![Segmentation comparison](assets/segmentation-comparison.png)

![Morphometric PCA](assets/morphometry-pairplots.png)

![Random Forest confusion matrix](assets/random-forest-confusion-matrix.png)

The dataset does not include manual whole-cell masks. The segmentation-method comparison is therefore qualitative and based on overlays and count consistency.

## Repository structure

```text
notebooks/
  cell_segmentation_morphological_profiling.ipynb
assets/
  pipeline.png
  segmentation-comparison.png
  morphometry-pairplots.png
  random-forest-confusion-matrix.png
requirements.txt
```

The notebook is committed without cell outputs. The original executed notebook exceeded GitHub's file limit and can be seen on Google Colab.

## Run the project

Google Colab is the recommended environment because Cellpose, StarDist and their dependencies are computationally demanding.

The notebook downloads the selected BBBC021 image plates during execution.

## Tools

Python, Cellpose, StarDist, scikit-image, scikit-learn, PyTorch, TensorFlow, pandas, NumPy, Matplotlib, Seaborn and Plotly.
