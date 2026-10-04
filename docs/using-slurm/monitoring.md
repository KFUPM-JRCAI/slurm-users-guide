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
PARTITION   AVAIL  TIMELIMIT  NODES  STATE NODELIST
A100           up 2-00:00:00      1    mix server02
LoginNode    down   infinite      1  down* login01
A4500          up 2-00:00:00      1  down* jrcai21
A4500          up 2-00:00:00      2    mix jrcai[15-16]
A4500          up 2-00:00:00      1   idle jrcai14
RTX3090*       up 2-00:00:00      2 drain* jrcai[02,08]
RTX3090*       up 2-00:00:00      3    mix jrcai[01,06-07]
RTX3090*       up 2-00:00:00      2   idle jrcai[09-10]
A5000          up 2-00:00:00      2   idle jrcai[12-13]
A6000          up 2-00:00:00      3  down* jrcai[17,27],server01
A6000          up 2-00:00:00      3    mix jrcai[18-19,31]
A6000          up 2-00:00:00      9   idle jrcai[22,30,33-39]
interactive    up    2:00:00      2 drain* jrcai[02,08]
interactive    up    2:00:00      4  down* jrcai[17,21,27],server01
interactive    up    2:00:00      9    mix jrcai[01,06-07,15-16,18-19,31],server02
interactive    up    2:00:00     14   idle jrcai[09-10,12-14,22,30,33-39]
```

## `scontrol` — Detailed Control Information

Show detailed parameters of jobs, nodes, and partitions:

```bash title="Inspecting cluster objects"
# Show specific job details (replace 115 with your own job ID, shown by `squeue -u $USER`)
scontrol show job 115

# Show node information
scontrol show node

# Show partition details
scontrol show partition <partition>   # e.g. RTX3090

# Show all job information
scontrol show jobs

# View all available partitions
scontrol show partitions

# Check node specifications
scontrol show nodes
```

**Example job details:**

```title="scontrol show job output"
JobId=30060 JobName=qwen_captioning
   UserId=slurm_g202012345(10001) GroupId=grp_advisor1(20001) MCS_label=N/A
   Priority=4382 Nice=0 Account=grp_advisor1 QOS=normal
   JobState=RUNNING Reason=None Dependency=(null)
   Requeue=1 Restarts=0 BatchFlag=1 Reboot=0 ExitCode=0:0
   RunTime=00:42:08 TimeLimit=16:00:00 TimeMin=N/A
   SubmitTime=2026-10-04T13:43:23 EligibleTime=2026-10-04T13:43:23
   StartTime=2026-10-04T13:43:23 EndTime=2026-10-05T05:43:23 Deadline=N/A
   Partition=A100
   NodeList=server02
   NumNodes=1 NumCPUs=10 NumTasks=1 CPUs/Task=10
   ReqTRES=cpu=10,mem=64G,node=1,billing=36,gres/gpu=1
   Command=/SLURM/home/slurm_g202012345/my_project/run_training.slurm
   WorkDir=/SLURM/home/slurm_g202012345/my_project
   StdOut=/SLURM/home/slurm_g202012345/my_project/logs/job_out_30060.txt
   StdErr=/SLURM/home/slurm_g202012345/my_project/logs/job_err_30060.txt
```

## `squeue` — Job Queue Status

Query the status of jobs in the queue:

The default columns shown below (including `ACCOUNT` and `TRES_PER_NOD`) come from `cluster_env slurm` in `~/.bashrc`.

```bash title="Querying the queue"
# View all jobs
squeue

# View only your jobs
squeue -u <username>

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
   JOBID                 NAME  PARTITION                 USER              ACCOUNT       ST       TIME TRES_PER_NOD NODELIST(REASON)
   30060      qwen_captioning       A100     slurm_g202012345         grp_advisor1        R      41:44   gres/gpu:1         server02
   30034    volume_processing       A100     slurm_g202023456         grp_advisor2        R    4:19:50   gres/gpu:1         server02
   30023    data_preprocess_a      A4500     slurm_g202034567         grp_advisor3        R    6:07:29   gres/gpu:1          jrcai15
   30022    data_preprocess_b      A4500     slurm_g202034567         grp_advisor3        R    6:07:55   gres/gpu:1          jrcai16
```
