# 🖥️ SLURM Guide - KFUPM JRCAI

**Simple Linux Utility for Resource Management**

Slurm is a workload manager designed for clusters. It efficiently schedules jobs and manages resources, ensuring fair and effective utilization of computational power.


## 📋 Table of Contents

- [Monitoring SLURM](#monitoring-slurm)
- [Utilizing SLURM](#utilizing_slurm)
- [Interactive Jobs](#interactive-jobs) 
- [Transferring Data](#transferring-data)
- [Account Commands](#account-commands)


###  Quick Reference Card

| **Category** | **Command** | **Purpose** |
|--------------|-------------|-------------|
| **Monitoring** | `sinfo` | Cluster status |
| | `scontrol show job ID` | Job details |
| | `squeue -u $USER` | Your jobs |
| **Utilizing** | `sbatch script.sh` | Submit batch job |
| | `salloc --cpus-per-task=4` | Interactive allocation |
| | `srun python script.py` | Execute command |
| **Account** | `spasswd` | Change password |
| | `userinfo` | View account expiration and disk quota |
| **Transferring** | `scp file.txt user@host:~/` | Upload file |
| | `sftp user@host` | Interactive transfer |

---

<details>
<summary>🔍 Monitoring SLURM</summary>

## Monitoring SLURM

Commands for checking cluster status, job information, and resource availability.

###  sinfo - Cluster Information
View cluster node information and partition status:

```bash
# Basic cluster info
sinfo

# Detailed node view
sinfo -N

# Partition summary
sinfo -s

# Node-specific details
sinfo -N -l
```

**Common Output:**
```
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
debug*       up   infinite      2   idle node[01-02]
gpu          up   infinite      1   idle gpu01
```

###  scontrol - Detailed Control Information
Show detailed parameters of jobs, nodes, and partitions:

```bash
# Show specific job details
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

**Example Job Details:**
```
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

###  squeue - Job Queue Status
Query the status of jobs in the queue:

```bash
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

**Job States:**
- **R**: Running
- **PD**: Pending
- **CG**: Completing
- **CD**: Completed
- **CA**: Cancelled
- **F**: Failed

**Example Output:**
```
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
  115     debug  my_job mohammed  R       5:42      1 node01
  116     debug test_job mohammed PD       0:00      1 (Resources)
```

</details>

---

<details>
<summary>⚡ Utilizing SLURM</summary>

## Utilizing SLURM

Commands for submitting and running jobs on the cluster.

###  sbatch - Submit Batch Jobs


 Basic job submission ( all options can be included inside the script)
```bash
sbatch my_script.slurm
```


You may also specify the required options directly within the command. Modify them as appropriate.
```
sbatch --partition=PartitionName --gres=gpu:N --nodelist=NodeName my_script.slurm
```

> **📋 View Script Samples:** [Job Script Templates](Job_Script_Templates.md)




### Job Control Commands:
```bash
# Cancel a job
scancel 115

# Cancel all your jobs
scancel -u username

# Hold a job
scontrol hold 115

# Release a held job
scontrol release 115
```

</details>

---

<details>
<summary>📖 Interactive Partition Guide</summary>

## Interactive Jobs 

#### Partition Details & Limits

- **Partition Name:** `interactive`
- **Available Nodes:** All compute nodes 
- **Time Limit:** Jobs are limited to 1 hour
- **GPU Limit:** Maximum of 3 GPUs across all interactive jobs simultaneously
- **Job Limit:** Up to 5 concurrent jobs per group/account

> **IMPORTANT NOTE**
> 
> 
> Sessions on the interactive partition automatically **terminate after 1 hour.** This partition is designed for development, testing, and debugging only.
> 
> **For longer or production jobs**, submit to the appropriate batch partitions



#### Request Specific Resources

Request resources for your interactive session:
```bash
# Basic resource request
salloc -p interactive --gres=gpu:1 --cpus-per-task=8 --mem=32G

# Run on a specific node
salloc -p interactive --nodelist=jrcai01 --gres=gpu:1 --cpus-per-task=8 --mem=32G
```

Adjust `--gres=gpu:N` for the number of GPUs, `--cpus-per-task` for CPU cores, `--mem` for memory, and `--nodelist` for a specific node as needed.

#### Interactive Code Testing
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

#### Best Practices for Interactive

1. **Exit when done:** Always exit your interactive session when finished to free up resources:
```bash
   exit
```

2. **Don't leave sessions idle:** Interactive sessions consume resources even when idle

3. **Use batch jobs for long runs:** If your job takes longer than 1 hour, submit it as a batch job to other partitions:
```bash
   sbatch -p RTX3090 my_long_job.sh
```



</details>

---


<details>
<summary>👨🏻‍💻 Account Commands</summary>

## Account Commands

Commands for managing your SLURM cluster account and authentication.

###  spasswd - Change SLURM Password
**Important**: The standard `passwd` command does not work for SLURM users. Always use `spasswd`:

```bash
# Change your SLURM password
spasswd
```
###  userinfo - check your expiration date and disk quota

```bash
userinfo
```


</details>

---

<details>
   
<summary>📁 Transferring Data</summary>

## Transferring Data

Commands for moving files between your local machine and the cluster.

###  scp - Secure Copy Protocol
Transfer files and directories securely:

#### **Upload to Cluster**
```bash
# Upload a single file
scp file.txt username@LoginNodeIP:~/

# Upload a directory
scp -r /local/directory username@LoginNodeIP:~/destination/

# Upload with specific destination
scp -r /Downloads/my-project mohammed_slurm@LoginNodeIP:~/data/

# Upload to specific path
scp dataset.csv mohammed_slurm@1LoginNodeIP:/home/mohammed_slurm/projects/
```

#### **Download from Cluster**
```bash
# Download a file
scp username@LoginNodeIP:~/results.txt ./

# Download a directory
scp -r username@LoginNodeIP:~/output/ ./local-results/

# Download with specific source
scp mohammed_slurm@LoginNodeIP:~/data/processed_data.csv ./
```




### 📋 sftp - Secure File Transfer Protocol
Interactive file transfer with more features:

```bash
# Connect to cluster
sftp username@10.22.188.36

# SFTP commands once connected:
sftp> pwd                    # Show remote directory
sftp> lpwd                   # Show local directory
sftp> ls                     # List remote files
sftp> lls                    # List local files
sftp> cd remote-directory    # Change remote directory
sftp> lcd local-directory    # Change local directory

# Transfer files
sftp> put local-file.txt     # Upload file
sftp> get remote-file.txt    # Download file
sftp> put -r local-dir/      # Upload directory
sftp> get -r remote-dir/     # Download directory

# Exit
sftp> quit
```

#### **Useful SFTP Commands**
```bash
# Create remote directory
sftp> mkdir new-directory

# Remove remote file
sftp> rm unwanted-file.txt

# Remove remote directory
sftp> rmdir empty-directory

# Show file permissions
sftp> ls -la

# Change permissions
sftp> chmod 755 script.sh
```


#### **Common Issues:**
- **Permission denied**: Check your username and password
- **Host key verification**: Accept the host key on first connection
- **Large files**: Consider using `rsync` for better performance
- **Windows users**: Use full paths, not `~/`

</details>

---

