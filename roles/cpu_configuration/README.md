The role lets a user manage the C-States of CPUs.
One could manage power mode with these variables:
- `cpu_configuration__disable_cstate`: set it to true to enable power-saving mode
- `cpu_configuration__cstate_num`: set the desired c-state number (and ensure that `cpu_configuration__disable_cstate` set to `false`)
