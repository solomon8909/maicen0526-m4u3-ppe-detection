### Reproducibility proof
- **Last successful run:** 2026-09-25 20:59 UTC
- **Run mode:** `full`
- **Hardware:** Tesla T4
- **Software:** Python 3.13.15 · torch 2.11.0+cu128 · ultralytics 8.4.163
- **Dataset:** frozen release asset `ppe-construction-v1-yolo11.zip` · SHA256 `979ebedaa790feb32be28f93d96dc034ac0f71bbdfaeb589746cc61d8b83c926` · train 574 / val 143 images
- **Data path used:** keyless GitHub Release (no credentials) · annotated and versioned in Roboflow `solomon-yirga/construction-site-safety-cnfob` v1
- **Model / parameters:** yolov8n.pt · epochs 30 (released model: 30) · batch 16 · imgsz 640 · seed 42
- **Training time:** 5.4 min · **Total notebook runtime:** 5.7 min
- **Expected runtime:** full ≈ 40–75 min on T4 · verify ≈ 10–15 min · load ≈ 5 min

**Validation metrics**

| class | precision | recall | mAP50 | mAP50-95 |
|---|---|---|---|---|
| all | 0.806 | 0.584 | 0.662 | 0.411 |
| Hardhat | 0.952 | 0.633 | 0.760 | 0.486 |
| NO-Hardhat | 0.700 | 0.510 | 0.545 | 0.291 |
| NO-Safety Vest | 0.767 | 0.499 | 0.573 | 0.338 |
| Person | 0.832 | 0.626 | 0.716 | 0.485 |
| Safety Vest | 0.779 | 0.653 | 0.717 | 0.456 |
