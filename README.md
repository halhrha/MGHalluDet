# MGHalluDet: Multi-Grained Benchmark for Medical LVLM Hallucination Detection Across Types and Lengths

## Overview

Medical Large Vision-Language Models (LVLMs) have demonstrated impressive capabilities in chest X-ray understanding and radiology report generation. However, they remain vulnerable to **hallucinations**, producing clinically unsupported findings, incorrect anatomical descriptions, or fabricated imaging characteristics that are inconsistent with the underlying medical image.

To address this limitation, we introduce **MGHalluDet**, a **multi-grained chest X-ray hallucination benchmark** that systematically evaluates hallucination robustness across different report lengths and contextual complexities. Instead of focusing solely on object recognition, MGHalluDet emphasizes realistic clinical reporting scenarios and provides fine-grained annotations for multiple hallucination types.

## Dataset Description

MGHalluDet contains **500 chest X-ray reports**, divided into three subsets according to report length and contextual complexity.

| Subset | Samples | Average Report Length |
|---------|---------|----------------------|
| Short | 200 | ~ 50 words |
| Medium | 200 | ~ 150 words |
| Long | 100 | ~ 250 words |
| **Total** | **500** | - |

The three subsets progressively increase the contextual complexity of radiology reports.

## Hallucination Categories

MGHalluDet categorizes hallucinations into four clinically meaningful dimensions. Compared with the conventional vision-language hallucination taxonomy (Object, Attribute, and Relation), we further introduce an **Imaging** category to capture radiology-specific hallucinations related to imaging acquisition and projection.


| Category | Description | Representative Example |
|:---------|-------------|------------------------|
| **Object** | Fabrication or omission of explicit medical devices or pathological findings. | *Pleural effusion* → *Pulmonary nodule*<br>*No pneumothorax* → *Pneumothorax present* |
| **Attribute** | Incorrect status, laterality, or severity of an existing finding. | *Mild* → *Severe*<br>*Clear* → *Opacity* |
| **Relation** | Incorrect anatomical or spatial relationships between existing findings. | *Superior to* → *Inferior to*<br>*Anterior to* → *Posterior to* |
| **Imaging** | Incorrect imaging projection, acquisition technique, or global image property. | *PA view* → *AP view*<br>*Normal view* → *Rotated view* |

## Annotation Format

Each sample consists of

- **id**: A unique identifier for the specific data sample.
- **images**: A list of file names representing the associated CXR images for the case.
- **source_text**: The original, factual medical narrative or report corresponding to the provided CXR images.
- **hallucinated_text**: The modified medical report into which targeted hallucinations have been injected.
- **annotations**: Fine-grained hallucination annotations.

Example:

```json
{
    "id": "1226",
    "images": [
        "CXR1226_IM-0150-1001.png",
        "CXR1226_IM-0150-1002.png"
    ],
    "source_text": "The heart size is within normal limits. Trachea is midline. No pleural effusions or pneumothorax. Cardiomediastinal contours are normal. There is focal consolidation in the posterior segment of the right lower lobe. No bony or soft tissue abnormalities. Right lower lobe pneumonia.",
    "hallucinated_text": "The heart size is enlarged with pacemaker leads noted. Endotracheal tube is in place. Trachea is midline. No pleural effusions or pneumothorax. Cardiomediastinal contours are normal. There is focal consolidation in anterior segment of the left upper lobe. No bony or soft tissue abnormalities. Left upper lobe pneumonia. Film is rotated.",
    "annotations": {
        "object": [
            {
                "text": "pacemaker leads",
                "span": [32, 47]
            },
            {
                "text": "Endotracheal tube",
                "span": [55, 72]
            }
        ],
        "attribute": [
            {
                "text": "enlarged",
                "span": [18, 26]
            },
            {
                "text": "Left upper lobe",
                "span": [294, 309]
            }
        ],
        "relation": [
            {
                "text": "anterior segment of the left upper lobe",
                "span": [215, 254]
            }
        ],
        "imaging": [
            {
                "text": "Film is rotated",
                "span": [321, 336]
            }
        ]
    }
}
```

## Repository Structure

```text
MGHalluDet/
│
├── README.md
├── LICENSE
│
└── dataset/
    ├── short_text/
    │   └── short_text.json
    ├── medium_text/
    │   └── medium_text.json
    └── long_text/
        └── long_text.json
```

## Dataset Preparation

This repository only provides the **hallucination annotation files** in JSON format. To construct the complete MGHalluDet benchmark, users should download the corresponding chest X-ray images from the original public datasets according to the `id` and `images` fields in each annotation file. Then, create an `images` directory and place downloaded images in this directory. Each subset corresponds to a different public chest X-ray dataset.

| MGHalluDet Subset | Source Dataset | Download Link |
|-------------------|---------------|--------------|
| **Short Text** | IU-XRay | https://www.kaggle.com/datasets/raddar/chest-xrays-indiana-university |
| **Medium Text** | MIMIC-CXR | https://physionet.org/content/mimic-cxr/2.1.0/ |
| **Long Text** | CheXpert-Plus | https://aimi.stanford.edu/datasets/chexpert-plus |

After downloading and organizing the images, the directory structure should be:

```text
MGHalluDet/
│
├── README.md
├── LICENSE
│
└── dataset/
    ├── short_text/
    │   ├── short_text.json
    │   └── images/
    │       └── ...
    ├── medium_text/
    │   ├── medium_text.json
    │   └── images/
    │       └── ...
    └── long_text/
        ├── long_text.json
        └── images/
            └── ...
```

## License

The MGHalluDet dataset is released under the **Creative Commons Attribution 4.0 International (CC BY 4.0)** License.
