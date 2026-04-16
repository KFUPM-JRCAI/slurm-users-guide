# 🖥️ How To Run Jupyter Notebook on Slurm

This guide explains how to run Jupyter Notebook on a Slurm HPC cluster using two different methods.

---

## 🔹Method 1: Run Jupyter Notebook Directly on Slurm

### 1. Access the Cluster

Connect to the Slurm login node via SSH.

![Slurm login](https://i.imgur.com/rJGis7n.png)

---

### 2. Check Available Resources

View available and idle nodes:

```bash
sinfo 
```
![Slurm login](https://i.imgur.com/ZD2UWg7.png)

---

### 3. Request a Compute Node

Allocate a compute node:

```bash
salloc -p interactive --gres=gpu:1 --cpus-per-task=8 --mem=32G
```
Optionally, You can optionally specify a node (e.g., jrcai01) if required and available.

Wait until the job is allocated — you will be automatically placed on the compute node.

![Image](https://i.imgur.com/H9roN2h.png)

After the allocation is granted, you will be redirected to the assigned compute node (e.g., jrcai01).

---

### 4. Start Jupyter Notebook / Lab

```bash
jupyter start
```
Wait for initialization. Once ready, a URL with a token will be printed in the terminal.

![Image](https://i.imgur.com/QAO66Va.png)

---

### 5. Access Jupyter

Open the generated URL in your browser.

**After opening the generated URL in your browser, the Jupyter Notebook interface will be available, allowing you to create and manage notebooks**.

![Image](https://i.imgur.com/RO5tKNY.png)

---

### 6. How to logout

| Action              | Description              |
|---------------------|--------------------------|
| Suspend the process | Ctrl + Z                |
| Logging out         | Logout from the server  |

---



## 🔹Method 2: Run Jupyter with VS Code

### 1. Connect to the Cluster

Connect to the Slurm login node via SSH.

![Slurm login](https://i.imgur.com/rJGis7n.png)

---

### 2. Request a Compute Node

Allocate a compute node:

```bash
salloc -p interactive --gres=gpu:1 --cpus-per-task=8 --mem=32G
```
Optionally, You can optionally specify a node (e.g., jrcai01) if required and available.

Wait until the job is allocated — you will be automatically placed on the compute node.

![Image](https://i.imgur.com/H9roN2h.png)

After the allocation is granted, you will be redirected to the assigned compute node (e.g., jrcai01).

---

### 3. Prepare Your Working Directory

```bash
mkdir <project_dir>
cd <project_dir>
```

### 4. Activate Your Environment

**Option A — Conda:**

```bash
conda activate <env_name>
```

**Option B — Python venv:**

```bash
source venv/bin/activate
```

**As you can see, the virtual environment has been activated successfully. This is indicated by (venv) appearing in the terminal prompt.**

![Image](https://i.imgur.com/zVxgM41.png)

---

### 5. Start Jupyter (No Browser Mode)

```bash
jupyter notebook --no-browser --port=<PORT> --ip=0.0.0.0
```

Choose a port in the ephemeral range: **49152–49999**

**The terminal will print a token-based URL — copy it for the next steps.**

![Image](https://i.imgur.com/neE5H2F.png)

---

### 6. Connect from VS Code

1. Open VS Code
2. Open your `.ipynb` file
3. Click **Select Kernel** (top-right corner)
4. Choose **Existing Jupyter Server**
5. Paste the token URL generated in Step 5
6. Select Kernel

### 7. Access Jupyter

**Now, the connection to the remote Jupyter server is established, and you can run and edit notebooks directly in VS Code.**

![Image](https://i.imgur.com/q670ZPk.png)

## Useful Diagnostic Commands

Run these inside a notebook cell to verify your environment:

```python
!hostname      # shows the current compute node you are running on
!which python  # shows the active Python environment
!nvidia-smi    # shows the GPU(s) allocated to your session
```
