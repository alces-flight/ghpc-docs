# Enabling Offline Repositories

In some circumstances an HPC stack may be unable or throttled in access to the Internet or may want to be airgapped for whatever reason. To better serve the ability to install and manage packages across the stack it is possible to mirror upstream repositories on the director.

To mirror the Rocky 9, EPEL and OpenFlight repositories: 
```bash
cd /export/repo/
bash sync.sh rocky9
```

_Note: This will take some time and use at least 50GB of disk space_
