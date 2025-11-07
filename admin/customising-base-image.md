# Customising Base Image

The base image for each image type is under `/export/image/`. Changes are made by entering the image environment

To enter an interactive environment within the image to make changes:
```
cd /export/image/
bash chroot.sh IMAGE_TYPE
```

When finished, run `exit` or press `ctrl`+`D` to ensure the environment is safely exited.

***Note: Making changes to a base image through this method while there are live nodes running from them may lead to issues. It is therefore recommended only to enter the image environment before live nodes are launched or during maintenance periods to minimise any risk to workloads.***
