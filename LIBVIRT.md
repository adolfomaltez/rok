# Installation of LibVirt

- Install debian 12
- Check KVM
- Install Libvirt
- Install vagrant
- Copy/Create Vagrantfile


## Install debian 12
    echo 'PATH=$PATH:/sbin' >> /root.bashrc
    source /root/.bashrc

## Install Libvirt

    apt-get install -y qemu-kvm libvirt-clients libvirt-daemon-system bridge-utils virtinst libvirt-daemon virt-manager dnsmasq qemu-utils swtpm swtpm-tools
    

### Create vmdisks pool

    virsh pool-define-as --name vmdisks --type dir --target /var/lib/libvirt/images
    virsh pool-list --all
    virsh pool-start --build vmdisks
    virsh pool-autostart vmdisks
    virsh pool-info vmdisks


## Install Vagrant
    apt-get install vagrant-libvirt libvirt-daemon-system
    usermod --append --groups libvirt taro

## Vagrantfile
copy vagrant file
    vagrant up


## References
- https://wiki.debian.org/KVM
- https://wiki.debian.org/Vagrant
- https://vagrant-libvirt.github.io/vagrant-libvirt/examples.html