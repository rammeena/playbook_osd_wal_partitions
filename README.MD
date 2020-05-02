# parted – Configure block device partitions

This playbook allows configuring block device partition using the `parted` command line tool.

# Requirements
```This module requires parted version 1.8.3 and above.```

```If the version of parted is below 3.1, it requires a Linux version running the sysfs file system /sys/```

# Examples
```
parted:
  device: /dev/sdb
  number: 1
  state: present

- name: Remove partition number 1
parted:
  device: /dev/sdb
  number: 1
  state: absent

- name: Create a new primary partition with a size of 1GiB
parted:
  device: /dev/sdb
  number: 1
  state: present
  part_end: 1GiB

- name: Create a new primary partition for LVM
parted:
  device: /dev/sdb
  number: 2
  flags: [ lvm ]
  state: present
  part_start: 1GiB

# Example on how to read info and reuse it in subsequent task
- name: Read device information (always use unit when probing)
parted: device=/dev/sdb unit=MiB
register: sdb_info

- name: Remove all partitions from disk
parted:
  device: /dev/sdb
  number: '{{ item.num }}'
  state: absent
loop: '{{ sdb_info.partitions }}'
```
