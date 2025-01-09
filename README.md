# :house: My Homelab

----------

This repo contains all of my configs and relevant documentation for my homelab. My homelab is mainly for testing and learning while also self hosting some apps. I am aiming to have a staging and production style environment.

### what I'm planning on learning
* Linux administration
* Networking
* CD/CI pipelines
* Ansible
* Python, go, java
* postgresql, mysql
* docker, kubernetes

# :computer: My Hardware

----------

#### Network stack

My network makes uses of the TP-Link Omada hardware.

* TL-ER7206, router
* TL-SG3428, primary switch
* TL-SG2210P, POE switch
* OC-200, omada controller

#### Virtualisation 

I run proxmox as my hypervisor platfrom and run two stand alone nodes.

Nodes: Minisforum MS-01
CPU: i9-13900H
Cores: 20
RAM: 96GB DDR5 5600 MHz
Disk: 2TB NVME SSD

#### Pi Cluster

I run an Elasticsearch cluster using five pi4 8GB models with a 2 TB ssd attachted. The Elasticsarch cluster is used for my main monitoring solution



 

