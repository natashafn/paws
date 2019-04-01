# PAWS: Paraphrase Adversaries from Word Scrambling

This dataset contains over 100,000 human-labeled pairs that feature the
importance of modeling structure, context, and word order information for the
problem of paraphrase identification. The dataset has two subsets, one based on
Wikipedia and the other one based on the
[Quora Question Pairs](https://data.quora.com/First-Quora-Dataset-Release-Question-Pairs)
(QQP) dataset.

## Motivation

Existing paraphrase identification datasets lack sentence pairs that have high
lexical overlap without being paraphrases. Models trained on such data fail to
distinguish pairs like *flights from New York to Florida* and *flights from
Florida to New York*.

Below are two examples from the dataset:

|     | Sentence 1                    | Sentence 2                    | Label |
| :-- | :---------------------------- | :---------------------------- | :---- |
| (1) | Although interchangeable, the | Although similar, the body    | 0     |
:     : body pieces on the 2 cars are : parts are not interchangeable :       :
:     : not similar.                  : on the 2 cars.                :       :
| (2) | Katz was born in Sweden in    | Katz was born in 1947 in      | 1     |
:     : 1947 and moved to New York    : Sweden and moved to New York  :       :
:     : City at the age of 1.         : at the age of one.            :       :

The first pair has different semantic meaning while the second pair is a
paraphrase. State-of-the-art models trained on existing datasets have dismal
performance on PAWS (<40% accuracy); however, including PAWS training data for
these models improves their accuracy to 85% while maintaining performance on
existing datasets such as the
[Quora Question Pairs](https://data.quora.com/First-Quora-Dataset-Release-Question-Pairs).

## PAWS-Wiki

This corpus contains pairs generated from Wikipedia pages, and can be downloaded
here:

*   [`PAWS-Wiki Labeled (Final)`](https://storage.googleapis.com/paws/english/paws_wiki_labeled_final.tar.gz):
    containing pairs that are generated from both word swapping and back
    translation methods. All pairs have human judgements on both paraphrasing
    and fluency and they are split into Train/Dev/Test sections.
*   [`PAWS-Wiki Labeled (Swap-only)`](https://storage.googleapis.com/paws/english/paws_wiki_labeled_swap.tar.gz):
    containing pairs that have no back translation counterparts and therefore
    they are not included in the first set. Nevertheless, they are high-quality
    pairs with human judgements on both paraphrasing and fluency, and they can
    be included as an auxiliary training set.
*   [`PAWS-Wiki Unlabeled (Final)`](https://storage.googleapis.com/paws/english/paws_wiki_unlabeled_final.tar.gz):
    Pairs in this set have noisy labels without human judgments and can also be
    used as an auxiliary training set. They are generated from both word
    swapping and back translation methods.

All files are in the tsv format with four columns:

Column Name   | Data
:------------ | :--------------------------
id            | A unique id for each pair
sentence1     | The first sentence
sentence2     | The second sentence
(noisy_)label | (Noisy) label for each pair

Each label has two possible values: `0` indicates the pair has different
meaning, while `1` indicates the pair is a paraphrase.

The number of examples and the proportion of paraphrase (Yes%) pairs are shown
below:

Data                | Train   | Dev    | Test  | Yes%
:------------------ | ------: | -----: | ----: | ----:
Labeled (Final)     | 49,401  | 8,000  | 8,000 | 44.2%
Labeled (Swap-only) | 30,397  | --     | --    | 9.6%
Unlabeled (Final)   | 645,652 | 10,000 | --    | 50.0%

## PAWS-QQP

This corpus contains pairs generated from the
[Quora Question Pairs](https://data.quora.com/First-Quora-Dataset-Release-Question-Pairs)
corpus. We cannot directly distribute the raw `PAWS-QQP` data due to the license
of QQP, so the examples must be reconstructed by downloading the original data
and then running our scripts to produce the data and attach the labels.

To reconstruct the `PAWS-QQP` corpus, first download the original
[Quora Question Pairs dataset](https://data.quora.com/First-Quora-Dataset-Release-Question-Pairs)
and save the tsv file to some location `/path/to/original_qqp/data.tsv`. Then
download the `PAWS-QQP` index file from the following link:

*   [`PAWS-QQP Index (Final)`](https://storage.googleapis.com/paws/english/paws_qqp.tar.gz)

Unpack it to some directory `/path/to/paws_qqp/`. Run the following commands to
generate the corpus.

```shell
export ORIGINAL_QQP_FILE="/path/to/original_qqp/data.tsv"
export PAWS_QQP_DIR="/path/to/paws_qqp/"
export PAWS_QQP_OUTPUT_DIR="/path/to/paws_qqp/output/"

python qqp_generate_data.py \
  --original_qqp_input="${ORIGINAL_QQP_FILE}" \
  --paws_input="${PAWS_QQP_DIR}/train.tsv" \
  --paws_output="${PAWS_QQP_OUTPUT_DIR}/train.tsv"

python qqp_generate_data.py \
  --original_qqp_input="${ORIGINAL_QQP_FILE}" \
  --paws_input="${PAWS_QQP_DIR}/dev_and_test.tsv" \
  --paws_output="${PAWS_QQP_OUTPUT_DIR}/dev_and_test.tsv"
```

Note: this script requires NLTK and was tested on version 3.2.5.

The generated tsv files have the same format as `PAWS-Wiki`. All pairs are
manually labeled, and the number of examples and the proportion of paraphrase
(Yes%) pairs are shown below:

Data     | Train  | Dev and Test | Yes%
:------- | -----: | -----------: | ----:
PAWS-QQP | 11,988 | 687          | 31.4%

## Reference

If you use or discuss this dataset in your work, please cite our paper:

```
@InProceedings{paws2019naacl,
  title = {{PAWS: Paraphrase Adversaries from Word Scrambling}},
  author = {Zhang, Yuan and Baldridge, Jason and He, Luheng},
  booktitle = {Proc. of NAACL},
  year = {2019}
}
```

## Contact

If you have a technical question regarding the dataset or publication, please
create an issue in this repository.
