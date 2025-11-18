# Access Cluster

Once "Create a Cluster" has been completed there are 2 ways of accessing the cluster:
- For Admins: From the `director` - `ssh controller.cluster1` (replacing `cluster1` with the cluster name)
- For Users: From the External Network to the login node via the IP it gets over DHCP

## Web Access

An alternate route for users to access the cluster is via a web interface that is available on the login node. 

An admin will need to configure the web interface as follows:
1. SSH in as root to the login node of the cluster
1. Set the access hostname/IP for the External network interface of the login node
    - If DNS is setup in your external network to find the host by a hostname then use that, otherwise whatever IP address it has been assigned by DHPC should be used
    - Set the interface as follows 
        ```bash
        ACCESS_NAME=login1.myexternalnetwork.mycompany.example.com
        /opt/flight/bin/flight web-suite set-domain $ACCESS_NAME
        ```
    - **Note: The above set `ACCESS_NAME` will be the only valid name/ip with which the web interface can be used**
1. Restart web services
    ```bash
    /opt/flight/bin/flight web-suite restart
    ```

Now users can visit `ACCESS_NAME` in their web browser. 

**Note: New users will need to login at least once via CLI in order to set their password before they can use the web interface**
