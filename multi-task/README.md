# Usage of Multi-task Model

## Obtaining Self-disclosure Scores

We are working on a more detailed ReadMe for our multi-task model. For now, to use our best performing model to detect self-disclosure across corpora, please use the following code block with your Roberta data: 

```python
m = torch.load("self-disclosure-model/multi-task/roberta/self-disclosure_multitask_RoBERTa_bestperforming.p")
m.get_self_dis(roberta_input_features)
```
Please replace _roberta\_input\_features_ with your corresponding inputs.

## Interpretation of the Output

The self-disclosure score that you receive as an output will be on a scale from 0 to 1. We suggest a threshhold of 0.5 to distinguish between sentences with high (>= 0.5) and low (< 0.5) self-disclosure.

