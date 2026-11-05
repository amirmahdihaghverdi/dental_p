# MOPG-7 Dataset

## Overview

**MOPG-7** is a publicly available multi-clinic dataset containing **2,095 fully anonymized dental panoramic radiographs (Orthopantomograms, OPGs)** with expert-annotated COCO JSON and YOLO `.txt` format bounding-box labels for seven clinically relevant dental conditions.

The database was retrospectively collected from **four independent dental centers in Bangladesh**, providing diversity in patient demographics, imaging devices, and clinical cases.

| Image Prefix | Dental Center | Location | Radiographs |
| :--- | :--- | :--- | ---: |
| `0000XXXX` | Sonia Nursing Home | Tangail | 1,433 |
| `ibnXXX` | Ibn Sina D. Lab & Consultation Center | Dayaganj, Dhaka | 533 |
| `nirXXX` | Niramoy Diagnostic Center | Tangail | 114 |
| `hXXX` | Health City Diagnostic Center | Gaibandha | 15 |
| **Total** | **4 Dental Centers** | **Bangladesh** | **2,095** |

All patient-identifying information was removed prior to annotation and release.

---

## Ethical Approval

This study was reviewed and approved by the Research Ethics Committee (Institutional Review Board), Daffodil International University (**IRB Ref. No: REC-FSIT-2026/No: 12566**, approved December 02, 2025).

---

## File Structure

```
MOPG-7/
│
├── annotations/
│   ├── instances_train.json
│   ├── instances_val.json
│   └── instances_test.json
│
├── images/
│   ├── train/        # 1,676 images (~80%)
│   ├── val/          #   209 images (~10%)
│   └── test/         #   210 images (~10%)
│
├── labels_yolo/
│   ├── train/        # 1,676 YOLO .txt files
│   ├── val/          #   209 YOLO .txt files
│   └── test/         #   210 YOLO .txt files
│
├── metadata/
│   ├── class_definitions.csv
│   ├── image_metadata.csv
│   └── split_assignments.csv
│
├── dataset_description.yaml
├── LICENSE
└── README.md
```

---

## Dataset Statistics

| Split | Images | Annotations | Percentage |
| :--- | ---: | ---: | ---: |
| Train | 1,676 | 7,823 | ~80% |
| Validation | 209 | 923 | ~10% |
| Test | 210 | 1,088 | ~10% |
| **Total** | **2,095** | **9,834** | **100%** |

* **Image Resolution Range:** Width $W \in [454, 2876]$, Height $H \in [224, 1646]$.
* **Image Modality & Format:** Orthopantomogram (OPG) in `.jpg` format.
* **Annotation Quality Control:** Produced via **LabelImg**, annotated by an experienced dentist, independently verified by a second dentist, followed by QA label refinement and invalid box removal.

---

## Class Definitions

The dataset contains seven active object classes (IDs 0–6) totaling **9,834 validated bounding boxes**.

| Class ID | Class Name | Bounding Boxes | Percentage (%) | Description |
| :---: | :--- | ---: | ---: | :--- |
| **0** | Missing Teeth | 2,609 | 26.53% | Congenitally missing or extracted permanent teeth. |
| **1** | Dental Crown | 1,984 | 20.17% | Full-coverage prosthetic dental crowns. |
| **2** | Root Canal | 1,956 | 19.89% | Endodontically treated teeth with radiopaque filling material. |
| **3** | Caries | 1,410 | 14.34% | Radiolucent carious lesions in enamel/dentin. |
| **4** | Broken Down Teeth | 795 | 8.08% | Teeth with extensive structural loss or coronal destruction. |
| **5** | Wisdom Teeth | 869 | 8.84% | Third molars regardless of eruption status. |
| **6** | Healthy OPG | 211 | 2.15% | Image-level bounding box covering the visible dentition region. |
| **Total** | — | **9,834** | **100.00%** | — |

---

## Annotation Schema

### 1. COCO JSON (`annotations/`)

Each `annotations/instances_{split}.json` file follows standard COCO format:

```json
{
  "info": { ... },
  "licenses": [ ... ],
  "images": [
    { "id": 1, "file_name": "train/00001.jpg", "width": 1920, "height": 1080 }
  ],
  "annotations": [
    {
      "id": 1,
      "image_id": 1,
      "category_id": 0,
      "bbox": [x_min, y_min, w, h],
      "area": 1234.56,
      "segmentation": [],
      "iscrowd": 0
    }
  ],
  "categories": [
    { "id": 0, "name": "Missing Teeth", "supercategory": "dental" }
  ]
}
```

* `bbox` values are in pixel coordinates clamped to image dimensions.
* `area` is calculated as `round(bbox_w * bbox_h, 2)`.

### 2. YOLO Bounding Boxes (`labels_yolo/`)

Per-image `.txt` files use normalized bounding-box coordinates:

```text
<class_id> <x_center> <y_center> <width> <height>
```

All floating point values are normalized to $[0, 1]$ relative to image width and height.

---

## Usage & Model Training

### Ultralytics YOLO Setup (`dataset.yaml`)

```yaml
path: MOPG-7
train: images/train
val: images/val
test: images/test

names:
  0: Missing Teeth
  1: Dental Crown
  2: Root Canal
  3: Caries
  4: Broken Down Teeth
  5: Wisdom Teeth
  6: Healthy OPG
```

### Training Command

```bash
yolo detect train model=yolo11m.pt data=dataset.yaml epochs=200 imgsz=640
```

---

## Authors & Contributors

* **Faiza Feroz** (Daffodil International University) — *Corresponding Author* (`faiferoz12@gmail.com`)
* **Md. Taufiq Hasan**
* **Eyevi Talukder Samapti**
* **Md. Tanvir Hasan**
* **Shuvo Singh Partho**
* **Dr. Zinia Talukder**
* **Dr. Afrina Chowdhury**
* **Md Zahid Hasan**
* **Md Hasan Imam Bijoy**
* **Md Abdus Samad**

---

## Citation & License

If you use MOPG-7 in your research, please cite:

```bibtex
@dataset{mopg7_2026,
  author       = {Feroz, Faiza and Hasan, Md. Taufiq and Samapti, Eyevi Talukder and Hasan, Md. Tanvir and Partho, Shuvo Singh and Talukder, Zinia and Chowdhury, Afrina and Hasan, Md Zahid and Bijoy, Md Hasan Imam and Samad, Md Abdus},
  title        = {{MOPG-7: A Multi-Clinic Dental Panoramic Radiograph Dataset with Expert YOLO Labels}},
  year         = {2026},
  version      = {v4.0}
}
```

Distributed under the **Creative Commons Attribution 4.0 International (CC BY 4.0)** license.
