# Customising Host

Boot-time information for a specific host can be changed in cloud-init data under `/var/www/cloudinit/HOST_IP/`

Live changes on a host are stored under `/var/run/upper/data/HOST` which can be useful to reference if interactively changing host while it's live. 

***Note: Any changes made live on a host will be lost if the host is stopped/restarted. For reference with making permanent changes see Customising Base Image doc***
