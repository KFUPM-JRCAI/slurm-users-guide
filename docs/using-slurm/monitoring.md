---
icon: material/monitor-dashboard
---

# Monitoring SLURM

Commands for checking cluster status, job information, and resource availability.

## `sinfo` — Cluster Information

View cluster node information and partition status:

```bash title="Cluster overview"
# Basic cluster info
sinfo

# Detailed node view
sinfo -N

# Partition summary
sinfo -s

# Node-specific details
sinfo -N -l
```

**Common output:**

```title="sinfo output"
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
debug*       up   infinite      2   idle node[01-02]
gpu          up   infinite      1   idle gpu01
```

## `scontrol` — Detailed Control Information

Show detailed parameters of jobs, nodes, and partitions:

```bash title="Inspecting cluster objects"
# Show specific job details (replace 115 with your own job ID, shown by `squeue -u $USER`)
scontrol show job 115

# Show node information
scontrol show node

# Show partition details
scontrol show partition XXXXX

# Show all job information
scontrol show jobs

# View all available partitions
scontrol show partitions

# Check node specifications
scontrol show nodes
```

**Example job details:**

```title="scontrol show job output"
JobId=115 JobName=my_job
   UserId=mohammed_slurm(1001) GroupId=users(100) MCS_label=N/A
   Priority=4294901758 Nice=0 Account=(null) QOS=normal
   JobState=RUNNING Reason=None Dependency=(null)
   Requeue=1 Restarts=0 BatchFlag=1 Reboot=0 ExitCode=0:0
   RunTime=00:05:42 TimeLimit=01:00:00 TimeMin=N/A
   SubmitTime=2025-10-08T10:30:15 EligibleTime=2025-10-08T10:30:15
   StartTime=2025-10-08T10:30:17 EndTime=2025-10-08T11:30:17 Deadline=N/A
   WorkDir=/home/mohammed_slurm
   StdOut=/home/mohammed_slurm/output_115.txt
   StdErr=/home/mohammed_slurm/error_115.txt
```

## `squeue` — Job Queue Status

Query the status of jobs in the queue:

```bash title="Querying the queue"
# View all jobs
squeue

# View only your jobs
squeue -u username

# View specific job
squeue -j 115

# View jobs by name
squeue --name=my_job

# Detailed format
squeue -o "%.10i %.20j %.10P %.10a %.8C %.30b %.10M %.10m %.10R"
```

**Job states:**

| State | Meaning |
|-------|---------|
| `R` | Running |
| `PD` | Pending |
| `CG` | Completing |
| `CD` | Completed |
| `CA` | Cancelled |
| `F` | Failed |

**Example output:**

```title="squeue output"
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
  115     debug  my_job mohammed  R       5:42      1 node01
  116     debug test_job mohammed PD       0:00      1 (Resources)
```
