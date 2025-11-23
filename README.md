# README



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
