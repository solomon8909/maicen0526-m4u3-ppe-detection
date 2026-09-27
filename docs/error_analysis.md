# Error analysis

**Prepared by:** Vijay Arun Dongre (Group 7), with the disputed images re-checked at full resolution and the findings consolidated by the group.

**Model:** YOLOv8n, 30 epochs · **Evaluated on:** validation split (143 images) · **Thresholds:** confidence 0.25, IoU 0.50 · **Totals:** 128 false positives, 251 false negatives

| class | false positives | false negatives | precision | recall |
|---|---|---|---|---|
| Hardhat | 5 | 40 | 0.952 | 0.633 |
| NO-Hardhat | 21 | 40 | 0.700 | 0.510 |
| NO-Safety Vest | 40 | 72 | 0.767 | 0.499 |
| Person | 45 | 83 | 0.832 | 0.626 |
| Safety Vest | 17 | 16 | 0.779 | 0.653 |

A prediction counts as a **false positive** when no same-class label overlaps it at IoU ≥ 0.50, and a label counts as a **false negative** when no same-class prediction overlaps it. The matching is mechanical, so the counts record where model and dataset disagree — not which of the two is right. Reading the images decides that, and in several cases the model is right and the label is missing.

## Part 1 · False positives

**FP1, FP2, FP3, FP5 — four confident `NO-Hardhat` calls (0.95, 0.93, 0.89, 0.85), all from one image.**
A queue of roughly seven shoppers in a retail interior. Two people carry ground-truth `NO-Hardhat` labels; the rest do not, although their heads are equally bare and equally visible. The model detected four of the unlabelled heads and was penalised once for each.

Two effects are working together. The annotation is **incomplete** — the annotator labelled the front of the queue and stopped — so these are correct detections scored as errors. And the scene is **densely packed**, with heads overlapping heads, which is where a detector is most likely to fire on partial evidence. The first effect is the dominant one here: the heads the model found are genuinely unhelmeted.

**FP4 — `NO-Safety Vest` at 0.89 on a pedestrian.**
A long-shot street scene in dull light, shoppers in coats, no worker and no PPE anywhere in the frame. At that distance the torso occupies few pixels, and a dark coat gives the same upper-body edge pattern as an unvested torso. The deeper problem is that the image is in the dataset at all.

**FP6 — `Safety Vest` at 0.84 on an excavator operator.**
At full resolution the operator is wearing a plain lime-green t-shirt: high-visibility *colour*, but no reflective banding. Our [class definitions](class_definitions.md) exclude ordinary clothing that merely happens to be yellow or green, so this is a true false positive rather than a labelling error. The model keyed on hue alone. Hold that thought for FN5.

## Part 2 · False negatives

**FN1 — missed `NO-Safety Vest`, 40.5% of the image.**
A promotional photograph of a model in a short top in front of a motorcycle poster, cropped below the waist. The model found the `Person` at 0.93 but did not read the exposed torso as an unvested one. Neither the pose, the attire nor the setting resembles anything in the site imagery.

**FN2 — missed `Person`, 40.2% of the image.**
A worker behind scaffolding, with poles crossing the body diagonally. The model **found his vest at 0.83** — it saw him — but could not assemble a whole-body box across the interruptions. This is the cleanest case for occlusion training data, precisely because the model was not blind to the person.

**FN3 — missed `Person`, 36.0% of the image.**
A frame containing the same worker more than once through reflection or compositing. Duplicated body and head features across the frame produced fragmented low-confidence boxes (0.46 and 0.35) and no confident detection of the central figure.

**FN4 and FN6 — missed `Person`, 32.6% and 21.6% of the image.**
The same street scene: pedestrians walking away from the camera, one in a business suit. The model detected a different pedestrian at 0.77 but not these two. Rear-view figures in everyday clothing are rare in the training data, which is site imagery seen from the front and side.

