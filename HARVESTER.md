# Install Harvester HCI

# Integrate Rancher and Harvester

## Rancher
Virtualization Management, Import existing.

Cluster name: harvester-t0
Cluster description: Harvester cluster for VMs Tier 0
Create

Anotate Registration URL


## Harvester HCI

Add private Rancher CA
Main menu -> Advanced -> Settings -> Addiotional CA

Rancher 
Main menu -> global settings -> settings -> show cacerts


Set cluster-registration-url on Harvester from Rancher.
Main menu -> Addvanced -> Settings -> cluster registration URL

# Create host network


# Load VM image on Harvester

Download cloud images from the OS (opensuse Leap 15 in this case)
https://download.opensuse.org/repositories/Cloud:/Images:/Leap_15.5/images/

https://download.opensuse.org/repositories/Cloud:/Images:/Leap_15.5/images/openSUSE-Leap-15.5.x86_64-1.0.0-NoCloud-Build1.57.qcow2

Images -> Create
Namespace: default
Name: openSUSE-Leap-15.5.x86_64



# Reference
- https://docs.harvesterhci.io/dev/upload-image
