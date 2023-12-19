## General description

QuData solution for recommendation systems for online stores, built using the OTTO dataset.

The solution contains notebooks for various stages of data preparation, as well as a  notebook for training the LGBMRanker ranking model.

## Solution structure

- __otto-matrix-factorisation.ipynb__ - studying data using matrix factorization.
- __local-validation-dataset.ipynb__ - preparing a dataset for local validation.
- __otto-calculate-features.ipynb__ - feature preparation pipeline.
- __otto-co-matrix (first_ver).ipynb__ - pipeline for calculating the co-visit matrix for data.
- __otto-calculate-matrix.ipynb__ - pipeline for calculating the joint visit matrix for data (version 2).
- __otto-last-items-with-populars.ipynb__ - pipeline for sampling user history for recommendation.
- __otto-models (first train pipeline with features).ipynb__ - model training pipeline.
- __otto-training-model.ipynb__ - model training pipeline (version 2)
- __otto-new-pipeline-for-ml.ipynb__ - extended model training pipeline (version 3)

## Dataset

The dataset was a sequence of user events by session.

- _s_ession__ - the unique session id
- __events__ - the time ordered sequence of events in the session
     - _aid_ - the article id (product code) of the associated event
     - _ts_ - the Unix timestamp of the event
     - _type_ - the event type, i.e., whether a product was clicked, added to the user's cart, or ordered during the session

Number of records:
- 12_899_739 sessions in each of which at least 1 event.
- Each event includes one of 1_855_602 unique products.

## Approaches to learning

During the development of the ranking system, several implementation options were investigated using the following models:
1. LGBMRanker
2. Own transformer.

### LGBMRanker

An important part of training any model is the preparation of features. This applies to a greater extent to LGBMRanker, since the main part of the work on building the model was done for us by wonderful people from Microsoft and all we have to do is select the features and parameters of the model.

The first stage of developing the solution was to split the dataset into train and test parts ([local-validation-dataset.ipynb](/notebooks/local-validation-dataset.ipynb)).

In addition to the features obtained from the base dataset, features obtained from the co-visit matrix, which is an important part of modern recommendation systems, also turned out to be useful ([otto-co-matrix (first_ver).ipynb](/notebooks/otto-co-matrix%20( first_ver).ipynb) and [otto-calculate-matrix.ipynb](/notebooks/otto-calculate-matrix.ipynb))

The next, also important data for generating features, is the user history, which we get from the basic data ([otto-last-items-with-populars.ipynb](/notebooks/otto-last-items-with-populars.ipynb)) .

The final generation of features was carried out using various calculations based on all available data, namely: basic dataset data, co-visitation matrix and history ([otto-calculate-features.ipynb](/notebooks/otto-calculate-features.ipynb)).

The LGBMRanker model was trained on the received features ([otto-models (first train pipeline with features).ipynb](/notebooks/otto-models%20(first%20train%20pipeline%20with%20features).ipynb), [otto-training- model.ipynb](/notebooks/otto-training-model.ipynb), [otto-new-pipeline-for-ml.ipynb](/notebooks/otto-new-pipeline-for-ml.ipynb)).

### Own transformer

One of the alternative solutions was to build a model based on a transformer.
