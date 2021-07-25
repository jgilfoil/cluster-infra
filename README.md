# Jason's Home Infrastructure

This repo conatins bootstrapping setup for my home infrastructure. 


## Deploying OS on Nodes
Currently a manual process.

Create boot usb stick with ubuntu 20.04 LTS [installed](https://ubuntu.com/tutorials/create-a-usb-stick-on-windows#1-overview) using [rufus](https://rufus.ie/en_US/) and boot off of it to install.
 1. Ensure host has a DHCP reservation for it's ip (192.168.1.200-204) from the router.
 2. Storage Configuration.
    Use entire Disk
      Sabrent (238.47G)
    uncheck `Set up this disk as an LVM Group`
 3. Profile Setup
    Your Name: Jason
    Your Servers's Name: odroid-xx
    Pick a username: ubuntu
    Password: see safe
 4. Check install OpenSSH server
    import keys from github username
 5. skip all featured server snaps

## k3sup Deployment

First prep the target server. First time you must specify the become password. The playbook will set NOPASSWORD in the suoders file for subsequent runs.
```
ansible-playbook odroid-bootstrap.yml -i hosts --limit odroid-## --extra-var="ansible_become_password=<bootstrap password>"
```

Install against first master server:
```
export IP=(ip of odroid target)
k3sup install \
  --ip $IP \
  --user ubuntu \
  --ssh-key ~/id_rsa.pub \
  --cluster
```
Install and join next master servers
```
export IP=(ip of odroid target)
export MASTER_IP=192.168.1.200
k3sup join \
  --ip $IP \
  --user ubuntu \
  --server-user ubuntu \
  --server-ip $MASTER_IP \
  --server
#  --k3s-version v1.19.1+k3s1 #if needed
```

## Troubleshooting k3s

### Embedded etcd3

See etcd tool and commands [here](https://gist.github.com/superseb/0c06164eef5a097c66e810fe91a9d408)