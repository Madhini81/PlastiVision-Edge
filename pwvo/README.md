# PWVO: Plastic Waste Vocabulary Ontology (v1)

PWVO is the set of 47 plastic-waste categories and text prompts used for zero-shot open-vocabulary detection with YOLOE-26 in the paper *A PlastiVision-Edge Enabled Plastic Waste Bank Framework for Community Income Generation and Circular Economy Transition in Rural and Urban India* (Balasundaram and Perumal, *Sustainability*, under review). Released under CC BY 4.0 (see `../LICENSE-DATA.md`).

## Files

| File | Content |
|---|---|
| `pwvo_v1.json` | The 47 categories: `prompt`, `super` (polymer super-class), `covered` |
| `taco_pwvo_mapping.json` | The 18 TACO categories mapped to PWVO keys in the TACO evaluation, with the evaluation thresholds |

`pwvo_v1.json` is identical to `results/pwvo.json`, which the notebook writes.

## Fields

| Field | Meaning |
|---|---|
| `prompt` | Text prompt given to the open-vocabulary model. Descriptive, material-form-context wording |
| `super` | One of six polymer super-classes: PET/Polyester, HDPE/LDPE film, Polypropylene, Polystyrene/EPS, Multilayer/composite, Other |
| `covered` | `true` for the 34 categories that the benchmark datasets used in the paper (TACO, TrashNet) can score. `false` for the 13 benchmark-uncovered ("BU") categories, which no benchmark in the study annotates |

"Covered" refers only to the benchmarks used in this study. It does not claim that no public dataset anywhere annotates these categories.

## Use

```python
import json
PWVO = json.load(open("pwvo/pwvo_v1.json"))
labels  = list(PWVO)
prompts = [v["prompt"] for v in PWVO.values()]

from ultralytics import YOLO
model = YOLO("yoloe-26s-seg.pt")
model.set_classes(prompts, model.get_text_pe(prompts))
results = model.predict("image.jpg", conf=0.10)
```

The paper's per-image protocol sets one prompt at a time; the full-vocabulary call above is shown only to illustrate the file format. See the notebook for the exact protocol.

## Prompts used and long prompts

Every entry has two prompt fields:

* `prompt`: the prompt used in the reported runs. 12 of the 47 are long descriptive prompts; the other 35 are short phrases.
* `prompt_long`: a longer material-form-context description for every category. For the 12 long prompts it is identical to `prompt`; for the other 35 it is an additional version released for reference. **The `prompt_long` versions of those 35 were not used in the reported runs.**

The notebook uses `prompt`.
