# Cleanup log

How the 105-cell working notebook (`v9_github`) became `PlastiVision_Edge.ipynb` (54 cells, outputs cleared). Cell numbers refer to the working notebook, counted from 0.

## Behaviour changes (everything that can move a number)

1. **Evaluator rewritten once and used everywhere** (old 037, 045, 058, 093, 095, 099, 100, 101, 102 each carried their own copy). It now takes the model's class-to-key map explicitly instead of assuming class *i* is PWVO key *i*.
2. **Mask/box pairing.** The old loop enumerated predictions in confidence order but read `pmasks[k]` from the unsorted array. Boxes returned by Ultralytics are already confidence-sorted, so this should be a no-op in practice, but it is no longer relied on.
3. **The headline TACO mean is over classes with at least one ground-truth instance** (the old 058/061/102 logic). The old 045, which produced the 459-image output, averaged over all 14 classes including `plastic_bag` (no ground truth). Both means are returned (`box_map`, `box_map_raw`).
4. **Prompt-ablation arm fixed (old 055).** It set a 14-prompt vocabulary but mapped class ids through the 47-entry order, so predictions were credited to the wrong classes; and its "prompt-free (LRPC)" arm used the same strings as the "generic" arm. All three text arms now keep the 47-class vocabulary; the LRPC arm is removed (it needs a mapping from the `-pf` model's vocabulary to PWVO).
5. **Unreadable TACO downloads are discarded** at download time (old 045 printed `Image Read Error` for `taco_000160.jpg` and still counted its ground truth).
6. **TACO subset is frozen by a manifest** (`results/taco_subset_manifest.csv`). The old notebook's subset size varied between runs (459, 400, 366, 355, 321) because about half of the Flickr links have rotted; the attempted zip snapshots (old 096, 098) never reached Drive.
7. **PWVO prompts.** The old notebook defined PWVO twice (cell 6, 46 entries, then cell 7, 47 entries); on Run-All, cell 7 overrides cell 6, and 8 of its prompts are the short versions (`pet_tray`, `bubble_wrap`, `shrink_wrap`, `woven_pp_sack`, `retort_pouch`, `rubber_band`, `synthetic_rope`, `pvc_pipe`). The clean notebook has a single ontology, using the longer descriptive prompts.
8. **Plotting.** The old `draw_and_show` and `overlay_masks_and_boxes` drew outlines, boxes and labels on temporary `.astype(np.uint8)` copies, so they never appeared in the output. Fixed in `draw_detections`.
9. **Latency.** The per-phone table in old 066 (iPhone 14, S22, Pixel 7, OnePlus 11, with a ">18 FPS" claim) and the hard-coded GPU values in old 040 are removed. GPU latency is measured (5 trials, every trial listed). Phone latencies are read from `data/android_latency.csv`.
10. **Export.** Sizes in KiB/MiB (the old cell divided by 1e6). Hard-coded CoreML size `~12` removed. Diagnostic that printed `None` and a wrong expected-output shape removed.
11. **Parameter counts** are computed, not typed (old 074's "Table 6" had hand-entered columns and an INT8 size of 15.8 MB that contradicts the exported 10.7 MiB file).
12. **Single-image prompt check** (old 035/057, numbers typed in "from earlier tests") is now computed from a photograph (Part 8b).

## Portability changes

No `/content` paths, `google.colab`, `files.upload/download`, `input()`, widgets or shell magics other than the one pinned `%pip` line. All paths hang off `ROOT`. Seeds are not needed (inference only, no sampling). Reviewer references, "run this cell", "paste here", version labels (v4/v5/v7/v8) and decorative emoji were removed from comments and prints.

## Cell-by-cell

| Old cells | Fate | Where |
|---|---|---|
| 0 | Rewritten; stale claims (687 images, 2.6 %, 25.7 %, 38.3 %, 10.8 MB, "Bisleri") removed | title cell |
| 1, 5, 8, 11, 20, 21, 25, 27, 30, 32, 34, 36, 41, 49, 54, 56, 59, 65, 67, 70, 73 | Markdown separators and section headers; rewritten or dropped (several held stale numbers) | section headers |
| 2 | Pinned install | Part 1 |
| 3 | Imports and paths | Part 1 |
| 4 | Duplicate GPU check, merged into 3 | – |
| 6 | Earlier 46-entry ontology, overridden by cell 7 | see behaviour change 7 |
| 7 | Single ontology + Fig. 2 | Part 2 |
| 9 | Model loading; stale parameter-count comment removed | Part 1b |
| 10 | Deleted images from `/content` (destructive, Colab-only) | dropped |
| 12 | Older zero-shot test (all 47 prompts, conf 0.01, ≥ 0.50 threshold) | dropped; superseded by Parts 3–4 |
| 13 | Raw forward-pass timing of the bare network | dropped; superseded by 11.2 |
| 14, 16 | Earlier versions of the per-image zero-shot figure | dropped; superseded by 103 |
| 15, 19 | `!ls` / `import os` | dropped |
| 17, 18 | Zero-shot figure with all 47 prompts active (different protocol, different results) | dropped |
| 22 | Upload widget | replaced by `data/samples/` |
| 23, 24, 26 | Text-prompted demo, drawing, confidence sweep | Part 5 |
| 28, 29 | Interactive novel-class demo (`input()`, upload) | dropped |
| 31, 33 | Visual-prompted (SAVPE), prompt-free | Part 5 |
| 35, 57 | Hard-coded prompt-confidence numbers | replaced by Part 8b |
| 37, 45 (helpers) | Evaluation utilities | Part 6.4 |
| 38, 39, 46, 47, 53, 62, 63, 64 | Directory creation, debug counters, variable checks, Colab downloads | dropped |
| 40 | Latency figure with typed-in values | rebuilt from measurements, 11.2 |
| 42 | TACO mapping; 9 dead keys and working notes removed | Part 6.1 |
| 43 | Annotations download; mapping check from old 48 merged in | 6.2 |
| 44 (= 97) | Subset construction | 6.3, with manifest |
| 45 (loop), 58 (= 61 = 102) | Table 9 evaluation | 6.5 |
| 48 | Mapping diagnostic | folded into 6.2 |
| 50, 51, 52 | TrashNet | Part 7 |
| 55 | Prompt ablation | Part 8 (fixed) |
| 60 | Edge export | 11.1 |
| 66 | Latency profile | 11.2 (phone table removed) |
| 68, 69 | Qualitative figures that crashed (`IndexError`, missing file) | replaced by `draw_detections` |
| 71, 72 | Unfiltered coverage demos (72 crashed) | dropped; superseded by 92 |
| 74 | Hand-typed "Table 6" and next-steps list | replaced by measured scale table |
| 75 | Interactive three-model comparison; overwrote the global `hm` | dropped |
| 76, 77, 79, 80, 81 | Figures built from the authors' photographs | Appendix A (paths fixed) |
| 78, 82 | Verification / variable-check cells | dropped |
| 83, 84, 85 | ZeroWaste-f download attempts (finished with 0 test images) | dropped; ZeroWaste results are not in this notebook |
| 86 (= 101) | YOLO-World evaluation | Part 9 |
| 87, 88, 89 | YOLO-World latency, 5-trial stability | 11.2 |
| 90 | Debug trace of identical coverage values | dropped |
| 91 | Reproducibility checklist with stale numbers | replaced by README |
| 92 | Filtered coverage | Part 10 |
| 93, 95 | Resolution (1280) and conf 0.001 variants of the evaluation | dropped; `evaluate(..., imgsz=1280)` and `conf_thr=0.001` reproduce them |
| 94 | Ground-truth counts | printed by 6.5 |
| 96, 98 | Zip snapshot of the TACO subset | replaced by the manifest |
| 99, 100 | mIoU | 6.6 |
| 103 | Final per-image zero-shot figure | Part 3 |
| 104 | Replicates + negative controls | Part 4 |

## Verification status

The notebook was cleaned without access to a GPU, the model weights or the datasets, so **it has not been executed end to end**. What was checked: every code cell parses; there are no undefined names (pyflakes over the concatenated cells); the evaluator was exercised on synthetic images with a stub model (correct TP/FP assignment, AP, IoU and mask/box alignment with unsorted predictions). Run it once top to bottom on Colab and compare the printed values with the paper before publishing the repository.
