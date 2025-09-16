**Interactive Jupyter Setup** (Debug/Testing Only):
```bash
# Step 1: Allocate resources
salloc --cpus-per-task=10 --mem=20G --gres=gpu:1

# Step 2: Start Jupyter
srun jupyter notebook --ip 10.22.xx.xx --port 8888
```

**Node IP Addresses:**

