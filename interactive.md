# Interactive Jobs on SLURM Cluster

This guide explains how to run interactive jobs using the dedicated `interactive` partition.

## Overview

The `interactive` partition is designed for interactive work sessions where you need real-time access to compute resources. Unlike batch jobs, interactive sessions give you a shell on a compute node where you can run commands, test code, and debug applications in real-time.

## Partition Details & Limits

- **Partition Name:** `interactive`
- **Available Nodes:** All compute nodes 
- **Time Limit:** Jobs Are limited by 1 hour
- **GPU Limit:** Maximum of 3 GPUs across all interactive jobs simultaneously
- **Job Limit:** Up to 5 concurrent jobs per group/account

### Session Timeout
If your session is killed after 1 hour, you've hit the time limit. For longer jobs:
- Use batch jobs on other partitions (RTX3090, A6000, Normal)


### Request Specific Resources

Request resources for your interactive session:
```bash
# Basic resource request
salloc -p interactive --gres=gpu:1 --cpus-per-task=8 --mem=32G

# Run on a specific node
salloc -p interactive --nodelist=jrcai01 --gres=gpu:1 --cpus-per-task=8 --mem=32G
```

Adjust `--gres=gpu:N` for the number of GPUs, `--cpus-per-task` for CPU cores, `--mem` for memory, and `--nodelist` for a specific node as needed.





###  Interactive Code Testing
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

### Best Practices

1. **Exit when done:** Always exit your interactive session when finished to free up resources:
```bash
   exit
```
2. **Don't leave sessions idle:** Interactive sessions consume resources even when idle

3. **Use batch jobs for long runs:** If your job takes longer than 1 hour, submit it as a batch job to other partitions:
```bash
   sbatch -p RTX3090 my_long_job.sh
```
4. **Check resource availability:**
```bash
   sinfo -p interactive
```


## Troubleshooting

### Session Pending
If your interactive session is pending:
```bash
squeue -u $USER
```
Check the reason in the `NODELIST(REASON)` column:
- `Resources`: No available resources, wait or try fewer resources
- `Priority`: Other jobs have higher priority
- `MaxJobsPerAccount`: You've reached the 5 job limit per group/account



**Remember:** The interactive partition is for development, testing, and debugging. For production runs, use the appropriate batch partition (RTX3090, A6000, Normal).
