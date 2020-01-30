# SUPP.AI: detecing supplement-drug interactions

This repository contains data and models used to detect dietary supplement interactions from scientific articles. The RoBERTa-DDI model is trained used drug-drug interaction data from the DDI-2013 and NLM-DailyMed datasets. We use this model to extract evidence sentences for supplement interactions from 22M articles in Semantic Scholar.

The resulting interactions are available at [SUPP.AI](https://supp.ai/).

Extracted evidence is available for bulk download [here](https://api.semanticscholar.org/supp/).

See [our arXiv preprint](https://arxiv.org/abs/1909.08135v1) for implementation and data details.

Please address feedback to lucyw [at] allenai [dot] org.

## RoBERTa-DDI Model

The model is available in `model/`. 

RoBERTa-DDI uses pre-trained representations from the [RoBERTa](https://arxiv.org/abs/1907.11692) language model and fine-tunes these representations on DDI classification data. The model is implemented using [AllenNLP](https://github.com/allenai/allennlp).

### Training data

Training data is derived from the DDI-2013 and NLM-DailyMed datasets. Train/test splits are preserved from the [Merged PDDI](https://github.com/dbmi-pitt/Merged-PDDI) data release. Development sets are split from the training set for each of the two datasets. Additional pre-processing is performed to create pairwise combinations of entities from each sentence.

Train/Dev/Test splits are available in `training_data/`.

### Supplement interaction evaluation data

A set of 500 sentences are manually labeled for the presence or absence of a supplement-related interaction. These labels are provided in `sdi_eval.tsv`.

Performance of RoBERTa-DDI on the DDI and SDI test sets is given below:

| Test set | Precision | Recall | F1-score |
|----------|-----------|--------|----------|
| Drugs (DDI-2013) | 0.90 | 0.87 | 0.88 |
| Drugs (NLM-DailyMed) | 0.83 | 0.85 | 0.84 |
| SDI-eval | 0.82 | 0.58 | 0.68 |


## UMLS CUI clusters

We leverage [UMLS Metathesaurus](https://www.nlm.nih.gov/research/umls/knowledge_sources/metathesaurus/index.html) identifiers (CUIs) to identify supplement and drug entities. We perform filtering and clustering to create a list of supplement and drug identifiers which we surface on [SUPP.AI](https://supp.ai/). These identifier clusters are available at `cui_clusters.json`.

## Citation

If using this data or model, please cite our arXiv preprint:

```
@misc{Wang2019ExtractingEO,
  title={Extracting evidence of supplement-drug interactions from literature},
  author={Lucy Lu Wang and Oyvind Tafjord and Sarthak Jain and Arman Cohan and Sam Skjonsberg and Carissa Schoenick and Nick Botner and Waleed Ammar},
  archivePrefix={ArXiv},
  primaryClass={cs.CL},
  year={2019},
  eprint={1909.08135}
}
```