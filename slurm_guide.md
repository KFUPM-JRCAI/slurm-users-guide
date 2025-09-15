# 🖥️ SLURM (Simple Linux Utility for Resource Management) Guide

Welcome to the comprehensive SLURM guide! This document will help you master job scheduling and resource management on HPC clusters.

---

<details>
<summary>📋 Table of Contents</summary>

- [What is SLURM?](#what-is-slurm)
- [Getting Started](#getting-started)
- [Basic Commands](#basic-commands)
- [Job Submission](#job-submission)
- [Job Management](#job-management)
- [Resource Allocation](#resource-allocation)
- [Advanced Features](#advanced-features)
- [Troubleshooting](#troubleshooting)

</details>

---

<details>
<summary>🤔 What is SLURM?</summary>

## What is SLURM?

SLURM is an open-source cluster management and job scheduling system for large and small Linux clusters. It provides three key functions:

1. **Resource Allocation** - Allocates exclusive/non-exclusive access to resources (compute nodes)
2. **Job Management** - Provides a framework for starting, executing, and monitoring work
3. **Queue Management** - Manages a queue of pending work

### Key Benefits
- Fault-tolerant and highly scalable
- Open source with no vendor lock-in
- Supports various interconnects and resource types
- Extensive configuration options

</details>

---

<details>
<summary>🚀 Getting Started</summary>

## Getting Started

### Prerequisites
- Access to a SLURM-enabled cluster
- Basic Linux command line knowledge
- SSH client for remote access

### First Steps
1. **Login to the cluster**
   ```bash
   ssh username@cluster.example.com
   ```

2. **Check cluster status**
   ```bash
   sinfo
   ```

3. **View your account information**
   ```bash
   sacctmgr show user $USER
   ```

### Environment Setup
- Most clusters load SLURM commands automatically
- If not available, try: `module load slurm`

</details>

---

<details>
<summary>⌨️ Basic Commands</summary>

## Basic Commands

### Cluster Information
| Command | Description | Example |
|---------|-------------|---------|
| `sinfo` | Display cluster node information | `sinfo -N` |
| `squeue` | Show job queue | `squeue -u $USER` |
| `scontrol` | Administrative tool | `scontrol show node` |
| `sacct` | Display job accounting data | `sacct -j 12345` |

### Quick Status Checks
```bash
# View all nodes
sinfo

# View only your jobs
squeue -u $USER

# View detailed node information
scontrol show nodes

# Check partition information
sinfo -s
```

### Useful Aliases
Add these to your `~/.bashrc`:
```bash
alias myq='squeue -u $USER'
alias nodes='sinfo -N'
alias partitions='sinfo -s'
```

</details>

---

<details>
<summary>📝 Job Submission</summary>

## Job Submission

### Interactive Jobs
Start an interactive session:
```bash
# Basic interactive job
srun --pty bash

# Interactive job with specific resources
srun -p compute -t 01:00:00 -c 4 --mem=8GB --pty bash

# Interactive job with GPU
srun -p gpu --gres=gpu:1 -t 02:00:00 --pty bash
```

### Batch Jobs
Submit jobs using `sbatch`:

#### Basic Job Script
```bash
#!/bin/bash
#SBATCH --job-name=my_job
#SBATCH --output=output_%j.txt
#SBATCH --error=error_%j.txt
#SBATCH --time=01:00:00
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=4
#SBATCH --mem=8GB
#SBATCH --partition=compute

# Your commands here
echo "Job started at: $(date)"
echo "Running on node: $(hostname)"

# Example: Python script
python my_script.py

echo "Job finished at: $(date)"
```

#### Submit the job
```bash
sbatch job_script.sh
```

### Common SBATCH Directives
```bash
#SBATCH --job-name=my_job           # Job name
#SBATCH --output=output_%j.txt      # Output file (%j = job ID)
#SBATCH --error=error_%j.txt        # Error file
#SBATCH --time=01:30:00             # Time limit (HH:MM:SS)
#SBATCH --ntasks=1                  # Number of tasks
#SBATCH --cpus-per-task=4           # CPUs per task
#SBATCH --mem=16GB                  # Memory per node
#SBATCH --partition=compute         # Partition name
#SBATCH --nodes=2                   # Number of nodes
#SBATCH --mail-type=END,FAIL        # Email notifications
#SBATCH --mail-user=you@email.com   # Your email
```

</details>

---

<details>
<summary>🔧 Job Management</summary>

## Job Management

### Monitoring Jobs
```bash
# View all your jobs
squeue -u $USER

# View specific job details
scontrol show job 12345

# Monitor job progress
watch squeue -u $USER

# Check job history
sacct -j 12345 --format=JobID,JobName,State,ExitCode,Start,End
```

### Job Control
```bash
# Cancel a job
scancel 12345

# Cancel all your jobs
scancel -u $USER

# Cancel jobs by name
scancel --name=my_job

# Hold a job (prevent it from running)
scontrol hold 12345

# Release a held job
scontrol release 12345
```

### Job Arrays
Submit multiple similar jobs:
```bash
#!/bin/bash
#SBATCH --job-name=array_job
#SBATCH --array=1-10
#SBATCH --output=output_%A_%a.txt
#SBATCH --time=00:30:00

# Use $SLURM_ARRAY_TASK_ID in your script
echo "This is array task $SLURM_ARRAY_TASK_ID"
python process_file_${SLURM_ARRAY_TASK_ID}.py
```

</details>

---

<details>
<summary>🎛️ Resource Allocation</summary>

## Resource Allocation

### CPU Resources
```bash
# Single core job
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1

# Multi-core job (shared memory)
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=16

# MPI job (distributed memory)
#SBATCH --ntasks=32
#SBATCH --ntasks-per-node=16
```

### Memory Allocation
```bash
# Memory per node
#SBATCH --mem=32GB

# Memory per CPU
#SBATCH --mem-per-cpu=4GB

# Example: 8 CPUs × 4GB = 32GB total
#SBATCH --cpus-per-task=8
#SBATCH --mem-per-cpu=4GB
```

### GPU Resources
```bash
# Request 1 GPU
#SBATCH --gres=gpu:1

# Request specific GPU type
#SBATCH --gres=gpu:tesla:2

# GPU with specific partition
#SBATCH --partition=gpu
#SBATCH --gres=gpu:v100:1
```

### Storage and I/O
```bash
# Request temporary storage
#SBATCH --tmp=100GB

# Specify working directory
#SBATCH --chdir=/path/to/workdir

# Copy files to local storage
cp $HOME/input/* $SLURM_TMPDIR/
cd $SLURM_TMPDIR
# ... run computations ...
cp results/* $HOME/output/
```

</details>

---

<details>
<summary>⚡ Advanced Features</summary>

## Advanced Features

### Job Dependencies
```bash
# Submit job that waits for another to complete
sbatch --dependency=afterok:12345 dependent_job.sh

# Multiple dependencies
sbatch --dependency=afterok:12345:12346 next_job.sh

# Chain of jobs
JOB1=$(sbatch --parsable first_job.sh)
JOB2=$(sbatch --parsable --dependency=afterok:$JOB1 second_job.sh)
sbatch --dependency=afterok:$JOB2 final_job.sh
```

### Exclusive Access
```bash
# Get exclusive access to nodes
#SBATCH --exclusive

# Exclude specific nodes
#SBATCH --exclude=node001,node002

# Request specific nodes
#SBATCH --nodelist=node010,node011
```

### Environment Modules
```bash
# In your job script
module load python/3.9
module load cuda/11.8
module load openmpi/4.1

# List available modules
module avail

# Show loaded modules
module list
```

### Container Jobs
```bash
# Singularity container
srun singularity exec container.sif python script.py

# Docker with Singularity
srun singularity exec docker://tensorflow/tensorflow:latest python script.py
```

</details>

---

<details>
<summary>🐛 Troubleshooting</summary>

## Troubleshooting

### Common Issues

#### Job Stuck in Queue
```bash
# Check why job is pending
squeue -j 12345 --start

# View detailed job information
scontrol show job 12345

# Check partition limits
scontrol show partition compute
```

#### Out of Memory Errors
```bash
# Check job memory usage
sacct -j 12345 --format=JobID,MaxRSS,MaxVMSize

# Monitor real-time usage
sstat -j 12345 --format=AveCPU,AveRSS,MaxRSS
```

#### Job Failures
```bash
# Check job exit status
sacct -j 12345 --format=JobID,State,ExitCode

# View job details
scontrol show job 12345

# Check error logs
cat error_12345.txt
```

### Performance Tips

1. **Right-size your jobs**
   - Don't request more resources than needed
   - Use `sacct` to analyze past job usage

2. **Use appropriate time limits**
   - Shorter jobs often get scheduled faster
   - Use checkpointing for long jobs

3. **Optimize I/O**
   - Use local storage (`$SLURM_TMPDIR`) for temporary files
   - Minimize small file operations

4. **Profile your code**
   ```bash
   # Time your job
   time python script.py
   
   # Use profiling tools
   srun --profile=task python script.py
   ```

### Getting Help
```bash
# SLURM documentation
man sbatch
man squeue
man scancel

# Cluster-specific help
# Check your cluster's documentation or contact support
```

</details>

---

<details>
<summary>📚 Quick Reference</summary>

## Quick Reference

### Essential Commands Cheat Sheet
| Task | Command |
|------|---------|
| Submit job | `sbatch script.sh` |
| Interactive session | `srun --pty bash` |
| Check queue | `squeue -u $USER` |
| Job details | `scontrol show job JOBID` |
| Cancel job | `scancel JOBID` |
| Node info | `sinfo -N` |
| Account info | `sacctmgr show user $USER` |
| Job history | `sacct -j JOBID` |

### Example Job Scripts

#### CPU-only Job
```bash
#!/bin/bash
#SBATCH --job-name=cpu_job
#SBATCH --time=02:00:00
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=8
#SBATCH --mem=16GB
#SBATCH --output=output_%j.txt

module load python/3.9
python cpu_intensive_script.py
```

#### GPU Job
```bash
#!/bin/bash
#SBATCH --job-name=gpu_job
#SBATCH --partition=gpu
#SBATCH --gres=gpu:1
#SBATCH --time=04:00:00
#SBATCH --mem=32GB
#SBATCH --output=gpu_output_%j.txt

module load cuda/11.8
module load python/3.9
python gpu_script.py
```

#### MPI Job
```bash
#!/bin/bash
#SBATCH --job-name=mpi_job
#SBATCH --ntasks=32
#SBATCH --ntasks-per-node=16
#SBATCH --time=01:00:00
#SBATCH --output=mpi_output_%j.txt

module load openmpi/4.1
mpirun ./mpi_program
```

</details>

---

## 🎯 Summary

This guide covers the essential aspects of using SLURM for job scheduling and resource management. Start with basic commands, practice job submission, and gradually explore advanced features as your needs grow.

**Key Takeaways:**
- Always specify resource requirements accurately
- Monitor your jobs and optimize based on usage
- Use appropriate partitions and time limits
- Keep your job scripts organized and documented

For cluster-specific information, consult your local documentation or system administrators.

---

*Last updated: September 2025*