# Identifying Nodes

On the director

- Prepare to "hunt" new nodes
```bash
$ flight hunter hunt
Hunter running on port 8888 - Ctrl+C to stop
```
- Launch node
- See the hunted node come through 
```bash
$ flight hunter hunt
Hunter running on port 8888 - Ctrl+C to stop


Found node.
ID: b20a5b1f
Name: hunterclient
IP: 10.178.31.91

Node added to buffer node list
```
- View node information (replace `b20a5b1f` with ID you see in previous output)
    ```bash
    flight hunter show --buffer b20a5b1f
    ```
    - The `label` preset (at the top of this output) is the MAC address of the interface it PXE booted on. This will be useful later on when assigning a personality to the node for PXE booting
