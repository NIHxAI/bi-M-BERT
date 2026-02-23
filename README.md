# Korean-English Bilingual Medical-BERT (bi-Medical-BERT)

**bi-Medical-BERT** is a bilingual (Korean–English) medical language model built by continued pre-training of three BERT-based models on Korean and English medical corpora. This project was conducted as part of the **Korea National Institute of Health (KNIH)** Research Project (No. 2024-ER0807-00).

Three BERT-based foundation models were further pre-trained using a newly constructed bilingual medical corpus and a shared vocabulary of 45,000 tokens:

| Model | Base Model | Weights |
|-------|-----------|---------|
| **bi-M-BERT** | [M-BERT](https://huggingface.co/bert-base-multilingual-cased) (Multilingual general) | [🔗 HuggingFace](https://huggingface.co/silber2258/bi-M-BERT) |

Please refer to our paper: [Domain and Language adaptive pre-training of BERT models for Korean-English bilingual clinical text analysis](https://link.springer.com/article/10.1186/s12911-025-03262-7)

---

## Overview

Clinical notes in Korean hospitals are frequently written in a mixture of Korean and English, making it difficult for monolingual models to process them effectively. bi-Medical-BERT was developed to address this challenge by constructing a bilingual Korean–English medical corpus and performing continued pre-training on BERT-based models for bilingual clinical text analysis.

---

## Training Data

A bilingual medical corpus totaling **486,729,187 tokens** was constructed for pre-training.

| Language | Source Types |
|----------|-------------|
| Korean | Medical textbooks, health information articles |
| English | Medical textbooks, health information articles, electronic health records |
---

## Vocabulary

A shared bilingual vocabulary was generated from the training corpus.

| Item | Value |
|------|-------|
| Vocabulary size | 45,000 tokens |
| Korean subwords | 24,497 (54.44%) |
| English subwords | 15,212 (33.80%) |
| Special tokens | [PAD], [UNK], [CLS], [SEP], [MASK] |

---

## Performance Summary

The bilingual models demonstrated consistent improvements over their respective foundation models in mixed Korean–English clinical environments. Continued pre-training with a shared bilingual medical vocabulary enhanced the models' ability to:

- Process mixed-language clinical notes more coherently
- Reduce unknown token fragmentation
- Capture cross-lingual medical semantics
- Improve robustness in downstream clinical NLP applications

Across real-world hospital documentation settings, the bilingual models showed stronger generalization compared to their original base models. Among them, bi-KM-BERT exhibited the most stable and overall strongest performance profile, suggesting that Korean medical initialization combined with bilingual adaptation is particularly well aligned with Korean clinical documentation practices, where English terminology is frequently embedded within Korean narratives.

Minor decreases in baseline semantic similarity were observed in some configurations following bilingual continued pre-training. This pattern is consistent with catastrophic forgetting, where adaptation to new bilingual distributions can slightly reduce performance on previously optimized monolingual tasks. These decreases were small and did not offset the practical gains observed in mixed-language clinical applications.

In summary:

- Bilingual vocabulary alignment substantially improves real-world clinical NLP robustness.
- Korean-initialized models benefit most in Korean hospital EMR environments.
- Continued pre-training yields practical improvements even with modest computational resources.

---

## Recommended Usage

### Default Recommendation: bi-KM-BERT

bi-KM-BERT is recommended as the default model for most Korean clinical NLP applications. It is particularly suitable when:

- Clinical notes are Korean-dominant
- English medical terminology is frequently embedded within Korean text
- Real-world hospital documentation contains substantial Korean–English mixing

In empirical evaluations, bi-KM-BERT demonstrated the most stable and strongest overall performance among the three bilingual variants. Its initialization from a Korean medical foundation model, combined with bilingual adaptation, makes it especially well aligned with typical Korean clinical documentation patterns.

### bi-BioBERT

bi-BioBERT may be considered when:

- Clinical documentation is English-dominant
- Korean content is present but secondary
- Tasks are closer to English biomedical text distribution

While it benefits from bilingual continued pre-training, it remains naturally aligned with English biomedical language structure. In environments where English terminology outweighs Korean narrative content, this model may be an appropriate alternative.

### bi-M-BERT

bi-M-BERT may be appropriate for:

- Balanced multilingual environments
- Cross-lingual benchmarking scenarios
- Experimental settings without clear language dominance

However, for typical Korean hospital EMR contexts, bi-KM-BERT generally provides the most favorable balance between Korean semantic representation and embedded English terminology handling.

---

## Quick Start

### Installation

```bash
pip install transformers torch
```

### Download Model

```bash
# Install Git LFS
git lfs install

# Clone a model repository
git clone https://huggingface.co/silber2258/bi-KM-BERT
git clone https://huggingface.co/silber2258/bi-BioBERT
git clone https://huggingface.co/silber2258/bi-M-BERT
```


### Usage

```python
from transformers import AutoTokenizer, AutoModel

# Load model from local directory
model_path = "./bi-KM-BERT"

tokenizer = AutoTokenizer.from_pretrained(model_path)
model = AutoModel.from_pretrained(model_path)

text = "환자는 chest CT상 both lung에 multiple metastatic nodule 소견 보임."
inputs = tokenizer(text, return_tensors="pt", truncation=True)

# Print tokenization result
tokens = tokenizer.tokenize(text)
print(tokens)
```
---
## Citation

```bibtex
@article{jo2025bimedbert,
  title     = {Domain and Language adaptive pre-training of BERT models for Korean-English bilingual clinical text analysis},
  author    = {Jo, Eunbeen and Cho, Eunbi and Lee, Yebin and Song, Sanghoun and Joo, Hyung Joon},
  journal   = {BMC Medical Informatics and Decision Making},
  volume    = {25},
  pages     = {428},
  year      = {2025},
  doi       = {10.1186/s12911-025-03262-7},
  publisher = {BioMed Central}
}
```


