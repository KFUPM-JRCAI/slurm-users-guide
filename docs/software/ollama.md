---
icon: material/chat-processing-outline
---

# Ollama

[Ollama](https://ollama.com) runs large language models on a GPU with one command. On the cluster it runs **inside your job**: each job starts its own private Ollama server on the GPU it was given, and the server stops when the job ends.

## Available Models

Models are stored in a shared, read-only location. Inside a job, list them with:

```bash
ollama list
```

!!! note "Currently available"

    Currently installed: `qwen2.5:0.5b` (small test model). More models are added over time.

The shared models are read-only, so `ollama pull` doesn't work with the default setup. To download models yourself, point your server at a folder in your home before starting it:

```bash
export OLLAMA_MODELS=~/ollama-models
```

That server then sees only your own models, and they count toward your home quota. To request a model for everyone, contact the JRCAI admin.

!!! tip "Pick a model that fits your GPU"

    Roughly, a quantized model needs a bit more GPU memory than its file size. Check `ollama list` for sizes and [Cluster Resources](../cluster-resources.md) for GPU memory per partition.

## Quick Start: Batch Job

A ready-made example is in your home directory under `~/guide/ollama/` (see [Storage](../storage.md#the-guide-folder) for how this folder works). Submit it as-is from any directory:

```bash
sbatch ~/guide/ollama/ollama-batch.sbatch
```

Output goes to `ollama-example-<jobid>.out` and the server log to `ollama-server-<jobid>.log`, in the directory you submitted from.

To change it, copy it first:

```bash
cp ~/guide/ollama/ollama-batch.sbatch ~/
```

What the script does:

```bash title="ollama-batch.sbatch"
#!/bin/bash
#SBATCH --job-name=ollama-example
#SBATCH --gres=gpu:1
#SBATCH --cpus-per-task=4
#SBATCH --mem=16G
#SBATCH --time=00:30:00
#SBATCH --output=ollama-example-%j.out

source ~/.bashrc
module load ollama

PORT=$(( 20000 + SLURM_JOB_ID % 20000 ))
while ss -ltn | awk '{print $4}' | grep -q ":$PORT\$"; do PORT=$((PORT + 1)); done
export OLLAMA_HOST=127.0.0.1:$PORT
export OLLAMA_KEEP_ALIVE=-1

ollama serve > ollama-server-$SLURM_JOB_ID.log 2>&1 &

for i in $(seq 1 60); do
    curl -s http://$OLLAMA_HOST/api/version > /dev/null && break
    sleep 1
done

ollama list
ollama run qwen2.5:0.5b "Explain what a GPU is in two sentences."
python /SLURM/public/guide/ollama/ollama-client.py
```

- `source ~/.bashrc` — loads your shell settings, including the `module` command.
- `PORT=$(( 20000 + SLURM_JOB_ID % 20000 ))` — picks a port for this job's server, so it doesn't collide with other users on the same node.
- `OLLAMA_KEEP_ALIVE=-1` — keeps the model loaded on the GPU until the job ends.
- `ollama serve ... &` — starts the server in the background. It stops automatically when the job ends.
- `for i in $(seq 1 60); do ...` — waits up to 60 seconds for the server to answer.
- `python .../ollama-client.py` — example of calling the server from Python through its HTTP API.

## Interactive Use

```bash
salloc --gres=gpu:1 -t 01:00:00
module load ollama
export OLLAMA_HOST=127.0.0.1:$(( 20000 + SLURM_JOB_ID % 20000 ))
ollama serve > ollama-server.log 2>&1 &
ollama run qwen2.5:0.5b
```

Type `/bye` to leave the chat, then `exit` to end the job.

!!! warning "Always set `OLLAMA_HOST` first"

    Without it, Ollama uses the default port `11434`, which may already be in use by another job on the same node. Set `OLLAMA_HOST` before both `ollama serve` and `ollama run`.

## From Python

Any HTTP client works. The example in `~/guide/ollama/ollama-client.py` uses only the standard library and reads the address from `OLLAMA_HOST`:

```python
import json, os, urllib.request

host = os.environ.get("OLLAMA_HOST", "127.0.0.1:11434")
payload = {"model": "qwen2.5:0.5b", "prompt": "Write a haiku about GPUs.", "stream": False}

request = urllib.request.Request(
    f"http://{host}/api/generate",
    data=json.dumps(payload).encode(),
    headers={"Content-Type": "application/json"},
)
with urllib.request.urlopen(request) as response:
    print(json.load(response)["response"])
```

## Ollama vs. the Model Zoo

| | Ollama | [Model Zoo](../model-zoo/index.md) |
|---|---|---|
| Format | Ollama models (quantized, ready to chat) | HuggingFace models |
| Use with | `ollama run`, HTTP API | `transformers`, vLLM, your own code |
| Best for | Quick chat/inference, prototyping | Fine-tuning, research code, full precision |
