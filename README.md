# A Model to Detect Self-disclosure Across Corpora

Being able to reliably estimate self-disclosure -- a key component of friendship and intimacy -- from language is important for many psychology studies. We build single-task models on five self-disclosure corpora, but find that these models generalize poorly; the within-domain accuracy of predicted message-level self-disclosure of the best-performing model (mean Pearson's r=0.69) is much higher than the respective across data set accuracy (mean Pearson's r=0.32), due to both variations in the corpora (e.g., medical vs. general topics) and labelling instructions (target variables: self-disclosure, emotional disclosure, intimacy). However, some lexical features, such as expression of negative emotions and use of first person personal pronouns such as 'I' reliably predict self-disclosure across corpora. We develop a multi-task model that improves results, with an average Pearson's r of 0.37 for out-of-corpora prediction. 

This repository contains our self-disclosure models to determine self-disclosure across corpora in text. It is part of our research efforts documented in our paper [add link once published] and contains all models we have used as pickle files. Our best performing models was a linear multi-task model based on Roberta features.


### Multi-task Models

Our *best performing model* was a *linear multi-task model* based on *Roberta features*. It can be found in the subfolder /multitask/linear/multitask_linear_roberta_self-disclosure. It was trained on four different corpora [add citations]. You can also find our best-performing multi-task model based on LIWC features in /multitask/linear/multitask_linear_liwc_self-disclosure. Nonlinear multi-task models for both features can be found in /multitask/nonlinear directory respectively.

### Single-task Models

In addition to the multi-task models, we provide single-task models which performed best for the across-corpora prediction of self-disclosure based on a variety of features such as Ngrams, EmoLex, LIWC and RoBERTa features. The respective models can be found in /singletask.

## Usage

Please refer to the README's in the multi-task and single-task directories for specific usage guidelines.

## Citation

If you use one of our self-disclosure models in your work, please cite the following paper:

TBD
