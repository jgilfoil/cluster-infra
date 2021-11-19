# Jason's Home Infrastructure

This repo conatins bootstrapping setup for my home infrastructure. 

## How to Use
The VagrantFile contains configuration for a "control" vm that is meant to be used as the workstation execution environment for deploying, configuring and administering the cluster with tools like k3sup, kubectl, etc. I do this because I work on Windows with some constraints around my workstation environment. Eventually i'll move this into a container and WSL2 or something.

## Deploying OS on Nodes
Currently a manual process.

Create boot usb stick with ubuntu 20.04 LTS [installed](https://ubuntu.com/tutorials/create-a-usb-stick-on-windows#1-overview) using [rufus](https://rufus.ie/en_US/) and boot off of it to install.
 1. Ensure host has a DHCP reservation for it's ip (192.168.1.200-204) from the router.
 2. Storage Configuration.
    - Use entire Disk
      - Sabrent (238.47G)
    - uncheck `Set up this disk as an LVM Group`
 3. Profile Setup
    - Your Name: Jason
    - Your Servers's Name: odroid-xx
    - Pick a username: ubuntu
    - Password: see safe
 4. Check install OpenSSH server
    - import keys from github username
 5. skip all featured server snaps

## Spin up VM Control client
This VM is contains all the tools and configuration required to deploy and manage the cluster infrastructure. The [provision_control_vm.yml](./provision_control_vm.yml) playbook will take care of most of the setup.
```
vagrant up
vagrant ssh
```
From here on out, all steps will be taken inside this VM.

If you rebuild your control VM after the cluster has been deployed, or need to pull down the kubeconfig info for any reason, run this:
```
export IP=192.168.1.200
k3sup install \
  --skip-install \
  --ip $IP \
  --user ubuntu
```

## Node Configuration

First prep the target server. This playbook will:
 * prep the target server with sudoers nopasswd access for the user account.
 * set the second nic to be optional to reduce boot time while it dhcp times out on it
 * set local timezone on target
 * set max number inotify user watches
 * setup un-attended upgrades
 * install nfs-common package to mount nfs shares from NAS

First time you must specify the sudo password(become). The playbook will set NOPASSWORD in the suoders file so that it won't be required for any subsequent runs against a given target.
```
cd /code/cluster-infra
ansible-playbook odroid-bootstrap.yml -i hosts \
  --limit odroid-01 \
  --extra-var="ansible_become_password=<bootstrap password>"
```

From here proceed to the [k8s-gitops repo](k8s-gitops repo) to deploy k3s, flux and the rest of the services and apps.

## Commands

Shutdown cluster - This runs the tasks asynchronously so there's no failure of the tasks.
```
ansible all -i hosts -B 3600 -P 0 -a "shutdown now" --become
```