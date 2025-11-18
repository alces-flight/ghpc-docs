# Setting Up

This guide describes how To setup your Director to ensure all services are using the correct domain information (including a fresh install of IPA).

1. Configure site information on `director`
    1. Edit `/root/personality/setup.sh` and set the name of your site and domain alongside your new root and IPA passwords
    2. Run the setup script (this will prompt for `root@domain` password for logging in which is the same as the default for domain `p4ssw0rd1;`)
        ```bash
        bash /root/personality/setup.sh
        ```
