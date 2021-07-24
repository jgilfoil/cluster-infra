# Jason's Home Infrastructure

This repo conatins bootstrapping setup for my home infrastructure. 


## Deploying OS on Nodes
Create boot usb stick with ubuntu 20.04 LTS [installed](https://ubuntu.com/tutorials/create-a-usb-stick-on-windows#1-overview) using [rufus](https://rufus.ie/en_US/).

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