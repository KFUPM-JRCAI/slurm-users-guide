---
icon: material/account-group
---

# Group Policies

How advisor groups, shared storage, and job limits work on the cluster — see [Cluster Resources](cluster-resources.md) for partition and node specs.

## Advisor Groups

Each advisor has a group with their students.

## Shared Storage

Each group has a shared directory at `/SLURM/group/<groupName>`. A group's total usage — home directories (`/SLURM/home/<username>`) plus the shared group directory combined — is hard-limited to **4 TB**, with no fixed split between them. See [Storage](storage.md) for details.

## Job Limit Rules

!!! info "Cluster-wide GPU limit"

    Up to **6 GPUs' worth of concurrent running jobs per group**, across the entire cluster. This is an updated policy — previously limits were one job per partition.

!!! info "A100 partition limit"

    Given its high demand: max **2 jobs per group** and **1 job per user** at any time.

## Best Practices

- **Plan ahead for A100 jobs** — with only 2 slots per group and 1 per user, queue early and avoid holding an A100 allocation idle.
- **Coordinate within your group** before submitting large or multi-GPU jobs, so you don't unexpectedly hit the 6-GPU cluster-wide cap and block your labmates.
- **Watch your shared storage quota** — run `userinfo` and `du -sh /SLURM/group/<groupName>` periodically and clean up old checkpoints/datasets well before hitting the 4 TB limit (shared across your group's home and group directories).
- **Release GPUs you're not using** — cancel (`scancel`) or exit idle interactive sessions promptly; they still count against your group's concurrent-job limit.
- **Use the right partition for the job** — reserve `A100` for models that genuinely need it, and prefer `RTX3090`/`A6000`/`A5000`/`A4500` for everything else to keep A100 availability high for the group.

### Interactive Jobs Specifically

- **Exit when done** — always exit your interactive session when finished to free up resources:

    ```bash
    exit
    ```

- **Don't leave sessions idle** — interactive sessions consume resources even when idle.
- **Use batch jobs for long runs** — if your job takes longer than 2 hours, submit it as a batch job to another partition instead:

    ```bash
    sbatch -p RTX3090 my_long_job.sh
    ```
