# 🖥️ SLURM Guide

A simple guide to using SLURM (Simple Linux Utility for Resource Management) on KFUPM clusters.

---

## 📖 Documentation

### 1. 🔗 [How to Connect](How_to_Connect.md)
Learn how to connect to the SLURM cluster using:
- SSH Terminal
- Visual Studio Code

### 2. ⚡ [How to Use SLURM](How_to_Use.md)
Complete guide covering:
- Monitoring commands (`sinfo`, `squeue`, `scontrol`)
- Job submission (`sbatch`, `srun`, `salloc`)
- Data transfer (`scp`, `sftp`)
- Account management (`spasswd`)

---

## 🚀 Quick Start

1. **Connect to cluster**: [Follow connection guide](How_to_Connect.md)
2. **Change password**: `spasswd`
3. **Check cluster status**: `sinfo`
4. **Submit a job**: `sbatch my_script.sh`
5. **Monitor jobs**: `squeue -u $USER`

---

*Need help? Start with [How to Connect](How_to_Connect.md), then move to [How to Use SLURM](How_to_Use.md).*
