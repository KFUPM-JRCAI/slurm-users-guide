
**Basic Job Script Template:**
```bash
#!/bin/bash
#SBATCH --job-name=my_job              # Job name
#SBATCH --output=output_%j.txt         # Output file (%j = job ID)
#SBATCH --error=error_%j.txt           # Error file (%j = job ID)
#SBATCH --time=01:00:00               # Time limit (hh:mm:ss)
#SBATCH --ntasks=1                    # Number of tasks
#SBATCH --cpus-per-task=4             # CPU cores per task
#SBATCH --mem=8G                      # Total memory limit
#SBATCH --partition=debug             # Partition (queue) name

# Activate environment
source .bashrc
conda activate myenv

# Execute your program
python my_script.py
```

**GPU Job Script:**
```bash
#!/bin/bash
#SBATCH --job-name=gpu_job
#SBATCH --partition=gpu
#SBATCH --gres=gpu:1                  # Request 1 GPU
#SBATCH --time=02:00:00
#SBATCH --mem=32G
#SBATCH --output=gpu_output_%j.txt

# Load modules and activate environment
source .bashrc
conda activate myenv

# Run GPU-enabled program
python gpu_script.py
```
