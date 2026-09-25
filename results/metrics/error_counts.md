| class | false_positives | false_negatives |
|---|---|---|
| Hardhat | 5 | 40 |
| NO-Hardhat | 21 | 40 |
| NO-Safety Vest | 40 | 72 |
| Person | 45 | 83 |
| Safety Vest | 17 | 16 |

- FP 1: `thumbnail-ba5c72edb320b49a69e86b05775c49b2-scaled-1_jpeg_jpg.rf.230025e006adb79fed2b07fab46afd5f.jpg` — predicted **NO-Hardhat** at 0.95
- FP 2: `thumbnail-ba5c72edb320b49a69e86b05775c49b2-scaled-1_jpeg_jpg.rf.230025e006adb79fed2b07fab46afd5f.jpg` — predicted **NO-Hardhat** at 0.93
- FP 3: `thumbnail-ba5c72edb320b49a69e86b05775c49b2-scaled-1_jpeg_jpg.rf.230025e006adb79fed2b07fab46afd5f.jpg` — predicted **NO-Hardhat** at 0.89
- FP 4: `YouTube_FreeStockFootage_People-wearing-face-mask_Empty-Street_Covid19-D_DbgrvhlGs-720p_mp4-66_jpg.rf.b9a329c4a883b52dc2914c153550222a.jpg` — predicted **NO-Safety Vest** at 0.89
- FP 5: `thumbnail-ba5c72edb320b49a69e86b05775c49b2-scaled-1_jpeg_jpg.rf.230025e006adb79fed2b07fab46afd5f.jpg` — predicted **NO-Hardhat** at 0.85
- FP 6: `youtube-126_jpg.rf.6e803bad239092e616215556cde7085d.jpg` — predicted **Safety Vest** at 0.84
- FN 1: `2009_004100_jpg.rf.34879e135497f4371bd1ba55d70c6539.jpg` — missed **NO-Safety Vest** (40.5% of image)
- FN 2: `youtube-470_jpg.rf.0fa80506782ed04b98a5681c1ea558b9.jpg` — missed **Person** (40.2% of image)
- FN 3: `-211-_png_jpg.rf.289c1de62b54cd4b0811142f3c908f63.jpg` — missed **Person** (36.0% of image)
- FN 4: `1125_jpg.rf.f2b8374c585569a085b69bb39984ffee.jpg` — missed **Person** (32.6% of image)
- FN 5: `ppe_1228_jpg.rf.bd596446dcd440694304dbd6f8172667.jpg` — missed **Safety Vest** (22.9% of image)
- FN 6: `1125_jpg.rf.f2b8374c585569a085b69bb39984ffee.jpg` — missed **Person** (21.6% of image)
