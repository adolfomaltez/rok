# Install Elemental on Rancher mgmt cluster


## Install elemental operator on k8s cluster
```sh
helm upgrade --create-namespace -n cattle-elemental-system \
  --install elemental-operator \
  oci://registry.opensuse.org/isv/rancher/elemental/stable/charts/rancher/elemental-operator-chart
```

```sh
kubectl get pods -n cattle-elemental-system
```


TPM emulation:
```sh
mkdir /tmp/mytpm1
swtpm socket --tpmstate dir=/tmp/mytpm1 \
  --ctrl type=unixio,path=/tmp/mytpm1/swtpm-sock \
  --tpm2 \
  --log level=20
```
Qemu command:

```sh
qemu-img create vm01-disk.img 20G
qemu-system-x86_64 -m 2048 -bios /usr/share/ovmf/OVMF.fd \
  -cdrom /tmp/elemental-teal.x86_64.iso -boot menu=on \
  -chardev socket,id=chrtpm,path=/tmp/mytpm1/swtpm-sock \
  -tpmdev emulator,id=tpm0,chardev=chrtpm  \
  -device tpm-tis,tpmdev=tpm0 vm01-disk.img 
```

## Referencias
- https://elemental.docs.rancher.com/quickstart-ui
- https://elemental.docs.rancher.com/quickstart-cli
- https://qemu-project.gitlab.io/qemu/specs/tpm.html