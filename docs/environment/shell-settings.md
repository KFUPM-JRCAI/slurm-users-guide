---
icon: material/cog-outline
---

# Shell Settings

Every account starts with a short JRCAI block at the top of `~/.bashrc`. It loads the cluster's shared settings, one feature per line, so you can turn any of them off yourself.

## The JRCAI Block

```bash title="~/.bashrc (top)"
# >>> JRCAI cluster settings (comment a line to turn it off) >>>
source /SLURM/public/etc/cluster-env.sh
cluster_env module        # module command (Lmod)
cluster_env slurm         # SLURM defaults
cluster_env jupyter       # jupyter-start command
cluster_env conda         # Anaconda
cluster_env nexus         # JRCAI package cache
# module load ollama      # Ollama
# <<< JRCAI cluster settings <<<
```

- `source /SLURM/public/etc/cluster-env.sh` defines the `cluster_env` command used by the lines below — keep this line.
- Lines starting with `#` are off. Remove the `#` to turn one on (e.g. `# module load ollama` is off by default).

## Features

| Feature | What it does |
|---|---|
| `module` | Makes the `module` command available — see [Modules](modules.md) |
| `slurm` | Sets wider default columns for `squeue` (job, name, partition, user, account, state, time, node/reason) |
| `jupyter` | Makes `jupyter-start` available — see [Jupyter Access](../jupyter-access.md) |
| `conda` | Initializes the cluster's Anaconda (`/opt/anaconda3`) |
| `nexus` | Routes `pip`/`uv` downloads through the JRCAI package cache — see [Package Cache](package-cache.md) |

To see all features and which are on in your current shell:

```bash
cluster_env list
```

## Turning a Feature Off

Put `#` at the start of its line, save, and open a new terminal (or run `source ~/.bashrc`).

```bash
# cluster_env nexus       # JRCAI package cache
```

!!! tip "Using your own Miniconda or micromamba?"

    Comment out `cluster_env conda` and keep your own conda setup lines under **Your own settings** (see below), so only one conda is initialized.

## Your Own Settings

Add personal lines (aliases, `export PATH=...`, your own conda setup) at the end of `~/.bashrc`, below this line:

```bash
# ---- Your own settings below ----
```

Lines there apply to interactive shells. The JRCAI block sits **above** the interactive check, so batch jobs that run `source ~/.bashrc` get the same `module`, conda and cache settings as your terminal.

## In Batch Jobs

Start job scripts with:

```bash
source ~/.bashrc
```

Use the full path `~/.bashrc` — batch jobs start in the directory you submitted from, not necessarily your home.
