---
icon: lucide/server
---

# SLURM Guide — KFUPM JRCAI

A simple guide to using SLURM (Simple Linux Utility for Resource Management) on KFUPM clusters.

[:octicons-arrow-right-24: How to Connect](how-to-connect.md){ .md-button .md-button--primary }
[View on GitHub :fontawesome-brands-github:](https://github.com/KFUPM-JRCAI/slurm-users-guide){ .md-button }

![Screenshot](assets/images/home/cluster-screenshot.png#only-light)
![Screenshot](assets/images/home/cluster-screenshot-dark.png#only-dark)

## Documentation

<div class="grid cards" markdown>

-   :material-server-network:{ .lg .middle } **Cluster Resources**

    ---

    Partitions, nodes, GPUs, and per-group job limits at a glance.

    [:octicons-arrow-right-24: Read more](cluster-resources.md)

-   :material-account-group:{ .lg .middle } **Group Policies**

    ---

    Storage quotas, job limits, and best practices for using the cluster responsibly.

    [:octicons-arrow-right-24: Read more](group-policies.md)

-   :material-connection:{ .lg .middle } **How to Connect**

    ---

    Reach the login node over SSH or Visual Studio Code Remote-SSH.

    [:octicons-arrow-right-24: Read more](how-to-connect.md)

-   :material-rocket-launch-outline:{ .lg .middle } **Using SLURM**

    ---

    Monitor the cluster, submit and manage jobs, move data, and manage your account.

    [:octicons-arrow-right-24: Read more](using-slurm/index.md)

-   :material-file-code-outline:{ .lg .middle } **Job Script Templates**

    ---

    Ready-to-copy `sbatch` templates for CPU and GPU jobs.

    [:octicons-arrow-right-24: Read more](job-script-templates.md)

-   :material-notebook-outline:{ .lg .middle } **Jupyter Access**

    ---

    Run Jupyter Notebook/Lab on a compute node, directly or through VS Code.

    [:octicons-arrow-right-24: Read more](jupyter-access.md)

-   :material-database-search-outline:{ .lg .middle } **Model Zoo**

    ---

    90+ ready-to-use HuggingFace models, including Arabic-specialized and multilingual LLMs.

    [:octicons-arrow-right-24: Read more](model-zoo.md)

</div>

## Basic SLURM Workflow

``` mermaid
graph TD
    Start([Start]) --> A[Write Job Script]
    Start --> A2[Need an Interactive Shell?]

    A --> B[Submit with sbatch]
    A2 --> B2[Request with salloc]

    B --> C[Job Queued - Status: PD]
    C --> D{Resources Available?}
    D -->|No| C
    D -->|Yes| E[Job Running - Status: R]
    E --> F[Job Complete]
    F --> G[Check Results]

    B2 --> C2[Job Queued - Status: PD]
    C2 --> D2{Resources Available?}
    D2 -->|No| C2
    D2 -->|Yes| E2[Interactive Shell on Compute Node]
    E2 --> F2[Run / Test Code Interactively]
    F2 --> G2[Exit Session]
```

## Getting Help

**Contact Information:**

- **System Administrator**: Contact JRCAI support team
- **Technical Issues**: mohammed.sinan@kfupm.edu.sa
- **Account Problems**: Submit ticket through proper channels

**Login Node**: (check your email/registration details)

---

*Last updated 6/9/2026 by Mohammed AlSinan (mohammed.sinan@kfupm.edu.sa)*
