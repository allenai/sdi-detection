# SUPP.AI: detecting supplement-drug interactions

This repository contains data used to detect dietary supplement interactions from scientific articles on SUPP.AI. We train the RoBERTa-DDI model using drug-drug interaction data from the DDI-2013 and NLM-DailyMed datasets. We use this model to extract evidence sentences for supplement interactions from 22M articles in Semantic Scholar.

The resulting interactions are available for search at [SUPP.AI](https://supp.ai/).

Extracted evidence is available for bulk download [here](https://api.semanticscholar.org/supp/).

See [our arXiv preprint](https://arxiv.org/abs/1909.08135v1) for implementation and data details.

Please address feedback to lucyw [at] allenai [dot] org.

## RoBERTa-DDI Model

RoBERTa-DDI uses pre-trained representations from the [RoBERTa](https://arxiv.org/abs/1907.11692) language model and fine-tunes these representations on DDI classification data. The model is implemented using [AllenNLP](https://github.com/allenai/allennlp).

### Training data

Training data is derived from the DDI-2013 and NLM-DailyMed datasets. Train/test splits are preserved from the [Merged PDDI](https://github.com/dbmi-pitt/Merged-PDDI) data release. Development sets are split from the training set for each of the two datasets. Additional pre-processing is performed to create pairwise combinations of entities from each sentence.

Train/Dev/Test splits are available in `training_data/`.

| Dataset | Train | Dev | Test |
|----------|-----------|--------|----------|
| DDI 2013 (max 10) | 9442 | 1077 | 2026 |
| NLM DailyMed (max 10) | 6847 | 870 | 740 |
| DDI 2013 (max 100) | 18362* | 2069* | 4413 |
| NLM DailyMed (max 100) | 11372* | 1255* | 927 |
| DDI 2013 (all pairs) | 25594 | 2069 | 5688* |
| NLM DailyMed (all pairs) | 13090 | 1255 | 927* |

Max 10 limits source sentences to those with less than 10 unique pairs of labeled entities. Max 100 limits source sentences to those with less than 100 unique pairs of labeled entities. All pairs perserves all pairwise relationships without any filtering.

The model was trained on training data splits with max 100 pairs perserved (`training_data/NLM-DDI2013-Corpora-Pairs-V1max100/`). Test results are reported on all pairs (`training_data/NLM-DDI2013-Corpora-Pairs-All/`) of entities from the test split for comparability to prior work. These are marked with asterisks in the table for clarity.

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

If using this data, please cite our arXiv preprint:

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