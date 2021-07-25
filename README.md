# Jason's Home Infrastructure

This repo conatins bootstrapping setup for my home infrastructure. 


## Deploying OS on Nodes
Create boot usb stick with ubuntu 20.04 LTS [installed](https://ubuntu.com/tutorials/create-a-usb-stick-on-windows#1-overview) using [rufus](https://rufus.ie/en_US/) and boot off of it to install.

 1. Storage Configuration.
    Use entire Disk
      Sabrent (238.47G)
    uncheck `Set up this disk as an LVM Group`
 2. Profile Setup
    Your Name: Jason
    Your Servers's Name: odroid-xx
    Pick a username: ubuntu
    Password: see safe
 3. Check install OpenSSH server
    import keys from github username
 4. skip all featured server snaps

## k3sup Deployment

Download and install k3sup
```
curl -sLS https://get.k3sup.dev | sh
sudo install k3sup /usr/local/bin/
```
Set sudo nopasswd on target server (to be added to ansible prep playbook)

install k3sup
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

Install against first master server:
```
k3sup install --ip $IP --user ubuntu --ssh-key ~/id_rsa.pub --cluster
```