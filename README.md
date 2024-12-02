
### Setup

Alongside with ansible present on your machine, you need to install the below requirements.

Galaxy collections
```
$ ansible-galaxy install -r galaxy-requirements.yml
```
And the pre-commit hook (for details consult the "Secrets" section): create a file named `.git/hooks/pre-commit` with the below contents and make it executable
```
#!/bin/bash

set -o nounset

SECRETS_DIR=./secrets
ENCRYPTED_PATTERN="\$ANSIBLE_VAULT"

for f in $(find $SECRETS_DIR -type f); do
    grep --quiet "^$ENCRYPTED_PATTERN" $f || (echo "I found unencrypted file $f" && exit 1)
done
```

### Playbooks

#### CPU C-state management

The playbook `cpu_power_mode.yml` lets a user manage the C-States of CPUs.
One could manage power mode with these variables:
- `cpu_configuration__disable_cstate`: set it to true to enable power-saving mode
- `cpu_configuration__cstate_num`: set the desired c-state number (and ensure that `cpu_configuration__disable_cstate` set to `false`)

This example run disables C-state for all available CPUs

```
$ ansible-playbook cpu_power_mode.yml -D -e cpu_configuration__disable_cstate=true
```

#### Disks and network management

The playbook `servers_setup.yml` performs the below actions:
- encrypts the second disk in a system (a disk path specified in `group_vars`)
- encrypts the partition that is present on the disk next to the root partition
- ensures that the active network interface is called "net0"
- at the end, prints information about CPUs and multithreading

### Secrets

Put the vault password into the `.vault_pass` file.
Store the secrets under `secrets/` directory and `{{ inventory_hostname }}` subdirectories.
If you forget to encrypt the secret, there's a pre-commit hook that won't let you commit it.
