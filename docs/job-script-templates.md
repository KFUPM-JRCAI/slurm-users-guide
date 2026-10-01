---
icon: material/file-code-outline
---

# Job Script Templates

=== "Basic"

    ```bash title="basic_job.slurm" hl_lines="8"
    #!/bin/bash
    #SBATCH --job-name=my_job              # Job name
    #SBATCH --output=output_%j.txt         # Output file (%j = job ID)
    #SBATCH --error=error_%j.txt           # Error file (%j = job ID)
    #SBATCH --time=01:00:00                # Time limit (hh:mm:ss)
    #SBATCH --ntasks=1                     # Number of tasks
    #SBATCH --cpus-per-task=4              # CPU cores per task
    #SBATCH --partition=XXXX               # Partition (queue) name
    #SBATCH --mem=8G                       # Total memory limit

    # Activate environment
    source .bashrc
    conda activate myenv

    # Execute your program
    python my_script.py
    ```

    Replace `XXXX` with one of the partitions listed in [Cluster Resources](cluster-resources.md) — e.g. `RTX3090`, `A6000`, `A5000`, `A4500`, or `A100`.

=== "GPU"

    ```bash title="gpu_job.slurm" hl_lines="3 4 5"
    #!/bin/bash
    #SBATCH --job-name=gpu_job
    #SBATCH --partition=XXXXX              # Partition (queue) name
    #SBATCH --gres=gpu:1                   # Request 1 GPU
    #SBATCH --nodelist=NodeName            # Push to specific node (optional)
    #SBATCH --time=02:00:00                # Limit your job by time
    #SBATCH --mem=32G
    #SBATCH --output=gpu_output_%j.txt

    # Load modules and activate environment
    source .bashrc
    conda activate myenv

    # Run GPU-enabled program
    python gpu_script.py
    ```

    Replace `XXXXX` with one of the GPU partitions — e.g. `RTX3090`, `A6000`, `A5000`, `A4500`, or `A100`.

!!! tip

    See [Submitting Jobs](using-slurm/submitting-jobs.md) for how to submit these scripts with `sbatch`, and the [Interactive Jobs](using-slurm/interactive-jobs.md) page if you just need a quick interactive session instead.
