---
icon: material/console-line
---

# Interactive Jobs

## Partition Details & Limits

- **Partition name:** `interactive`
- **Available nodes:** All compute nodes
- **Time limit:** Jobs are limited to 2 hours
- **GPU limit:** Maximum of 3 GPUs across all interactive jobs simultaneously
- **Job limit:** Up to 5 concurrent jobs per group/account

!!! danger "Sessions terminate after 2 hours"

    Sessions on the `interactive` partition automatically **terminate after 2 hours**. This partition is designed for development, testing, and debugging only.

    For longer or production jobs, submit to the appropriate batch partitions instead.

## Request Specific Resources

Request resources for your interactive session:

```bash title="Request an interactive allocation"
# Basic resource request
salloc -p interactive --gres=gpu:1 --cpus-per-task=8 --mem=32G

# Run on a specific node
salloc -p interactive --nodelist=jrcai01 --gres=gpu:1 --cpus-per-task=8 --mem=32G
```

Adjust `--gres=gpu:N` for the number of GPUs, `--cpus-per-task` for CPU cores, `--mem` for memory, and `--nodelist` for a specific node as needed.

## Interactive Code Testing

```bash
# Get interactive session
salloc -p interactive --gres=gpu:1

# Test your code interactively
python test_script.py

# Debug
python -m pdb my_script.py

# Exit when done
exit
```

:zap: [How to run Jupyter Notebook](../jupyter-access.md)

!!! tip

    See [Group Policies](../group-policies.md) for best practices on using interactive sessions responsibly.
