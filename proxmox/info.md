# Proxmox

-------

I currently run a 3 node proxmox cluster with the following nodes.

| Node | CPU | RAM | Disk |
| ---- | --- | --- | ---- |
| ms01ap | i9-13900H 20x core 4.8 GHz | 96GB (2x48GB) DDR5 5200MHz | 2TB nvme 
| ms01ap | i9-13900H 20x core 4.8 GHz | 96GB (2x48GB) DDR5 5200MHz | 2TB nvme
| junk01p | AMD Ryzen 3900X 24x core 3.8 GHz | 64GB (4x16GB) DDR4 2400MHz | 2x 1TB nvme

## Post Install

------

1. Run the following [proxmox helper script](https://tteck.github.io/Proxmox/#proxmox-ve-post-install).
    ```bash
    bash -c "$(wget -qLO - https://github.com/tteck/Proxmox/raw/main/misc/post-pve-install.sh)"
    ```
    1.1. use the following settings when prompted.
    ```
    start the proxmox ve post install script? (y/n) y
    correct proxmox ve sources? (y/n) y
    disable pve-enterprise repository? (y/n) y
    enable pve-no-subscription reprository? (y/n) y
    correct ceph package sources? (y/n) y
    add disabled pvetest repository? (y/n) n
    disable subscription nag? (y/n) y
    disable high availability? (y/n) n
    update proxmox ve now? (y/n) y
    ```
    1.2. reboot proxmox.


2. Add synologoy NFS share.

    2.1. In the web interface click on datacenter and select storage.
    
    2.2. In the top left click on Add and select NFS.
    
    2.3. Use the following settings.
    | key | value |
    | --- | ----- |
    | ID | synology-nas |
    | Server | 192.168.20.100 |
    | Export | /volume1/homelab |
    | Content | Disk image, ISO Image, Container template, VZDump backup file, Container, Snippets |


3. Create a linux bond on MS-01 nodes.
3.1. In the web interface click on the node and select network.
    
    3.2. Click on vmbr0 and select edit.
    
    3.3. Remove bridge ports.
    
    3.4. In the top left click on create and select Linux Bond.
    
    3.5. Give it the name bond 0 and auto start enabled.
    
    3.6. Set the slaves to enp87s0 enp88s0.
    
    3.7. Select mode to balance-rr.
    
    3.8. Click on vmbr0 and select edit.
    
    3.9. Set Bridge ports to bond0.
    
    3.10. Click on Apply Configuration.

4. Create SFP+ Link for clustering.
    4.1. In the web interface click on node and select network.

    4.2. In the top left click on create and select Linux Bridge

    4.3. Give it the name vmbr1 and auto start enabled and VLAN aware.

    4.4. Set the slaves to the two SFP+ ports.

    4.5. Click on apply network configuration.


## Create a cluster

1. In the web interface click on datacentre and select Cluster.
2. In the top left click on Create Cluster.
3. Give the cluster a name and click create.
4. Click on Join Information and select Copy Information.

## Join a cluster

1. In the web interface click on datacentre and select Cluster.
2. In the top left click on Join Cluster.
3. Paste the copied information.
4. Provide the remote notes root password.
5. Select the cluster Network link.
6. Click Join "cluster name".



