---
icon: material/view-module-outline
---

# Modules

Shared software on the cluster is provided as **modules**. Loading a module adds that software to your shell; unloading it removes it again. Nothing is installed in your account.

!!! note "Requirement"

    The `module` command comes from the line `cluster_env module` in your `~/.bashrc` — see [Shell Settings](shell-settings.md). If you get `module: command not found`, check that line isn't commented out.

## Commands

| Command | What it does |
|---|---|
| `module avail` | List available software |
| `module load <name>` | Load the default version (e.g. `module load ollama`) |
| `module load <name>/<version>` | Load a specific version |
| `module list` | Show what is loaded |
| `module unload <name>` | Unload one module |
| `module purge` | Unload everything |
| `module spider <name>` | Search for a module |

## Available Modules

| Module | Software |
|---|---|
| `ollama/0.34.4` | [Ollama](../software/ollama.md) — run LLMs on a GPU |

`module avail` always shows the current list.

## In Batch Jobs

```bash title="job.sbatch"
#!/bin/bash
#SBATCH --gres=gpu:1
#SBATCH --time=01:00:00

source ~/.bashrc
module load ollama

ollama --version
```

`source ~/.bashrc` loads your shell settings, including the `module` command, before using it.

## Good to Know

- Modules only affect the current shell or job. A new terminal starts clean.
- Load modules **inside** your job (after `salloc`, or in your `sbatch` script), where the software will actually run.
