---
icon: material/rocket-launch-outline
---

# Using SLURM

Commands for monitoring, submitting, and managing jobs on the cluster, organized by task.

<div class="grid cards" markdown>

-   :material-monitor-dashboard:{ .lg .middle } **[Monitoring](monitoring.md)**

    ---

    Check cluster status, job state, and queue information.

-   :material-send-outline:{ .lg .middle } **[Submitting Jobs](submitting-jobs.md)**

    ---

    Submit batch jobs with `sbatch` and manage running jobs.

-   :material-console-line:{ .lg .middle } **[Interactive Jobs](interactive-jobs.md)**

    ---

    Get an interactive shell for development, testing, and debugging.

-   :material-folder-swap-outline:{ .lg .middle } **[Transferring Data](transferring-data.md)**

    ---

    Move files to and from the cluster with `scp` or `sftp`.

-   :material-account-key-outline:{ .lg .middle } **[Account Commands](account-commands.md)**

    ---

    Change your password and check your quota and expiration date.

</div>

## Quick Reference Card

| Category | Command | Purpose |
|--------------|-------------|-------------|
| Monitoring | `sinfo` | Cluster status |
| Monitoring | `scontrol show job ID` | Job details |
| Monitoring | `squeue -u $USER` | Your jobs |
| Submitting | `sbatch script.sh` | Submit batch job |
| Submitting | `salloc --cpus-per-task=4` | Interactive allocation |
| Submitting | `srun python script.py` | Execute command |
| Account | `spasswd` | Change password |
| Account | `userinfo` | View account expiration and disk quota |
| Transferring | `scp file.txt user@host:~/` | Upload file |
| Transferring | `sftp user@host` | Interactive transfer |
