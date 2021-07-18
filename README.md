# Jason's Home Infrastructure

This repo conatins bootstrapping setup for my home infrastructure. 

For now this includes:
 * Vagrant files to spawn a pxe server
 * Files to auto-install ubuntu from pxe and initialize configuration
 * Installation of k3s via k3sup (in progress)

## Deploying PXE VM
The PXE VM runs in vagrant currently. Will probably move to something else eventually.

```
cd pxe/
vagrant up
```
Once the server is up, simply boot the node, it should pick up the ubuntu auto install by default.

### PXE Values to change
If needed, you can change the default boot label from the pxe server in the task `Create dnsmasq defaults` in the line `default focal-live-install-autoinstall`. This is helpful in cases were you want to regenerate the cloud-init user-data file for some reason. You can switch to the manual install `focal-live-install` as the default instead, do the install, and then after it's done, copy the auto-installer file back into this repo for deployment.

## Work in Progress