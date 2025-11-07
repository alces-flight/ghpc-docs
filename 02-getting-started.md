# Getting Started

**Note: If you're reading this then congratulations, you've launched your GHPC Director!**

Ensure your platform is ready for appliance deployment, to be prepared you will need a libvirt virtualisation host.

1. Download the Director & Domain appliances from LINK_URL to `/data/ghpc-source/` on your VM host
2. Launch Director VM
    ```bash
    SITENAME=mysite
    DOMAIN=$SITENAME.network
    VM_HOSTNAME=director.$DOMAIN
    VM_IMAGE=/data/$SITENAME/director-root.qcow2
    DATA_IMAGE=/data/$SITENAME/director-data.qcow2
    NET_ETH0=virbr3 # Host Bridge interface for Domain Network
    NET_ETH1=virbr2 # Host Bridge interface for External Network

    mkdir -p $(dirname $VM_IMAGE)
    
    cp /data/ghpc-source/director-root.qcow2 $VM_IMAGE
    cp /data/ghpc-source/director-data.qcow2 $DATA_IMAGE
    
    VM_IMAGE_FORMAT=$(qemu-img info ${VM_IMAGE} | grep "^file format" | cut -d':' -f2 | xargs)
    DATA_IMAGE_FORMAT=$(qemu-img info ${DATA_IMAGE} | grep "^file format" | cut -d':' -f2 | xargs)
    
    virt-install --name=${VM_HOSTNAME} --boot uefi --ram=2048 --vcpus=1 --import --disk path=${VM_IMAGE},format=${VM_IMAGE_FORMAT} --disk path=${DATA_IMAGE},format=${DATA_IMAGE_FORMAT} --os-variant=rhel9.4 --network bridge=${NET_ETH0},model=virtio --network bridge=${NET_ETH1},model=virtio --graphics vnc,listen=0.0.0.0 --noautoconsole
    ```
3. Launch Domain (requires at least 2 cores and 4GB RAM)
    ```bash
    SITENAME=mysite
    DOMAIN=$SITENAME.network
    VM_HOSTNAME=domain.$DOMAIN
    VM_IMAGE=/data/$SITENAME/domain-root.qcow2
    NET_ETH0=virbr3 # Host Bridge interface for Domain Network
    
    cp /data/ghpc-source/domain-root.qcow2 $VM_IMAGE
    
    VM_IMAGE_FORMAT=$(qemu-img info ${VM_IMAGE} | grep "^file format" | cut -d':' -f2 | xargs)
    
    virt-install --name=${VM_HOSTNAME} --boot uefi --ram=4096 --vcpus=2 --import --disk path=${VM_IMAGE},format=${VM_IMAGE_FORMAT} --os-variant=rhel9.4 --network bridge=${NET_ETH0},model=virtio --graphics vnc,listen=0.0.0.0 --noautoconsole
    ```
4. Log into director as root (default password: `p4ssw0rd1;`)
    1. Can locate DHCP IP on `ext` network of VM with `virsh net-dhcp-leases ext` (might need to cross reference with MAC address of VM interface shown in `virsh domiflist $VM_HOSTNAME`)
5. Read the embedded docs
    ```bash
    ghpc-docs
    ```
