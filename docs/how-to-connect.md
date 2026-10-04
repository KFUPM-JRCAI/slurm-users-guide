---
icon: material/connection
---

# How to Connect

Slurm (Simple Linux Utility for Resource Management) is a workload manager designed for clusters. It efficiently schedules jobs and manages resources, ensuring fair and effective utilization of computational power.

To use SLURM, you need to connect to the login node of the cluster. Pick whichever method fits your workflow:

=== "Terminal"

    For all operating systems, the command to connect via terminal is the same:

    ```bash title="Connect over SSH"
    ssh <username>@<login-node>
    ```

    **Example connection process:**

    ![SLURM SSH Connection Example](assets/images/how-to-connect/ssh-connection-example.png)

=== "Visual Studio Code"

    If you have VS Code installed, use the Remote-SSH extension to connect to the login node.

    [Download VS Code :material-download:](https://code.visualstudio.com/download){ .md-button }

    1.  **Install the Remote-SSH extension**

        ![Remote SSH Extension Installation](assets/images/how-to-connect/vscode-remote-ssh-install.png)

    2.  **Open the Remote Window menu**

        ![Remote Window Button](assets/images/how-to-connect/vscode-remote-window-button.png)

    3.  **Add a new SSH host**

        ![Add New SSH Host](assets/images/how-to-connect/vscode-add-ssh-host.png)

    4.  **Enter the connection details**, `<username>@<login-node>`

        ![Enter SSH Command](assets/images/how-to-connect/vscode-enter-ssh-command.png)

    5.  **Select the SSH config file** to save the host to

        ![Select SSH Config File](assets/images/how-to-connect/vscode-select-ssh-config.png)

    6.  **Open the Remote Window menu again** and connect to the host

        ![Connect to Host Menu](assets/images/how-to-connect/vscode-remote-window-button.png)

    7.  **Select your host** from the list

        ![Select Host](assets/images/how-to-connect/vscode-select-host.png)

    8.  **Authenticate** with your password

        ![Password Authentication](assets/images/how-to-connect/vscode-password-auth.png)

    9.  **Open your home directory**

        ![Open Folder Option](assets/images/how-to-connect/vscode-open-folder.png)
        ![File Explorer](assets/images/how-to-connect/vscode-file-explorer.png)

!!! warning

    Login nodes are for access only and should not be used to run scripts or computational workloads. Submit jobs with `sbatch`/`salloc` instead — see [Using SLURM](using-slurm/index.md).
