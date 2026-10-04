---
icon: material/send-outline
---

# Submitting Jobs

Commands for submitting and running batch jobs on the cluster.

## `sbatch` — Submit Batch Jobs

=== "Script file"

    All options can be included inside the script as `#SBATCH` directives — see the [Job Script Templates](../job-script-templates.md).

    ```bash
    sbatch my_script.slurm
    ```

=== "Inline flags"

    You can also specify options directly on the command line. Modify them as appropriate:

    ```bash
    sbatch --partition=<partition> --gres=gpu:N --nodelist=<node> my_script.slurm
    ```

> :material-file-document-outline: **View script samples:** [Job Script Templates](../job-script-templates.md)

## Job Control Commands

```bash title="Managing a submitted job"
# Cancel a job
scancel 115

# Cancel all your jobs
scancel -u <username>

# Hold a job
scontrol hold 115

# Release a held job
scontrol release 115
```
