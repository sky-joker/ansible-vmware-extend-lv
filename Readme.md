# Ansible VMware extend LV

[![](https://img.shields.io/badge/license-MIT-brightgreen.svg)](https://github.com/sky-joker/ansible-vmware-windows-winrm-setup/blob/master/LICENSE.txt)

This Playbook will extend logical volume after adding disk to VM running on ESXi.

## Install

Clone this repository.

```
$ git clone https://github.com/sky-joker/ansible-vmware-extend-lv.git
```

## Usage example

Please rewrite `vars/vmware_parameters.yml` and `vars/vm_disk_parameters.yml` and `inventory` according to the environment.

`vmware_parameters.yml` is a VMware parameter vars file.

```yaml
---
# vmware parameters.
vcenter_hostname: vCenter IP
vcenter_username: administrator@vsphere.local
vcenter_password: secret
datacenter_name: change to datacenter name
disk_number: 0 # fixed parameter, do not change.
vm_name: change to target vm name
```

`vm_disk_parameters.yml` is a disk parameter vars file.

```yaml
# guest lvm parameters.
vg_name: change to volume group name
lv_name: change to logical volume name
lv_block_device_path: block device path of logical volume 
fstype: file system type(example xfs)
existing_pv_path:
  - change to existing pv path
  - add to list if there is more than one

# vm disk parameters.
vm_disk:
  - controller_key: change to scsi controller key number
    size_mb: specify disk size
    type: specify disk type, choice from thin, eagerzeroedthick, thick
    datastore: specify datastore name for saving vmdk
    scsi_controller: specify scsi controller bus number
    scsi_type: specify scsi type, choice from buslogic, lsilogic, lsilogicsas
    state: present
    device_path: block device path of the disk to be added(ex, /dev/sdb)
    disk_partition_number: partition number to be created
  - add to list if there is more than one
```

inventory file example.  
add host to extend logical volume.

```
[all]
lvm-test ansible_host=192.168.0.65 ansible_user=root ansible_password=secret
```

Playbook execution demo.

[![asciicast](https://asciinema.org/a/257236.svg)](https://asciinema.org/a/257236)

## License

This project is licensed under the MIT License - see the [LICENSE.txt](https://github.com/sky-joker/ansible-vmware-extend-lv/blob/master/LICENSE.txt) file for details.
