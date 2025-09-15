# 🖥️ How To Use SLURM (Connect)



Slurm (Simple Linux Utility for Resource Management) is a workload manager designed for clusters. It efficiently schedules jobs and manages resources, ensuring fair and effective utilization of computational power. Here's a brief guide to help you get started with Slurm.

To use SLURM, you will need to connect to the login node of your high-performance computing (HPC) cluster. Here's how to do it:

---

## 🔗 Remote Access to the Login Node

### A. Terminal/Command Line

For all OS systems, the command used to connect to the workstation via terminal is the same:

**Command:**
```bash
ssh username@LoginNodeIP
```

**Example connection process:**
![SLURM SSH Connection Example](https://i.imgur.com/WcV0U5k.png)

---

### B. Visual Studio Code (Optional)

For all OS systems, if you have VS Code installed, then follow these steps to connect to your workstation via VS Code. 

**Download VS Code:** https://code.visualstudio.com/download

Once you run VS Code, follow these steps to connect to your workstation:

#### Step 1: Install Remote SSH Extension

![Remote SSH Extension Installation](https://i.imgur.com/uhVeOvD.png)


#### Step 2: Connect to Remote Host


![Remote Window Button](https://i.imgur.com/kQ2BYjJ.png)


#### Step 3: Add New SSH Host


![Add New SSH Host](https://i.imgur.com/5BfceHp.png)



#### Step 4: Enter Connection Details `username@workstation_ip_address`


![Enter SSH Command](https://i.imgur.com/rtHqdS8.png)


#### Step 5: Select Configuration File



![Select SSH Config File](https://i.imgur.com/iT153zV.png)


#### Step 6: Connect to Host


![Connect to Host Menu](https://i.imgur.com/kQ2BYjJ.png)


#### Step 7: Select Your Host

![Select Host](https://i.imgur.com/AWKicFl.png)


#### Step 8: Authentication


![Password Authentication](https://i.imgur.com/aHhiXdv.png)



#### Step 10: Open Home Directory

![Open Folder Option](https://i.imgur.com/K8ydg4t.png)

![File Explorer](https://i.imgur.com/DCwZ95e.png)




*Ready to start using SLURM? Check out our complete [SLURM Commands Guide](README.md) for job submission and management!*
