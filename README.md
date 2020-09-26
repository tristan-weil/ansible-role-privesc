# Ansible Role: OpenBSD_doas

An Ansible Role to configure the *sudo* and *doas* (OpenBSD) commands.

[![Actions Status](https://github.com/tristan-weil/ansible-role-privesc/workflows/molecule/badge.svg?branch=master)](https://github.com/tristan-weil/ansible-role-privesc/actions)

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |

Optional variables:

| Variable                | Default | Description |
| :---------------------- | :------ | :---------- |
| privesc_configs_list    | []      | a list of <*privesc rules group*> |
    
### <*privesc rules group*>    

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |
| name          | the name of the group |

Optional variables:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |
| state         | present | *present / absent*: to add a remove the group |
| rules         | []      | a list of <*rule*>

### <*rule*>

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |
| from          | the user allowed to do privileges escalation |

Optional variables:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |
| to            | ALL     | the user to impersonate |
| nopasswd      | False   | *True / False*: enable the no password option |
| commands      | ALL     | a list of allowed <*command*> |

### <*command*>

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |
| command       | the absolute path of a command |

Optional variables:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |
| args          | []     | a list of arguments |

## Example Playbook

    - hosts: 'webserver'
    
      tasks:
        - import_role:
            name: 'privesc'
          vars:
            privesc_configs_list:
              - name: 'molecule'
                state: 'present'
                rules:
                  - from: molecule
                    nopasswd: True
    
              - name: 'testinfra'
                state: 'present'
                rules:
                  - from: testinfra
                    nopasswd: True
                    commands:
                      - command: '/bin/ls'
                        arguments:
                          - '/root'
                      - command: '/usr/bin/whoami'

## Dependencies

See [requirements_galaxy.yml](https://github.com/tristan-weil/ansible-role-privesc/blob/master/requirements_galaxy.yml)

## Supported platforms

See [meta/main.yml](https://github.com/tristan-weil/ansible-role-privesc/blob/master/meta/main.yml)

## License

See [LICENSE.md](https://github.com/tristan-weil/ansible-role-privesc/blob/master/LICENSE.md)
