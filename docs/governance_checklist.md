# Governance checklist

This covers the model as built for this academic project and what would have to be true before it went anywhere near a live AHG site. The two are kept separate on purpose. A checklist that treats a coursework prototype as deployment-ready is worse than no checklist.

## 1. Privacy and consent

| Item | Status | Notes |
|---|---|---|
| Training images contain identifiable people | ✅ Acknowledged | The public dataset shows workers' faces. We rely on the dataset's CC BY 4.0 licence for reuse, but a licence to reuse an image is not consent from the people in it. We therefore use these images for training and evaluation only and do not publish crops of individual faces. |
| AHG / client site photographs | ✅ Excluded | None are published in this repository, in the Release, or uploaded to Roboflow. Publishing a dataset is redistribution, and project photography is not ours to redistribute. The unseen images used for inference evidence are publicly licensed — see [`new_images_sources.md`](new_images_sources.md). |
| Redistribution of the dataset | ✅ Checked | The frozen zip is republished as a Release asset under the upstream CC BY 4.0 licence with attribution. Licence checked *before* publishing, not after. |
| Roboflow workspace visibility | ✅ Checked | The Roboflow free plan publishes projects to Universe by default. The project holds only the public CC BY 4.0 dataset, so this is acceptable; no internal imagery was ever uploaded to it. |
| Consent for any future live use | ❌ Not in place | Workers would need to be told in advance where cameras are, what is detected and who sees the output, in line with the data-protection law of the country of deployment. AHG legal would need to confirm the requirements for each country before any trial. |
| Purpose limitation | ✅ Defined | The only purpose is PPE compliance on the site. The output must not be used for attendance, productivity measurement or disciplinary action against named individuals. |

## 2. Data minimisation

- The model outputs **boxes and class labels only**. It does not identify people and has no face-recognition component.
- For any live trial, the recommended design keeps **counts and compliance rates per zone and time window**, not stored images. Where an image is needed to confirm a flag, it is kept for a short fixed period (e.g. 7 days) and then deleted.
- Training data: five of the ten upstream classes were removed because they were not needed for the question asked.
- Nothing personal is stored in this repository: evidence images come from the public dataset and from the publicly licensed unseen images listed in [`new_images_sources.md`](new_images_sources.md).
- **Credentials:** no API key appears in any committed cell, cell output or in the git history. The reproduction path needs no credentials at all, so the reader never supplies one either. If a key were ever exposed, it would be revoked and regenerated in Roboflow rather than deleted in a later commit.

## 3. Limitations — when not to use this model

- **Not a safety system.** It must never be the only check before someone enters a hazardous area, and must never be used to *clear* a worker as compliant. It can only raise a flag for a person to look at.
- **Not validated on AHG sites.** It was trained on a public dataset collected elsewhere, and the unseen test images are public ones too, because this repository is public and project photography cannot be. Its performance on DDIP, YBW-VIA or any other AHG site is therefore unknown. Testing it internally on AHG images, in a private workspace, is the first step of any pilot.
- **Weak on small and occluded people.** Distant workers and people behind scaffolding or plant are the most frequently missed (see [`error_analysis.md`](error_analysis.md)).
- **Two PPE items only.** It says nothing about harnesses, gloves, eye protection or boots — including fall protection, where the most serious construction injuries occur.
- **Daylight, ground-level photos.** Night work, heavy dust, rain and drone or crane-camera angles are under-represented in training and should be treated as untested.
- **Metrics are from one validation split** of about ⟪FILL: n⟫ images. They are an estimate, not a guarantee.

## 4. Risk note — false negatives vs false positives

For this use the two errors are not equal.

A **false negative** (a worker without a hardhat is missed) leaves a real hazard unflagged. Worse, if people come to trust the system, a missed violation can *lower* vigilance compared with no system at all. This is the error that can hurt someone.

A **false positive** (a compliant worker is flagged) costs a safety officer a minute to check. It becomes a problem only in volume, when alert fatigue sets in and flags stop being read.

So the model is tuned to favour recall on the `NO-` classes: a low confidence threshold (0.25) and a success criterion written on `NO-Hardhat` recall rather than on overall precision. The false-positive rate is then watched rather than ignored. If flags per shift grow beyond what one safety officer can review, the system fails in practice, even if every flag is technically correct.

## 5. Accountability

- A named safety officer owns every flag and every decision taken on it. The model never acts on its own.
- Any change of site, camera or season triggers re-evaluation on labelled local images before results are relied on.

## 6. Licensing and data rights

| Component | Rights | Licence | What it means for us |
|---|---|---|---|
| Our code, notebooks, documentation | Owned by the author | **MIT** | Free to reuse with attribution |
| Construction Site Safety dataset | Licensed (public) | **CC BY 4.0** | Reuse permitted with attribution to Roboflow Universe Projects — given in the README |
| Unseen inference images | Third party, publicly licensed | Per image — see [`new_images_sources.md`](new_images_sources.md) | Redistributable in the Release with attribution |
| AHG / client site photographs | Owned by AHG or its clients | Internal only | Never published, never uploaded to Roboflow |
| Ultralytics YOLOv8 library and pretrained weights | Third party | **AGPL-3.0** | Fine for open academic work. Commercial deployment inside AHG would need either AGPL compliance (publishing the full source of the deployed system) or an Ultralytics Enterprise licence |
| Our trained `best.pt` | Derived from the above | Academic use | Released for reproduction of this project only; inherits the AGPL-3.0 position of the training library |

The licence point in the last two rows matters most for any real trial. It is a cost and procurement decision, not a technical one, and belongs in the business case before any pilot rather than being discovered afterwards.
