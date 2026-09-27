# Dataset label review

**Scope:** 10 validation images reviewed against the five rules in [`class_definitions.md`](class_definitions.md), plus the 12 images surfaced by the error mining in [`error_analysis.md`](error_analysis.md). This is a sample, not an audit of all 717 images, and the findings below should be read as indicative.

**Why we did it.** We forked this dataset rather than annotating it, so we did not know how good its labels were. Without that check there is no way to tell whether a model error is really a model error.

## What the sample contains

The most immediate finding is about composition rather than labelling. Of the 10 validation images reviewed:

| Image | Scene | Construction context? |
|---|---|---|
| val_pred_01 | Indoor webcam selfie, person in a face mask | No |
| val_pred_02 | Person in a storeroom holding a traffic cone | Marginal |
| val_pred_03 | Excavator in a field, no people present | Yes, no people |
| val_pred_04 | Aerial view of a crane on a site, workers in hi-vis | Yes |
| val_pred_05 | Empty room under construction, no people | Yes, no people |
| val_pred_06 | Worker on a utility pole | Yes |
| val_pred_07 | Group of utility workers outdoors | Yes |
| val_pred_08 | Same storeroom scene, motion-blurred | Marginal |
| val_pred_09 | — | — |
| val_pred_10 | — | — |

The error-mining images add a shopping-mall crowd, a pedestrian shopping street, a motorcycle-show promotional photograph and watermarked stock photography. Several of these contain no site, no equipment and no PPE, and carry only `NO-Hardhat` and `NO-Safety Vest` labels because the people in them are dressed ordinarily.

**This is a dataset built to detect the absence of PPE anywhere, not compliance on a construction site.** That is a reasonable dataset for someone else's problem. It is not quite ours, and it explains a good deal of the model's behaviour.

## Labelling problems found

| Image | Class | Problem |
|---|---|---|
| `thumbnail-ba5c72...` (FP1–3, FP5) | `NO-Hardhat`, `NO-Safety Vest` | **Incomplete labelling.** A dozen or more people in frame; some heads and torsos labelled, others not. The model detected four unlabelled bare heads at 0.85–0.95 and was penalised for each. |
| `YouTube_FreeStockFootage...` (FP4) | `NO-Safety Vest` | **Incomplete labelling.** Pedestrian torsos unlabelled in a street scene. |
| `youtube-126...` (FP6) | `Safety Vest` / `NO-Safety Vest` | **Wrong class.** A worker in a yellow hi-vis jacket is labelled `NO-Safety Vest`. The model's `Safety Vest` call at 0.84 is correct; the label is not. |
| `1125_jpg...` (FN4, FN6) | `Person` | Labelling is selective in a crowd — some pedestrians labelled, others not, with no apparent rule. |
| `ppe_1228...` (FN5) | `Safety Vest` | **Rule ambiguity rather than error.** A navy jacket with yellow reflective banding is labelled `Safety Vest`. Our rules permit that reading; almost all other training examples are fluorescent. The rule needs tightening either way. |
| `-211-_png...` (FN3) | `Person` | A composited video frame showing the same worker twice. Duplicated content inside one image is not something our rules anticipate. |

Roughly **half the reviewed images had at least one labelling problem**, and the single crowded mall image accounts for four of the six worst false positives on its own.

## What our own rules failed to settle

1. **Non-fluorescent reflective clothing.** Navy or dark jackets with reflective banding — vest or not? Our current wording says yes; the data mostly says no.
2. **Crowds.** How many people must be labelled in a scene containing dozens? An "all of them" rule is honest but expensive; a "foreground only" rule needs a definition of foreground.
3. **Duplicated or composited frames.** Video stills where the same person appears twice.
4. **Non-site images.** Whether an image with no construction context belongs in the dataset at all. We would now say no.

## Recommendation

Filter the dataset to construction contexts before any further training, and treat the current metrics as measuring two domains at once. This is the first of the three data improvements in the [error analysis](error_analysis.md).

---

**Method note.** This review was carried out on the ground-truth-versus-prediction images produced by `02_train_eval.ipynb` (`results/evidence/validation_predictions/` and the false-positive and false-negative folders), which render each label on its image. A larger review in the Roboflow annotation editor was planned and not completed before submission; the sample here is what supports the conclusions above, and 10 images is too few to put a percentage on dataset quality.

