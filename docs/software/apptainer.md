---
icon: material/cube-outline
---

# Apptainer (Containers)

[Apptainer](https://apptainer.org) runs containers without administrator rights. It is installed on all compute nodes and can run Docker images directly.

Run containers **inside a job**, not on the login node.

## Run a Docker Image

```bash
salloc --gres=gpu:1 -t 01:00:00
apptainer exec --nv docker://python:3.12 python --version
```

`--nv` gives the container access to the job's GPU(s). Leave it out for CPU-only work.

The first run downloads and converts the image, which can take a while. Later runs reuse the cached copy.

## Save an Image as a File

Converting once to a `.sif` file avoids repeating the download:

```bash
apptainer pull pytorch.sif docker://pytorch/pytorch:latest
apptainer exec --nv pytorch.sif python -c "import torch; print(torch.cuda.is_available())"
```

## Common Commands

| Command | What it does |
|---|---|
| `apptainer exec <image> <command>` | Run one command in the container |
| `apptainer shell <image>` | Open a shell inside the container |
| `apptainer run <image>` | Run the image's default command |
| `apptainer pull <file>.sif docker://<image>` | Save an image as a file |
| `apptainer cache clean` | Free space used by downloaded images |

## In a Batch Job

```bash title="container-job.sbatch"
#!/bin/bash
#SBATCH --gres=gpu:1
#SBATCH --time=02:00:00

apptainer exec --nv ~/pytorch.sif python train.py
```

## Good to Know

- Your home directory is available inside the container automatically.
- Downloaded images are cached under `~/.apptainer/cache` and count toward your home quota — see [Storage](../storage.md). Clean it with `apptainer cache clean`.
- There are currently no shared/course container images — build or pull your own as shown above.