**FN5 — missed `Safety Vest`, 22.9% of the image.**
A navy work jacket with yellow-and-silver reflective banding, well lit, close to the camera. The model found the `Hardhat` at 0.98 and the `Person` at 0.84, so nothing was wrong with its vision. It simply did not accept a dark garment as a safety vest. Almost every `Safety Vest` instance in training is fluorescent yellow or orange.

## The finding that ties FP6 and FN5 together

Taken alone, each looks like an isolated mistake. Taken together they are two halves of one behaviour: **the model has learned fluorescent colour as the signature of a safety vest, not reflective banding.** It called a plain lime t-shirt a vest, and refused to call a banded navy jacket one. Colour is the feature it actually uses.

That is a data problem with a clean fix, and it is testable: feed it dark high-visibility workwear and bright non-workwear and the pattern should reproduce.

## The finding underneath all of it

Working through the twelve images, the recurring subject is not the model but the dataset. Among these failures are a retail queue, a pedestrian shopping street, a motorcycle-show promotional photograph, and watermarked stock photography. The [label review](label_review.md) found the same in the validation sample: indoor webcam selfies, storerooms, images with no site and no PPE, labelled `NO-Hardhat` and `NO-Safety Vest` only because the people in them wear ordinary clothes.

So the model has partly learned "bare head anywhere" rather than "bare head on a site", which is why it fires so confidently on shoppers. And our headline mAP50 of 0.662 is measuring two domains at once, which makes it a poor estimate of site performance in either direction.

## Part 3 · Class-level diagnosis

1. **Recall is the failure, and it fails where it matters most.** `NO-Safety Vest` (0.499) and `NO-Hardhat` (0.510) miss roughly half of all labelled violations. On a live site a missed violation leaves a hazard unflagged, while a false alarm costs a safety officer a minute — the two errors are not equal, and the model's bias runs the wrong way.
2. **`Person` is the largest source of absolute error** (83 misses, 45 false positives) despite having the most training instances by far (910). Occlusion, crowds, rear views and partial crops explain it; a shortage of data does not.
3. **`Safety Vest` has the best recall (0.653) on the thinnest evidence** — only 49 validation instances. Its metrics should not carry much weight until the class has more validation data.

## Part 4 · Prioritised data improvements

**Priority 1 — Break the colour bias in the vest classes.**
Add 150+ annotated site images with non-fluorescent high-visibility workwear (navy, dark green, red with banding) and with bright non-workwear (lime and orange t-shirts, jackets) under harsh shadow and low light. Settle in `class_definitions.md` whether banding or hue is the deciding feature, and relabel to match.
*Measurable check:* the FP6/FN5 pair stops reproducing — a banded dark jacket is detected as `Safety Vest`, a plain bright t-shirt is not — and `Safety Vest` precision holds above 0.78 while `NO-Safety Vest` false positives fall below 25.

**Priority 2 — Occluded, crowded and rear-view people.**
Add 200+ images of workers partially hidden by scaffolding, plant and each other, including rear and side views, and enable mosaic and random-erasing augmentation during training.
*Measurable check:* `Person` recall rises from 0.626 to ≥ 0.750 and `Person` false negatives fall below 40, down from 83.

**Priority 3 — Filter the out-of-domain images and complete the labels.**
Remove images with no construction context — retail queues, street scenes, indoor selfies, promotional and stock photography — and re-annotate what remains so that every visible head and torso carries a label rather than only the foreground ones.
*Measurable check:* `NO-Hardhat` precision rises from 0.700 to ≥ 0.820, and the confident pedestrian false positives disappear. Expect the headline mAP50 to fall when the everyday-scene images go; that is the measurement becoming honest rather than the model getting worse.

A longer schedule would also help — the [training curves](../results/curves/training_curves_summary.png) show mAP, recall and both losses still improving at epoch 30, so the run stopped while the model was still learning. But that is a separate lever. Without better data, a longer run mostly learns the existing gaps more confidently.

