# Error analysis

<!--
HOW TO COMPLETE (delete this block before submitting)
1. Run 02_train_eval.ipynb. Section 10 saves the 6 most confident false positives to
   results/evidence/false_positives/ and the 6 largest missed objects to
   results/evidence/false_negatives/, and writes counts to results/metrics/error_counts.md.
2. Open the images. Pick the 3 FPs and 3 FNs that show DIFFERENT failure types — three copies
   of the same mistake teach less than three different ones.
3. For each, write what the image shows and your best explanation. The "likely causes" list
   below is a menu of common PPE-detection failures to check against — use one only if the
   image actually supports it, and say what in the image supports it.
4. Rewrite the three improvements so each one points at a failure you actually saw.
-->

**Prepared by:** Vijay Arun Dongre (Group 7) · **Model:** YOLOv8n, 30 epochs · **Evaluated on:** validation split (⟪FILL: n⟫ images) · **Thresholds:** confidence 0.25, IoU 0.50 · **Totals:** ⟪FILL⟫ false positives, ⟪FILL⟫ false negatives

⟪FILL: paste the per-class table from `results/metrics/error_counts.md`⟫

A prediction counts as a **false positive** when it has no same-class label overlapping it at IoU ≥ 0.50. A label counts as a **false negative** when no same-class prediction overlaps it at IoU ≥ 0.50. Some "errors" found this way turn out to be missing or wrong labels in the dataset rather than model mistakes. Where that is the case it is said so below, because it changes what the fix is.

## False positives — the model flagged something that is not there

| # | Image | What the model predicted | What is actually there | Why we think it happened |
|---|---|---|---|---|
| FP1 | <img src="../results/evidence/false_positives/fp_01.jpg" width="260"> | ⟪FILL⟫ | ⟪FILL⟫ | ⟪FILL⟫ |
| FP2 | <img src="../results/evidence/false_positives/fp_02.jpg" width="260"> | ⟪FILL⟫ | ⟪FILL⟫ | ⟪FILL⟫ |
| FP3 | <img src="../results/evidence/false_positives/fp_03.jpg" width="260"> | ⟪FILL⟫ | ⟪FILL⟫ | ⟪FILL⟫ |

*Likely causes to check against:* yellow or orange objects (cones, machinery paint, signage) read as `Safety Vest`; round shapes (buckets, lamps, a person's bald head in strong light) read as `Hardhat`; a person at the image edge where only part of the head is visible read as `NO-Hardhat`; a correct detection that simply has no label in the dataset (a labelling gap, not a model error).

## False negatives — the model missed something that is there

| # | Image | What was missed | Conditions in the image | Why we think it happened |
|---|---|---|---|---|
| FN1 | <img src="../results/evidence/false_negatives/fn_01.jpg" width="260"> | ⟪FILL⟫ | ⟪FILL⟫ | ⟪FILL⟫ |
| FN2 | <img src="../results/evidence/false_negatives/fn_02.jpg" width="260"> | ⟪FILL⟫ | ⟪FILL⟫ | ⟪FILL⟫ |
| FN3 | <img src="../results/evidence/false_negatives/fn_03.jpg" width="260"> | ⟪FILL⟫ | ⟪FILL⟫ | ⟪FILL⟫ |

*Likely causes to check against:* small, distant workers (the nano model at 640 px loses detail on people only a few dozen pixels tall); heavy occlusion by scaffolding, machinery or other workers; back-lit or low-light scenes; unusual viewpoints such as drone or crane-camera angles that are rare in the training data; `NO-` classes in general, since they depend on noticing an absence.

## Three next data improvements, in priority order

The order follows the risk note in the governance checklist: missed violations matter more than false alarms, so fixes that raise recall on `NO-Hardhat` and `NO-Safety Vest` come first.

1. **⟪FILL — e.g. "Add ~150 images of distant workers (person under 60 px tall), labelled to the rules in class_definitions.md."⟫** Addresses FN⟪#⟫. How we will know it worked: `NO-Hardhat` recall on the validation split rises above ⟪FILL⟫.
2. **⟪FILL — e.g. "Add ~100 hard negatives: site images with cones, yellow plant and signage but no people, with no labels."⟫** Addresses FP⟪#⟫. How we will know it worked: `Safety Vest` precision rises and the FP count for that class falls.
3. **⟪FILL — e.g. "Audit and correct labels in the ⟪n⟫ validation images where the FP/FN review showed missing labels."⟫** Addresses FP⟪#⟫/FN⟪#⟫. How we will know it worked: re-running evaluation on corrected labels removes those cases without retraining — which also tells us how much of our measured error was the dataset's, not the model's.

A modelling change such as moving to `yolov8s` or training at 960 px would probably help small objects too, but it is deliberately not on this list: the brief asks for data improvements, and without better data a bigger model mostly learns the existing gaps more confidently.
