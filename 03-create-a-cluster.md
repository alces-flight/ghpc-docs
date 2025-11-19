# Create a Cluster

Up to 8 clusters supported within site network. IP address ranges per cluster are in the 10.10.0.0/19 range, moving onto the next for each cluster, e.g.:
- First Cluster: 10.10.0.1 - 10.10.31.254
- Second Cluster: 10.10.32.1 - 10.10.63.254

The Controller node requires an IP on the Domain Network. This can be anything in the 10.178.0.0/19 range. The range 10.178.31.1 - 10.178.31.254 is reserved for network booting.

1. Generate cluster & controller configuration (TO DO: Probs could auto-step through Domain Network & Cluster Network assignment to reduce chance for input issues?)
    - This requires the following information:
        - `CLUSTERNAME`: Your chosen name for the cluster you're creating
        - `CONTROLLER_ADMIN_IP`: The IP address for the controller to use on the admin subnet (10.178.0.0/19) of the GHPC network
        - `CONTROLLER_ADMIN_INTERFACE`: The interface name of the controller which will be assigned the admin IP above
        - `CONTROLLER_PRIMARY_IP`: The IP address for the controller to use on the cluster subnet (something in the 10.10.0.0/19 ranges mentioned at beginning of this doc) of the GHPC network
        - `CONTROLLER_PRIMARY_INTERFACE`: The interface name of the controller which will be assigned the primary IP above
        - `CONTROLLER_BOOT_MAC`: The MAC address of the interface that the controller will boot off of
    - _Note: if using a VM for controller, jump to step 4 for VM creation reference and see **Identifying Nodes** document for getting MAC address and interface name_
    - Configuring cluster and controller like so
        ```bash
        CLUSTERNAME=cluster1
        CONTROLLER_ADMIN_IP=10.178.2.1
        CONTROLLER_ADMIN_INTERFACE=enp1s0
        CONTROLLER_PRIMARY_IP=10.10.0.1
        CONTROLLER_PRIMARY_INTERFACE=enp2s0
        CONTROLLER_BOOT_MAC=AA:BB:CC:DD:EE:FF

        cd /root/personality/
        bash new_cluster.sh $CLUSTERNAME $CONTROLLER_ADMIN_IP $CONTROLLER_ADMIN_INTERFACE $CONTROLLER_PRIMARY_IP $CONTROLLER_PRIMARY_INTERFACE $CONTROLLER_BOOT_MAC
        ```
2. Generate NFS boot environment and configuration for login node
    - This requires the following information:
        - `CLUSTERNAME`: The name of the cluster used in the previous step
        - `LOGIN_NAME`: The short hostname to be used by the login node
        - `LOGIN_BOOT_MAC`: The MAC address of the interface that the login node will boot off of
        - `LOGIN_PRIMARY_IP`: The IP address for the login node to use on the cluster subnet (something in the 10.10.0.0/19 ranges mentioned at beginning of this doc) of the GHPC network
        - `LOGIN_PRIMARY_INTERFACE`: The interface name of the login node which will be assigned the primary IP above
        - `EXTERNAL_INTERFACE`: The interface name of the login node which is connected to the External network
    - Generating login node configuration like so
        ```bash
        CLUSTERNAME=cluster1
        LOGIN_NAME=login1
        LOGIN_BOOT_MAC=AA:BB:CC:DD:EE:FF
        LOGIN_PRIMARY_IP=10.10.4.1
        LOGIN_PRIMARY_INTERFACE=enp1s0
        EXTERNAL_INTERFACE=enp2s0

        cd /root/personality/
        bash add-node.sh $LOGIN_NAME $CLUSTERNAME $LOGIN_BOOT_MAC $LOGIN_PRIMARY_IP $LOGIN_PRIMARY_INTERFACE
        ```
3. Generate NFS boot environment and configuration for compute node (repeat this for all compute nodes being added)
    - This requires the following information:
        - `CLUSTERNAME`: The name of the cluster used in the previous step
        - `COMPUTE_NAME`: The short hostname to be used by this compute node
        - `COMPUTE_BOOT_MAC`: The MAC address of the interface that the compute node will boot off of
        - `COMPUTE_PRIMARY_IP`: The IP address for the compute node to use on the cluster subnet (something in the 10.10.0.0/19 ranges mentioned at beginning of this doc) of the GHPC network
        - `COMPUTE_PRIMARY_INTERFACE`: The interface name of the compute node which will be assigned the primary IP above
    - Generating compute node configuration like so
        ```bash
        CLUSTERNAME=cluster1
        COMPUTE_NAME=node01
        COMPUTE_BOOT_MAC=AA:BB:CC:DD:EE:FF
        COMPUTE_PRIMARY_IP=10.10.5.1
        COMPUTE_PRIMARY_INTERFACE=enp1s0

        cd /root/personality
        bash add-node.sh $COMPUTE_NAME $CLUSTERNAME $COMPUTE_BOOT_MAC $COMPUTE_PRIMARY_IP $COMPUTE_PRIMARY_INTERFACE
        ```
4. Launch controller (reference below for an NFS boot VM, could use hardware) NOTE: If booting once to find MAC address, subsequent boots will try to HD boot instead of network. To fix; `virsh destroy VM_NAME`, `virsh edit` (change `boot` from `hd` to `network`), `virsh start VM_NAME`
    ```bash
    CLUSTERNAME=cluster1
    SITENAME=mysite
    DOMAIN=$SITENAME.network
    VM_HOSTNAME=controller.$CLUSTERNAME.$DOMAIN
    NET_ETH0=virbr3 # Host Bridge interface for GHPC Admin Network
    NET_ETH1=virbr3 # Host Bridge interface for GHPC Cluster Network

    virt-install --name=${VM_HOSTNAME} --pxe --boot uefi=off --ram=4096 --vcpus=2 --import --network bridge=${NET_ETH0},model=virtio --network bridge=${NET_ETH1},model=virtio --graphics vnc,listen=0.0.0.0 --os-variant rhel9.4 --noautoconsole
    ```
5. Launch login node (with primary network interface (the one to PXE from) plugged into the GHPC cluster network and secondary network interface plugged into the External network)
6. Launch compute nodes
7. Configure SLURM to:
    1. Ensure that node names and resource information are correct
    1. Create whatever partitions you'd like
    1. Perform any other configuration changes to meet your needs
    1. **Read the Configuring SLURM doc for information on how to roll out the above changes**
