# Usage of Multi-task Models

We are working on a more detailed ReadMe for our multi-task model. For now, to use our best performing model, please use the following code block with your Roberta data: 

```python
m = torch.load("self-disclosure-model/multi-task/roberta/self-disclosure_multitask_RoBERTa_bestperforming.p")
m.get_self_dis(roberta_input_features)
```

Please replace _roberta\_input\_features_ with your corresponding inputs.
