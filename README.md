# Similarity Learning of Product Descriptions and Images using Multimodal Neural Networks

This is the code implementation of "Similarity Learning of Product Descriptions and Images using Multimodal Neural Networks", submitted at the [Natural Language Processing Journal by Elsevier](https://www.sciencedirect.com/journal/natural-language-processing-journal).

# Project structure

    .
    ├──assets                           # images, docs and other supplementary resources
    |
    ├──datasets                         # default location for datasets
    │  └──abo                           # Amazon Berkeley Objects (ABO) dataset
    │
    ├──data_loader                      # data loading and preprocessing
    │  ├──dataset.py                    # abstract Dataset class (handles download and overall preprocessing)
    │  └──abo.py                        # ABO class (inherits Dataset), also serves as template for custom data
    │
    ├──models                           # Model implementations in Tensorflow/Keras
    |  ├──addons                        # Tensorflow Addons fork, for MNN-BTL
    │  ├──char_cnn_zhang.py             # Character-level CNN by Zhang et al. (2015)
    │  ├──mnn_em.py                     # MNN-EM (Multimodal Neural Network with Element-wise Multiplication, base + extended model)
    │  └──mnn_btl.py                    # MNN-BTL (Multimodal Neural Network with Bidirectional Triplet Loss)
    |
    ├──notebooks                        # Jupyter notebooks
    │  ├──experiments                   # Experiments (from downloading, preprocessing up to training and evaluation)
    |  |  ├──configs                    # JSON configs for data (path, alphabet size), models (layer shapes) and training (hyperparams, callbacks, metrics)
    |  |  ├──mnn_em.ipynb               # MNN-EM experiments
    |  |  ├──extended_mnn_em.ipynb      # Extended MNN-EM experiments
    |  |  └──mnn_btl.ipynb              # MNN-BTL experiments
    |  ├──data_preparation.ipynb        # "Hands-on" ABO Data preparation, covering every processing step handled by the Data Loader
    |  └──visualize_results.ipynb       # Visualize "advanced" metrics, such as ROC and Precision-Recall curve and threshold optimization
    |
    ├──runs                             # default location for experiment outputs
    |
    ├──utils                            # Utilities and convenience functions
    │  ├──metrics.py                    # Evaluation-related functions (generate callbacks, plot and save loss and metrics,...)
    │  ├──img_processing.py             # Generate image embeddings with CNNs
    │  └──text_processing.py            # Tokenize texts and create embedding weights
    │
    ├──.gitignore
    ├──README.md                        # this very file
    └──requirements.txt                 # Pip requirements

# Installation
```
pip install -r requirements.txt
```

# Get started
In general, all the notebooks provided are standalone and may be run separately. However, we still recommend the following order:
## ABO
### Character-level models
1. [Data Preparation](./notebooks/data_preparation_amazon.ipynb), to gain understanding of the ABO data and necessary processing
2. Experiments for running our ML pipeline:
    1. [Base MNN-EM](./notebooks/experiments/mnn_em_amazon.ipynb) (Multimodal Neural Network with Element-wise Multiplication)
    2. [Extended MNN-EM](./notebooks/experiments/extended_mnn_em_amazon.ipynb) (3 inputs)
    3. [MNN-BTL](./notebooks/experiments/mnn_btl_amazon.ipynb) (Multimodal Neural Network with Bidirectional Triplet Loss)
    4. [Character-level Siamese MLSTM (Falzone et al., 2022)](./notebooks/experiments/siam_char_mlstm_falzone_amazon.ipynb)
3. [Visualizing results](./notebooks/visualize_results_amazon.ipynb), such as ROC curve, Precision Recall curve and perform threshold optimization
### Word-level models
#### Word2Vec
We compare against Word2Vec embeddings with and without pretraining.

We obtained the pretrained word embeddings from: https://huggingface.co/fse/word2vec-google-news-300
1. [Base MNN-EM (w/o pretrained embeddings)](./notebooks/experiments/mnn_em_w2v_amazon.ipynb)
2. [Base MNN-EM](./notebooks/experiments/mnn_em_w2v_pretrained_amazon.ipynb)
3. [Extended MNN-EM](./notebooks/experiments/extended_mnn_em_w2v_pretrained_amazon.ipynb)
4. [MNN-BTL](./notebooks/experiments/mnn_btl_w2v_pretrained_amazon.ipynb)
#### GloVe
We obtain the GloVe embeddings from: https://nlp.stanford.edu/projects/glove/

We only choose the embeddings trained on:
* Wikipedia 2014 + Gigaword 5 (6B tokens, 400K vocab, uncased, 50d and 100d vectors),
* Twitter (2B tweets, 27B tokens, 1.2M vocab, uncased, 50d and 100d vectors),

since the # of parameters is the closest to our models.
1. [Base MNN-EM](./notebooks/experiments/mnn_em_glove_amazon.ipynb)
2. [Extended MNN-EM](./notebooks/experiments/extended_mnn_em_glove_amazon.ipynb)
3. [MNN-BTL](./notebooks/experiments/mnn_btl_glove_amazon.ipynb)

#### BERT
We obtain the BERT model from: https://tfhub.dev/google/collections/bert/1

Due to hardware limitations, we only use `small_bert/bert_en_uncased_L-6_H-256_A-4` with:
* L=6 (Layers)
* H=256 (Hidden size)
* A=4 (Attention heads)

The # of parameters closely match our models.
1. [Base MNN-EM](./notebooks/experiments/mnn_em_bert_amazon.ipynb)
2. [Extended MNN-EM](./notebooks/experiments/extended_mnn_em_bert_amazon.ipynb)
3. [MNN-BTL](./notebooks/experiments/mnn_btl_bert_amazon.ipynb)

## MSCOCO
1. [Data Preparation](./notebooks/data_preparation_mscoco.ipynb)
2. Experiments:
    1. [Base MNN-EM](./notebooks/experiments/mnn_em_mscoco.ipynb)
    2. [Extended MNN-EM](./notebooks/experiments/extended_mnn_em_mscoco.ipynb)
    3. [MNN-BTL](./notebooks/experiments/mnn_btl_mscoco.ipynb)
3. [Visualizing results](./notebooks/visualize_results_mscoco.ipynb)
## Flickr30k Images
1. [Data Preparation](./notebooks/data_preparation_flickr30k.ipynb)
2. Experiments:
    1. [Base MNN-EM](./notebooks/experiments/mnn_em_flickr30k.ipynb)
    2. [Extended MNN-EM](./notebooks/experiments/extended_mnn_em_flickr30k.ipynb)
3. [Visualizing results](./notebooks/visualize_results_flickr30k.ipynb)