# Configuring SLURM

The standard cluster images come with SLURM installed and configured for 2 compute nodes named `node01` and `node02`.

For ease of management, the SLURM configuration files are located in a shared persistent location per cluster on the director that is then mounted onto all the systems within that cluster. This is accessible via `/export/sharedpersistent/CLUSTERNAME/etc/slurm/`. 

To make permanent changes to SLURM configuration:
1. Make whatever desired changes are needed to `slurm.conf` under the shared storage directory
1. Reload changes (from the cluster's controller system)
    ```bash
    scontrol reconfigure
    ```
