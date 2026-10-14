# PlastiVision-Edge

Code and data pointers for *PlastiVision-Edge: A Zero-Shot Open-Vocabulary Framework for Real-Time Plastic Waste Detection and Instance Segmentation on Edge Devices* (M. Balasundaram, S. Perumal).

The whole pipeline is one notebook, `PlastiVision_Edge.ipynb`. It uses pretrained YOLOE-26 checkpoints from Ultralytics **without fine-tuning**; all the work is in the Plastic Waste Vocabulary Ontology (PWVO) prompts and the evaluation protocol.

## Repository layout

```
PlastiVision_Edge.ipynb     the notebook (outputs cleared)
requirements.txt            versions used
CLEANUP_LOG.md              what changed relative to the working notebook, and why
data/
  README.md                 expected file names for the authors' photographs
  zeroshot_images/          replicate photographs of the 13 benchmark-uncovered categories
  negative_controls/        the 11 non-target control photographs
  pilot/                    urban pilot data (daily collection log, weekly sampling, weekly totals, composition)
results/                    written by the notebook (CSV tables, figures, TACO subset manifest)
exports/                    written by the notebook (ONNX / TFLite files, label list)
```

## Running it

1. Open the notebook on a GPU runtime (developed on a Colab T4, Python 3.13, PyTorch 2.11, Ultralytics 8.4.177).
2. Run the cells top to bottom. The first cell installs the pinned Ultralytics version.
3. TACO annotations and images are downloaded automatically (Flickr links; roughly half no longer resolve). `results/taco_subset_manifest.csv` records the images that were evaluated; if it is present in the repository the notebook evaluates exactly those images.
4. TrashNet is fetched with the Kaggle API (credentials needed) or can be placed in `data/trashnet/` by hand.

Everything configurable is in the *Configuration* cell. Set `PLASTIVISION_ROOT` to change the root folder.

## Mapping from notebook parts to the paper

| Part | Output | Paper item |
|---|---|---|
| 2 | `results/pwvo.json`, `pwvo.png` | Table 1, Fig. 2 |
| 3 | `results/zeroshot_33_categories.png`, `zeroshot_per_image.csv` | Fig. 7 |
| 4 | `results/table10_replicates_negcontrols.csv` | Table 10 |
| 5 | `results/mode*.png` | Fig. 3 |
| 6 | `results/table9_taco*.csv`, `miou_*.csv`, `taco_class_distribution.csv` | Table 9 |
| 7 | `results/trashnet_eval.csv` | Table 9 |
| 8 | `results/table5_ablation.csv` | Table 5 |
| 9 | `results/table9_yolo_world.csv` | Table 9 |
| 10 | `results/coverage_taco_filtered.csv` | Fig. 4 |
| 11 | `results/export_summary.csv`, `latency_gpu.csv`, `exports/*` | Tables 6–7, Fig. 8 |

Values are produced by the notebook; none are hard-coded. Small differences from the paper are expected when the TACO subset or the GPU differs; the manifest and the per-trial latency table make such differences visible.

## Evaluation protocols in one place

* **TACO**: confidence ≥ 0.20, IoU ≥ 0.5, PASCAL-VOC 11-point AP, minimum annotation area 300 px, 18 TACO categories mapped to 13 PWVO classes (a 14th, `plastic_bag`, has no source category). The mean is taken over classes with at least one ground-truth instance.
* **Zero-shot, BU categories (Figs. 7, Table 10)**: one prompt active per image (the image's own category), confidence ≥ 0.10, YOLOE-26S. Success rates and false-positive rates carry Wilson 95 % intervals.
* **TrashNet**: whole-image detection rate (no boxes), confidence ≥ 0.10.
* **Latency**: end-to-end `predict()` on a 640×640 frame, batch 1, 8 warm-up + 40 timed runs per trial, 5 trials; every trial is listed.

## Data

Code: MIT (`LICENSE`). The authors' photographs and the urban pilot data: CC BY 4.0 (`LICENSE-DATA.md`). TACO, TrashNet and the Ultralytics weights keep their own licences (note that the Ultralytics package itself is AGPL-3.0). See the Data Availability Statement of the paper.

## Citation

Balasundaram, M.; Perumal, S. PlastiVision-Edge: A Zero-Shot Open-Vocabulary Framework for Real-Time Plastic Waste Detection and Instance Segmentation on Edge Devices. *Sustainability* (under review).
