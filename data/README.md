# Data folders

## `zeroshot_images/` — replicate photographs (Part 3, Part 4)

Author-captured photographs of the 13 benchmark-uncovered (BU) categories. File names start with the PWVO key; anything after the key is free, e.g.

```
pet_tray_1.jpeg  cling_film_1.jpeg  bubble_wrap_1.jpeg  shrink_wrap_1.jpg  woven_pp_sack_1.jpg
eps_block_1.jpeg  retort_pouch_1.jpeg  blister_pack_1.jpeg  multilayer_film_1.jpeg
microplastic_fragments_1.jpeg  rubber_band_1.png  synthetic_rope_1.jpeg  pvc_pipe_1.jpeg
```

Matching is by prefix and is case-insensitive; `.jpg`, `.jpeg` and `.png` are accepted. Part 3 draws Fig. 7 from the first photograph of each category; Part 4 uses all of them.

## `negative_controls/` — non-target control photographs (Part 4)

The 11 control photographs: everyday objects chosen to resemble a BU category in shape, colour or texture without being an instance of any of the 13 (charging cable, rolled paper sheet, clear acrylic ruler, aluminium-foil food container, white cardboard box, cloth carry bag, mesh cloth, dark fabric pouch, printed cardboard carton, rolled grey mat, white paper in a clear polybag). Files are numbered `nc01`–`nc11` and named after the object and the BU category it resembles (e.g. `nc09_charging_cable_for_synthetic_rope.jpeg`). The ruler and the polybag contain incidental plastic, which is why these are called *non-target* controls, not *non-plastic* ones.

## `samples/` — scene photographs for the demo in Part 5 (optional)
## `figure_images/` — photographs used by Appendix A (optional)
## `pilot/` — urban pilot data
`PlastiVision_Pilot_Data_release.xlsx` (and the same sheets as CSV in `csv/`): digitised daily collection log (1 July – 31 August 2026), weekly sampling and sorting log, weekly totals by block and composition. Daily weights were weighed on site, written into the collection notebook and transcribed here (Section 2.2). No household-level or personal data are included.
