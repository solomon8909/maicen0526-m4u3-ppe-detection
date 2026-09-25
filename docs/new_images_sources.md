# Unseen images — sources, rights and privacy

**Compiled by:** Chloe C. (Group 7) · **Release asset:** `new-images.zip` · **SHA256:** `5d3597fc72a5e0d2b38a7eeaa969aa80ad6c13df59ad543d9bb93404b1958dde`

The five images used for the "new image" inference evidence are published as the Release
asset `new-images.zip`, not committed to this repository. They appear in neither the
training nor the validation split. All five are from Wikimedia Commons; none is a client,
employer or project photograph, and none was uploaded to Roboflow.

**Checked before release:** licence recorded per image, no GPS metadata in any file, no
identifiable faces, no name badges or vehicle plates.

**Share-alike obligation.** `new_02.jpg` (CC BY-SA 3.0) and `new_05.jpg` (CC BY-SA 2.0)
carry share-alike terms. The prediction overlays generated from them in
`results/evidence/new_image_predictions/` are adaptations, so each is shared under the same
licence as its source image, with attribution. This is noted in README §8 as well.

**Selection criteria:** real construction sites; at least one person visible; a deliberate
spread of conditions — a clear baseline case, distant and small figures, missing PPE,
a back-lit silhouette, and a close-up — so the evidence shows the model's range rather
than five easy cases.

| File | Source URL | Author | Licence | Licence link |
|---|---|---|---|---|
| new_01.jpg | https://commons.wikimedia.org/wiki/File:Construction_Photography_of_Workers_on_Site_by_Construction_Photographer_Daniel_Mekis.jpg | Boudoirphotographyguide (photo credited to Daniel Mekis on the file page) | CC BY 4.0 | https://creativecommons.org/licenses/by/4.0/ |
| new_02.jpg | https://commons.wikimedia.org/wiki/File:Workers_dismantling_a_bamboo_scaffolding.JPG | Clément Bucco-Lechat | CC BY-SA 3.0 | https://creativecommons.org/licenses/by-sa/3.0/ |
| new_03.jpg | https://commons.wikimedia.org/wiki/File:Construction_workers_in_Iran-_Social_documentary-Photo_by_Mostafa_meraji_12.jpg | Mostafa Meraji (Mostafameraji) | CC0 1.0 | https://creativecommons.org/publicdomain/zero/1.0/ |
| new_04.jpg | https://commons.wikimedia.org/wiki/File:Construction_workers_in_Iran_01.jpg | Mostafa Meraji (Mostafameraji) | CC0 1.0 | https://creativecommons.org/publicdomain/zero/1.0/ |
| new_05.jpg | https://commons.wikimedia.org/wiki/File:Construction_worker_-_geograph.org.uk_-_1882862.jpg | David Lally (via Geograph Britain and Ireland) | CC BY-SA 2.0 | https://creativecommons.org/licenses/by-sa/2.0/ |

**Note on new_01.jpg:** Licence as declared by the uploader on Wikimedia Commons; no permission ticket on file. Workers are photographed from behind, so no faces are identifiable. The image is redistributed unmodified; the prediction overlay in results/evidence/ is an adaptation and remains under CC BY 4.0.

**Note on new_02.jpg:** Credit: Clément Bucco-Lechat, CC BY-SA 3.0, via Wikimedia Commons. Workers dismantling bamboo scaffolding in Hong Kong. Workers are small and distant, so no faces are identifiable; the packaged copy contains no GPS metadata. The image is redistributed unmodified; the prediction overlay in results/evidence/ is an adaptation and is shared under CC BY-SA 3.0, as the licence requires.

**Note on new_03.jpg:** Credit: Mostafa Meraji, CC0, via Wikimedia Commons. Construction worker in Qom, Iran, working on rebar without a hardhat or vest; an unworn hardhat lies beside him. His head is bowed and his face is only partly visible in profile; included because it is the clearest missing-PPE example available, with this limitation stated. The image is redistributed unmodified.

**Note on new_04.jpg:** Credit: Mostafa Meraji, CC0, via Wikimedia Commons. Worker on scaffolding, strongly backlit against the sky (poor-light test case). The hardhat is faintly visible; whether a vest is worn cannot be determined in silhouette, so vest predictions on this image have no reliable ground truth. No face is visible. The image is redistributed unmodified.

**Note on new_05.jpg:** Credit: "Construction worker" by David Lally, CC BY-SA 2.0, via Geograph Britain and Ireland and Wikimedia Commons. Worker on a metal roof deck wearing a blue hardhat and hi-vis vest (baseline case; note the hardhat colour is less common in the training data). Face turned down and covered by safety glasses and ear defenders; not identifiable. The image is redistributed unmodified; the prediction overlay in results/evidence/ is an adaptation and is shared under CC BY-SA 2.0, as the licence requires.
