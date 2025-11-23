# README

## Create a VM from the command line


```bash
virt-install \
  --name ubuntu2504 \
  --ram 8192 \
  --vcpus 4 \
  --disk path=/var/lib/libvirt/images/ubuntu2504.img,format=qcow2,size=40 \
  --location ubuntu-25.04-live-server-amd64.iso,kernel=casper/vmlinuz,initrd=casper/initrd \
  --os-variant ubuntu25.04 \
  --network network=default \
  --graphics none \
  --console pty,target_type=serial \
  --extra-args="console=ttyS0,115200n8"
```


## List all VMs

```bash
virsh list --all
```

## Details about VM

```bash
virsh dominfo <vmname>
```

## List all checkpoints for a VM

```bash
virsh snapshot-list <vmname>
```

## Actual checkpoints (used with backup features):

```bash
virsh checkpoint-list <vmname>
```

## Revert/restore to a checkpoint

For standard snapshots:

```bash
virsh snapshot-revert <vmname> <snapshot_name>
```

For checkpoint-based restore (rare unless you set them up):

```bash
virsh restore <vmname> <checkpoint-file>
```

## Start the VM in headless mode

virsh defaults to headless unless you explicitly attach a viewer.

```bash
virsh start <vmname> --console
```

If you don’t want console output:

```bash
virsh start <vmname>
```

If you want to attach to the serial console:

```bash
virsh console <vmname>
```

Exit console with:

```bash
Ctrl + ]
```

## List all public keys

```bash
ls ~/.ssh/id_*.pub
```
## Create key if not exists

```bash
ssh-keygen -t rsa -b 4096
```

## Copy RSA Keys to VM

```bash
sudo apt install openssh-client
```

Use it to send your key to a VM

```bash
ssh-copy-id user@<vm-ip>
```

It’ll prompt for the password once, then append your public key to the VM’s ~/.ssh/authorized_keys and set the correct permissions.

After that:

```bash
ssh user@<vm-ip>
```

## Shutdown a VM

```bash
virsh shutdown <vm-name>
```

## Undefine the VM including ALL extras

This is the important part. Include flags depending on what the VM has.

Full nuclear undefine:

```bash
virsh undefine <vmname> --nvram --remove-all-storage --snapshots-metadata
```

Explanation of flags

--remove-all-storage → deletes all disks tied to the VM that are in storage pools managed by libvirt

--nvram → removes UEFI variables (OVMF, etc.)

--snapshots-metadata → deletes libvirt snapshot metadata

--managed-save → if the VM has a “managed save” state, include this

--keep-snapshots vs --snapshots-metadata → use the snapshots one to delete them

If you want everything absolutely gone, use:

```bash
virsh undefine <vmname> \
  --remove-all-storage \
  --snapshots-metadata \
  --nvram \
  --managed-save
```


