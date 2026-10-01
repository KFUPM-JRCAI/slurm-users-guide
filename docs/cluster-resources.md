---
icon: material/server-network
---

# Cluster Resources

An overview of the partitions and nodes on the KFUPM JRCAI cluster. See [Group Policies](group-policies.md) for storage quotas and job limits.

## Partition Details

| Partition | Purpose | Default / Max Time | Nodes | GPUs |
|-----------|---------|--------------------|-------|------|
| **A100** | Large models | 16 h / 2 days | server02 | 6× A100 (80 GB) |
| **RTX3090** (Default) | GPU computing | 16 h / 2 days | jrcai[01-02,06-10] | 19× RTX 3090 (24 GB) |
| **A6000** | GPU computing | 16 h / 2 days | jrcai[17-19], server01 | 14× A6000 (48 GB) |
| **A5000** | GPU computing | 16 h / 2 days | jrcai[12-13] | 8× A5000 (24 GB) |
| **A4500** | GPU computing | 16 h / 2 days | jrcai[14-16,21] | 8× RTX A4500 (20 GB) |
| **interactive** | Short interactive sessions | 2 h / 2 h | all compute nodes | any available |
| **LoginNode** | Access only | - | login01 | Login access |

!!! note

    If you don't specify a partition, your job runs on **RTX3090** (the default). Jobs get a **16 h** time limit if you don't request one, up to a **2-day** maximum.

!!! warning

    Login nodes (login01) are for access only and should not be used to run scripts or computational workloads.

## Available Nodes and Their Resources

| Node | Partition | GPUs | GPU Type | VRAM | CPUs | Memory |
|------|-----------|------|----------|------|------|--------|
| server02 | A100 | 6 | A100 | 80 GB | 255 | ~2 TB |
| server01 | A6000 | 8 | A6000 | 48 GB | 64 | ~1 TB |
| jrcai17 \* | A6000 | 2 | A6000 | 48 GB | 28 | ~256 GB |
| jrcai18 | A6000 | 2 | A6000 | 48 GB | 32 | ~256 GB |
| jrcai19 | A6000 | 2 | A6000 | 48 GB | 32 | ~256 GB |
| jrcai01 | RTX3090 | 2 | RTX 3090 | 24 GB | 48 | ~64 GB |
| jrcai02 | RTX3090 | 2 | RTX 3090 | 24 GB | 48 | ~64 GB |
| jrcai06 | RTX3090 | 3 | RTX 3090 | 24 GB | 64 | ~256 GB |
| jrcai07 | RTX3090 | 3 | RTX 3090 | 24 GB | 64 | ~256 GB |
| jrcai08 | RTX3090 | 3 | RTX 3090 | 24 GB | 64 | ~256 GB |
| jrcai09 | RTX3090 | 3 | RTX 3090 | 24 GB | 64 | ~256 GB |
| jrcai10 | RTX3090 | 3 | RTX 3090 | 24 GB | 64 | ~256 GB |
| jrcai12 | A5000 | 4 | A5000 | 24 GB | 64 | ~256 GB |
| jrcai13 | A5000 | 4 | A5000 | 24 GB | 64 | ~256 GB |
| jrcai14 | A4500 | 2 | RTX A4500 | 20 GB | 32 | ~256 GB |
| jrcai15 | A4500 | 2 | RTX A4500 | 20 GB | 32 | ~256 GB |
| jrcai16 | A4500 | 2 | RTX A4500 | 20 GB | 20 | ~128 GB |
| jrcai21 | A4500 | 2 | RTX A4500 | 20 GB | 20 | ~128 GB |

\* jrcai17 is configured but not yet in service (`State=FUTURE`).

**Total: ~55 GPUs across 18 compute nodes.**

!!! tip

    Click a column header to sort a table by that column.
