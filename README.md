# MGHalluDet: A Multi-Granularity Benchmark for Hallucination Detection in Medical Large Vision-Language Models

## Overview

MGHalluDet is a benchmark for evaluating hallucination detection in Medical Large Vision-Language Models (Medical LVLMs). Unlike existing benchmarks that primarily focus on short radiology captions or isolated sentence-level modifications, MGHalluDet provides multi-granularity chest X-ray reports with fine-grained hallucination annotations across varying contextual scales.

The benchmark is specifically designed to evaluate whether LVLMs can accurately identify hallucinated content in realistic radiology reports, ranging from concise findings to comprehensive clinical narratives.

---

## Motivation

Large Vision-Language Models have achieved remarkable progress in medical image understanding and radiology report generation. However, they remain prone to hallucinations, i.e., generating clinically unsupported or factually incorrect statements that are not grounded in the input chest X-ray.

Most existing hallucination benchmarks concentrate on short reports or sentence-level perturbations, making them insufficient for evaluating hallucination behaviors in realistic long-form radiology reports, where multiple findings, anatomical structures, and imaging descriptions interact simultaneously.

To address this limitation, we introduce **MGHalluDet**, a multi-granularity benchmark that systematically evaluates hallucination detection across different report lengths while providing token-level annotations for multiple hallucination categories.

---

## Dataset Description

MGHalluDet contains **500 manually curated chest X-ray reports**, divided into three subsets according to report length and contextual complexity.

| Subset | Source Dataset | Samples | Average Report Length |
|---------|---------------|---------|----------------------|
| Short | IU-XRay | 200 | ~50 words |
| Medium | MIMIC-CXR | 200 | ~150 words |
| Long | CheXpert-Plus | 100 | ~250 words |
| **Total** | - | **500** | - |

The three subsets progressively increase the contextual complexity of radiology reports.

- **Short subset** evaluates fundamental visual grounding and verification of localized clinical findings.
- **Medium subset** introduces more complex anatomical descriptions and multiple co-existing findings.
- **Long subset** simulates realistic comprehensive radiology reports and evaluates hallucination robustness under long-context reasoning.

This hierarchical design enables systematic evaluation of LVLMs across different narrative scales.

---

## Hallucination Categories

Each hallucinated report is manually annotated using four clinically meaningful hallucination categories.

### Object Hallucination

Object hallucinations refer to fabricated or omitted pathological findings or medical devices.

Examples include:

- Endotracheal tube
- Pacemaker
- Pleural effusion
- Pulmonary nodule
- Central venous catheter

This category evaluates whether an LVLM can correctly ground explicit medical entities in the chest X-ray.

---

### Attribute Hallucination

Attribute hallucinations modify the properties of genuine findings without changing the underlying entity.

Typical examples include:

- Left ↔ Right
- Mild ↔ Severe
- Present ↔ Absent
- Clear ↔ Opacity
- Enlarged ↔ Normal

This category measures whether models correctly understand disease severity, laterality, and clinical status.

---

### Relation Hallucination

Relation hallucinations alter anatomical or spatial relationships between findings.

Typical examples include:

- superior ↔ inferior
- anterior ↔ posterior
- upper lobe ↔ lower lobe
- right upper lobe ↔ left lower lobe

Unlike object hallucinations, relation hallucinations preserve the involved entities while changing their spatial relationships.

---

### Imaging Hallucination

Imaging hallucinations concern global imaging properties rather than localized abnormalities.

Examples include:

- PA view
- AP view
- Lateral view
- Rotated projection
- Low lung volumes
- Adequate inspiration

This category evaluates whether LVLMs correctly recognize holistic imaging characteristics.

---

## Annotation Format

Each sample contains the original report, a hallucinated report, and fine-grained hallucination annotations.

Example:

```json
{
  "id": 5,
  "images": [
    "image1.png",
    "image2.png"
  ],
  "source_text": "...",
  "hallucinated_text": "...",
  "annotations": {
    "object": [
      {
        "text": "endotracheal tube",
        "span": [70,87]
      }
    ],
    "attribute": [
      {
        "text":"enlarged",
        "span":[54,62]
      }
    ],
    "relation":[
      ...
    ],
    "imaging":[
      ...
    ]
  }
}
```

Each annotation includes:

- hallucinated text
- character span
- hallucination category

allowing token-level hallucination localization.

---

## Repository Structure

```
MGHalluDet/
│
├── README.md
├── LICENSE
└── dataset/
    ├── short_text/
    │   └── short_text.json
    ├── medium_text/
    │   └── medium_text.json
    └── long_text/
        └── long_text.json
```

---

## Data Format

Each JSON file contains a list of samples.

Each sample consists of

- sample ID
- image path(s)
- original report
- hallucinated report
- hallucination annotations

The annotation spans are indexed using character offsets in the hallucinated report.

---

## Statistics

(Add dataset statistics here.)

Example figures include:

- Number of samples
- Average report length
- Number of hallucinations
- Distribution of four hallucination types

Example:

| Category | Count |
|----------|------:|
| Object | xxx |
| Attribute | xxx |
| Relation | xxx |
| Imaging | xxx |

---

## Usage

Load the dataset:

```python
import json

with open("data/short/short.json") as f:
    dataset = json.load(f)

sample = dataset[0]

print(sample["hallucinated_text"])
print(sample["annotations"])
```

---

## Citation

If you use MGHalluDet in your research, please cite our paper.

```bibtex
@inproceedings{xxx2026mghalludet,
  title={MGHalluDet: A Multi-Granularity Benchmark for Hallucination Detection in Medical Large Vision-Language Models},
  author={...},
  booktitle={...},
  year={2026}
}
```

---

## License

The MGHalluDet dataset is released under the **Creative Commons Attribution 4.0 International (CC BY 4.0)** License.

Users are free to use, modify, and redistribute the dataset provided that appropriate attribution is given.

---

## Data Source

MGHalluDet is constructed based on publicly available chest X-ray datasets, including:

- IU-XRay
- MIMIC-CXR
- CheXpert-Plus

This repository only releases the hallucination annotations and benchmark files.

Users are responsible for obtaining access to the original datasets and complying with their respective licenses and data use agreements.

---
