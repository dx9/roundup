# Roundup

A really simple iPXE cluster scaffolding tool for [Rancher](https://github.com/rancher/rancher).

Wanna build a Rancher cluster from a stack of metal? This might help ya get started.

## Requirements

- Docker
- Docker Compose
- A host interface (different ways of doing this, but the pxe service needs host networking)

## Getting Started

Roundup defaults to a proxy-dhcp config, meaning it works alongside an existing DHCP server. It can also provide DHCP.

It defaults to using the eth0 interface.

1. clone it
2. run it __docker-compose up__ or __MODE=**dhcp** docker-compose up__
3. boot your metal

Watch your compose logs. You'll see a request like this go by:

GET /hosts/ABCDEF3-4c245544-1147-5g1g-8s4c-c6s0dffdgc31/boot.txt

This is iPXE looking for a boot file for the host with:

serial=ABCDEF3
uuid=4c245544-1147-5g1g-8s4c-c6s0dffdgc31

Make the directory boot/hosts/ABCDEF3-4c245544-1147-5g1g-8s4c-c6s0dffdgc31

Copy the default files from boot/default

- boot.txt - iPXE boot file for latest RancherOS
- cloud-config.yaml - cloud config

Customize the hosts config and reboot.

That's it. Pretty simple.

Here are some other cool approaches to pixie magic that may work better for you:

- https://github.com/jpetazzo/pxe (Roundup PXE service is based on this approach)
- https://github.com/danderson/pixiecore
