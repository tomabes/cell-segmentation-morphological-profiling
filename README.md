# Cell Segmentation and Morphological Profiling

End-to-end microscopy workflow for segmenting drug-treated MCF-7 breast cancer cells, extracting morphometric features and testing whether cell morphology can distinguish treatment conditions. The project uses the public [BBBC021 dataset](https://bbbc.broadinstitute.org/BBBC021).

[Open the notebook in Google Colab](https://colab.research.google.com/drive/1EOPU8AgNrdOFhd61035fZIoDfvqkdfvW)

![Project pipeline](assets/pipeline.png)

## What this project does

- compares classical watershed, StarDist-guided watershed and Cellpose `cyto3` for whole-cell segmentation;
- preprocesses the DAPI, beta-tubulin and actin channels independently using 1st–99th percentile normalization, and evaluates actin-only input against merged actin/tubulin input with CLAHE (`clip_limit=0.01`, `kernel_size=512`);
- explores Cellpose `diameter={auto, 50, 70, 90}`, `cellprob_threshold={-1.0, -0.5, 0.0, 1.0}` and `flow_threshold={0.2, 0.4, 0.6, 0.8, 1.0}`; the final pipeline uses automatic diameter estimation, `cellprob_threshold=0.0` and `flow_threshold=0.8`;
- extracts 11 interpretable features: area, perimeter, equivalent diameter, major and minor axis lengths, eccentricity, aspect ratio, solidity, extent, mean actin intensity and mean tubulin intensity;
- removes border-touching objects and masks outside the 300–25,000 px area range, retaining 3,179 cells for the main analysis;
- evaluates class-balanced Random Forest classifiers with 300 trees, `max_depth=8`, `max_features="sqrt"` and `min_samples_leaf=10` on a stratified 75/25 split of entire microscopy fields.

## Results

The main classifier reached **0.67 accuracy** and **0.63 macro F1** on held-out fields. A drug-only experiment, excluding the DMSO control, reached **0.72 accuracy** and **0.67 macro F1** on 507 cells.

![Segmentation comparison](assets/segmentation-comparison.png)

[View morphometric feature pairplots (PDF)](assets/morphometry_pairplots.pdf)

![Random Forest confusion matrix](assets/random-forest-confusion-matrix.png)

The dataset does not include manual whole-cell masks. The segmentation-method comparison is therefore qualitative and based on overlays and count consistency; the reported classification metrics should not be interpreted as segmentation accuracy.

## Repository structure

```text
notebooks/
  cell_segmentation_morphological_profiling.ipynb
assets/
  pipeline.png
  segmentation-comparison.png
  morphometry_pairplots.pdf
  random-forest-confusion-matrix.png
requirements.txt
```

The notebook committed here is a source-only portfolio copy with cell outputs cleared. The original executed notebook exceeded GitHub's 100 MB file limit; the key results are preserved above.

## Run the project

Google Colab is the recommended environment because Cellpose, StarDist and their dependencies are computationally demanding. Open the notebook using the link above and run the cells in order. For a local environment:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook notebooks/cell_segmentation_morphological_profiling.ipynb
```

The notebook downloads the selected BBBC021 image plates during execution; the microscopy data are not stored in this repository.

## Tools

Python, Cellpose, StarDist, scikit-image, scikit-learn, PyTorch, TensorFlow, pandas, NumPy, Matplotlib, Seaborn and Plotly.
