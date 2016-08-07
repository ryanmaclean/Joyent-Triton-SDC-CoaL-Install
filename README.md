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

#### VMware Fusion Setup

##### Installation
`brew install vmware-fusion`

##### First Run
`vmware-fusion &` 

#### Aria2 Setup 

Appologies to the Joyent folks, but the server sharing CoaL is quite slow and has yet to resume a failed download, so I'll be recommending the use of the Aria2 downloader here which will support 5 retries by default, and we'll use it to connect to the server more than once in order to increase speed. 


`brew install aria2`

#### Cloud on a Laptop Download
`aria2 -x 5` 
