# Making Changes to Client Operating Systems

There are multiple ways of making changes to hosts within a cluster. The recommended method varies depending on the change being made and the persistence of it.

## Customising Host Directly

> Recommended For: Testing changes and temporary modifications

**Note: Any changes made live on a host will be lost if the host is stopped/restarted. For reference with making permanent changes see Customising Base Image doc**

Boot-time information for a specific host can be changed in cloud-init data under `/var/www/cloudinit/HOST_IP/`.

Live changes on a host are stored under `/run/upper/data/HOST` which can be useful to reference if interactively changing host while it's live.

## Customising Base Image

> Recommended For: Package installations

**Note: Making changes to a base image through this method while there are live nodes running from them may lead to issues. It is therefore recommended only to enter the image environment before live nodes are launched or during maintenance periods to minimise any risk to workloads.**

The base image for each image type is under `/export/image/`. Changes are made by entering the image environment

To enter an interactive environment within the image to make changes:
```bash
cd /export/image/
bash chroot.sh IMAGE_TYPE
```

When finished, run `exit` or press `ctrl`+`D` to ensure the environment is safely exited.

### Live Environment Quirks

While it is **not recommended** to make changes to the base image like this, if there are live environments they may hit `Stale file handle` issues when accessing a file that has been changed in the base image since launch. This can be alleviated by remounting the file system:
```bash
mount -o remount /
```
