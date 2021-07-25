# Jason's Home Infrastructure

This repo conatins bootstrapping setup for my home infrastructure. 


## Deploying OS on Nodes
Currently a manual process.

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

First prep the target server. First time you must specify the become password. The playbook will set NOPASSWORD in the suoders file for subsequent runs.
```
ansible-playbook odroid-bootstrap.yml -i hosts --limit odroid-## --extra-var="ansible_become_password=<bootstrap password>"
```

Install against first master server:
```
k3sup install --ip $IP --user ubuntu --ssh-key ~/id_rsa.pub --cluster
```