---
icon: material/database-search-outline
---

# Model Usage Guide

All models in our model zoo are HuggingFace models. For complete documentation and model-specific details, please visit the model's page on [HuggingFace](https://huggingface.co/).

## Path

```title="Local models directory"
/SLURM/public/models/
```

## Basic Usage Example

```python title="load_model.py" hl_lines="5"
from transformers import AutoModelForCausalLM, AutoTokenizer
import torch

model_path = "/SLURM/public/models/Qwen2.5-3B-Instruct"  # swap for any model in the Model Zoo
tokenizer = AutoTokenizer.from_pretrained(model_path)
model = AutoModelForCausalLM.from_pretrained(
    model_path,
    torch_dtype=torch.bfloat16
).to("cuda:0")

prompt = "What is the color of the sky?"
inputs = tokenizer(prompt, return_tensors="pt").to("cuda:0")
outputs = model.generate(**inputs, max_new_tokens=20)
response = tokenizer.decode(outputs[0], skip_special_tokens=True)

print(response)
```

See the [Model Zoo](model-zoo.md) for the full list of available model paths.

## Large Models (Multiple GPUs)

Models larger than one GPU's memory (for example 32B or 70B) can be spread across several GPUs. Request more GPUs for the job and let transformers place the model.

```bash
salloc -p A6000 --gres=gpu:2
```

```python title="load_large_model.py" hl_lines="10"
from transformers import AutoModelForCausalLM, AutoTokenizer
import torch

model_path = "/SLURM/public/models/Qwen2.5-32B-Instruct"  # too large for one GPU
tokenizer = AutoTokenizer.from_pretrained(model_path)
model = AutoModelForCausalLM.from_pretrained(
    model_path,
    torch_dtype=torch.bfloat16,
    device_map="auto",  # spread the model across all GPUs in the job
)

prompt = "What is the color of the sky?"
inputs = tokenizer(prompt, return_tensors="pt").to(model.device)
outputs = model.generate(**inputs, max_new_tokens=20)
response = tokenizer.decode(outputs[0], skip_special_tokens=True)

print(response)
```

!!! tip "Rule of thumb"

    In bf16 a model needs about 2 GB of GPU memory per billion parameters, plus some headroom — a 32B model needs ~64 GB (2× A6000 48 GB, or 1× A100 80 GB).

## Installation

```bash
pip install transformers torch accelerate
```

`device_map="auto"` (used for multi-GPU models above) requires `accelerate`.

## Finding Model Pages

To find the HuggingFace page for any model, visit [huggingface.co](https://huggingface.co/) and search for the model name.
