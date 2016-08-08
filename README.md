# Joyent-Triton-SDC-CoaL-Install
A quide to installing Joyent's Triton/SDC "Cloud on a Laptop" (CoaL) on Macs which should also apply to iMacs, Mac Minis and Mac Pros

# Setting up Joyent Triton/SDC CoaL 

### The Real Guide
https://github.com/joyent/triton/blob/master/docs/developer-guide/coal-setup.md

Slow, opinionated: perfect for your first run through!

What follows is a journal of what I've found works for me personally based on a few bad experiences, slow internet connections, some vacation time, and 3 Macs. 

### Requirements

This guide is based on having: 

 * An up-to-date Mac running El Capitan (10.11) though it was also tested on the developer preview of macOS Sierra (10.12)
 * VMware Fustion >8.1.1
 * 40GB fast-ish hard disk space free
 * About 8GB RAM free
 * A somewhat capable internet connection 

### Installing Pre-Requisites

We'll need to have VMware Fusion installed, along with a few other tools. For this, I'll use the awesome Homebrew tool. 

#### Install Homebrew

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" < /dev/null 2> /dev/null
```

#### VMware Fusion Setup

##### Installation
`brew install vmware-fusion`

##### First Run
`vmware-fusion &` 

##### Setup Networks

Note that before proceeding, you'll want to turn off/suspend any VMs you have running, or you'll be met with the following: 

```
You have Virtual Machines running. Please shut them down before continuing.
```

If you're OK simply turning it all of and suspending, you can do that like so:

```
if [ -d "/Applications/VMware Fusion.app/Contents/Library" ]; then export PATH=$PATH:"/Applications/VMware Fusion.app/Contents/Library"; fi
vms=(vmrun list | sed -n '1!p')
for (( vm=0; vm < ${#vms[@]}; vm++ )); do vmrun stop "${vms[$vm]// /\\ }"
```

The guide sets up two networks: 
* External network on 10.0.88.0/24
* Admin network on 10.0.99.0/24

These can be edited in the bash script if neeed. For now, we're going to YOLO-style curl-bash:

```
curl -s https://raw.githubusercontent.com/joyent/sdc/master/tools/coal-mac-vmware-setup | sudo bash
```

#### Aria2 Setup 

Appologies to the Joyent folks, but the server sharing CoaL is quite slow and has yet to resume a failed download, so I'll be recommending the use of the Aria2 downloader here which will support 5 retries by default, and we'll use it to connect to the server more than once in order to increase speed. 


`brew install aria2`

#### Cloud on a Laptop Download

We'll create a directory for CoaL, move to it, download CoaL with 5 streams (the max from the Joyent servers as of this writing), then extract the archive and remove the source file. 

On my end, the max speed I could get was about 3MiB/s, so expect this to take about 10 minutes or so as the file is 1.9GiB compressed. 

##### Download and Unpack Script
```
cd
mkdir CoaL
cd CoaL
aria2c -x 5 https://us-east.manta.joyent.com/Joyent_Dev/public/SmartDataCenter/coal-latest.tgz
tar -xzvf coal-latest.tgz
rm coal-latest.tgz
``` 

##### Check Folder Structure

Now that the script is done, if we run `find .`, the folder structure should like something like this:

```
.
./boot_archive.manifest
./coal-release-20160804-20160804T215306Z-g3ce6e16-4gb.vmwarevm
./coal-release-20160804-20160804T215306Z-g3ce6e16-4gb.vmwarevm/4gb.img
./coal-release-20160804-20160804T215306Z-g3ce6e16-4gb.vmwarevm/USB-headnode.vmdk
./coal-release-20160804-20160804T215306Z-g3ce6e16-4gb.vmwarevm/USB-headnode.vmsd
./coal-release-20160804-20160804T215306Z-g3ce6e16-4gb.vmwarevm/USB-headnode.vmx
./coal-release-20160804-20160804T215306Z-g3ce6e16-4gb.vmwarevm/USB-headnode.vmxf
./coal-release-20160804-20160804T215306Z-g3ce6e16-4gb.vmwarevm/zpool.vmdk
./root.password.20160804t173034z
./usb_key.manifest
```
