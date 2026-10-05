---
icon: material/harddisk
---

# Storage

All storage below is shared across the login node and every compute node — a file you save on one is visible on all of them.

## Where Your Files Go

| Location | Path | Who can access | Use for |
|---|---|---|---|
| Home | `/SLURM/home/<username>` (`~`) | Only you | Code, scripts, environments, small datasets |
| Group storage | `/SLURM/group/<group>` | Your research group | Datasets and results shared within the group |
| Model Zoo | `/SLURM/public/models` | Everyone (read-only) | Ready-to-use HuggingFace models — see [Model Zoo](model-zoo/index.md) |
| Examples | `~/guide` | Everyone (read-only) | Example scripts — see below |

!!! note "Quota"

    Home and group storage share one pool: a group's combined usage — everyone's home directory plus the shared group directory — is hard-limited to **4 TB**, with no fixed split between them. See [Group Policies](group-policies.md).

## Check Your Quota

```bash
userinfo
```

Shows your account expiration date and disk usage. To find what takes the most space in your home:

```bash
du -sh ~/* ~/.[!.]* 2>/dev/null | sort -h | tail -15
```

Common space users: conda environments (`~/.conda`), pip cache (`~/.cache/pip`), HuggingFace cache (`~/.cache/huggingface`), Apptainer images (`~/.apptainer/cache`).

```bash
conda clean --all        # remove unused conda packages
pip cache purge          # clear the pip cache
apptainer cache clean    # clear downloaded container images
```

## The `~/guide` Folder

Every home directory has a `guide` link to a shared, read-only folder of examples:

```bash
ls ~/guide
```

- Run examples directly, e.g. `sbatch ~/guide/ollama/ollama-batch.sbatch`.
- To change one, copy it to your own folder first: `cp ~/guide/ollama/ollama-batch.sbatch ~/`.
- Examples are kept up to date for everyone; your copies are not.

## Moving Data

See [Transferring Data](using-slurm/transferring-data.md).
