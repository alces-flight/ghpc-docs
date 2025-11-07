# Create a Cluster

Up to 8 clusters supported within site network. IP address ranges per cluster are in the 10.10.0.0/19 range, moving onto the next for each cluster, e.g.:
- First Cluster: 10.10.0.1 - 10.10.31.254
- Second Cluster: 10.10.32.1 - 10.10.63.254

The Controller node requires an IP on the Domain Network. This can be anything in the 10.178.0.0/16 range.

1. Generate cluster configuration (TO DO: Probs could auto-step through Domain Network & Cluster Network assignment to reduce chance for input issues?)
    ```bash
    CLUSTERNAME=c1
    cd /root/personality/
    bash new_cluster.sh $CLUSTERNAME 10.178.8.1 10.10.0.1
    ```
2. Prepare NFS boot environment for controller (note: if using a VM for controller, jump to step 4 for VM creation reference and see "Identifying Nodes" document for getting MAC address and interface name)
    ```bash
    CLUSTERNAME=c1
    bash generate_personality.sh $CLUSTERNAME cluster1 10.10.0.1 10.178.8.1
    bash set_boot_site.sh MAC_ADDRESS INTERFACE_NAME 10.178.8.1 controller
    ```
3. Generate login & compute cloud-init
    ```bash
    CLUSTERNAME=c1
    bash generate_personality.sh login1 $CLUSTERNAME 10.10.2.1
    bash set_boot.sh LOGIN_MAC_ADDRESS INTERFACE_NAME 10.10.2.1 $CLUSTERNAME login 10.10.0.1
    bash generate_personality.sh node01 $CLUSTERNAME 10.10.4.1
    bash set_boot.sh NODE01_MAC_ADDRESS INTERFACE_NAME 10.10.4.1 $CLUSTERNAME compute 10.10.0.1
    bash generate_personality.sh node02 $CLUSTERNAME 10.10.4.2
    bash set_boot.sh NODE02_MAC_ADDRESS INTERFACE_NAME 10.10.4.2 $CLUSTERNAME compute 10.10.0.1
    ```
4. Launch controller (reference below for an NFS boot VM, could use hardware) NOTE: If booting once to find MAC address, subsequent boots will try to HD boot instead of network. To fix; `virsh destroy VM_NAME`, `virsh edit` (change `boot` from `hd` to `network`), `virsh start VM_NAME`
    ```bash
    CLUSTERNAME=c1
    SITENAME=mysite
    DOMAIN=$SITENAME.network
    VM_HOSTNAME=controller.$CLUSTERNAME.$DOMAIN
    NET_ETH0=virbr3 # Host Bridge interface for Domain Network
    NET_ETH1=virbr3 # Host Bridge interface for Cluster Network

    virt-install --name=${VM_HOSTNAME} --pxe --boot uefi=off --ram=4096 --vcpus=2 --import --network bridge=${NET_ETH0},model=virtio --network bridge=${NET_ETH1},model=virtio --graphics vnc,listen=0.0.0.0 --os-variant rhel9.4 --noautoconsole
    ```
5. Launch login node (with primary network interface (the one to PXE from) plugged into cluster network and secondary network interface plugged into external network)
6. Launch compute nodes
