# Usage of Multi-task Models

We are working on a more detailed ReadMe for our multi-task model. For now, to use our best performing model, please use the following code block with your Roberta data: 

```python
import torch
m = torch.load("results/annkruel/latest2/roberta_full_model_vanilla/full_model.p")
m.get_self_dis(roberta_values_for_sentences)
```
