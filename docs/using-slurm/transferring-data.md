---
icon: material/folder-swap-outline
---

# Transferring Data

Commands for moving files between your local machine and the cluster.

=== "scp"

    Secure Copy Protocol — transfer files and directories securely.

    **Upload to cluster:**

    ```bash
    # Upload a single file
    scp file.txt <username>@<login-node>:~/

    # Upload a directory
    scp -r /local/directory <username>@<login-node>:~/destination/

    # Upload with specific destination
    scp -r /Downloads/my-project mohammed_slurm@<login-node>:~/data/

    # Upload to specific path
    scp dataset.csv mohammed_slurm@<login-node>:/home/mohammed_slurm/projects/
    ```

    **Download from cluster:**

    ```bash
    # Download a file
    scp <username>@<login-node>:~/results.txt ./

    # Download a directory
    scp -r <username>@<login-node>:~/output/ ./local-results/

    # Download with specific source
    scp mohammed_slurm@<login-node>:~/data/processed_data.csv ./
    ```

=== "sftp"

    Secure File Transfer Protocol — interactive file transfer with more features.

    ```bash
    # Connect to cluster
    sftp <username>@<login-node>

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

    **Useful SFTP commands:**

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

??? info "Common issues"

    - **Permission denied**: Check your username and password
    - **Host key verification**: Accept the host key on first connection
    - **Large files**: Consider using `rsync` for better performance
    - **Windows users**: Use full paths, not `~/`
