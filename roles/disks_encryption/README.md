The role encrypts a given disk with LUKS, creates a filesystem on it and mounts the partition.
To manage the resulting partition, utilize these vars:
- `disks_encryption__source`: a partition to encrypt
- `disks_encryption__luks_device_name`: a LUKS device name
- `disks_encryption__mount_point`: a path where we will mount the encrypted partition

A LUKS password is located under `secrets/{{ invenotory_hostname }}/<partition_name>`. Do not forget to encrypt it before the commit.
