# Usage of Multi-task Model

We are working on a more detailed ReadMe for our multi-task model. For now, to use our best performing model, please use the following code block with your Roberta data: 

```python
m = torch.load("self-disclosure-model/multi-task/roberta/self-disclosure_multitask_RoBERTa_bestperforming.p")
m.get_self_dis(roberta_input_features)
```

## Interpretation of the Output

The self-disclosure score that you receive as an output will be on a scale from 0 to 1. We suggest a threshhold of 0.5 to distinguish between sentences with high (>= 0.5) and low (< 0.5) self-disclosure.

