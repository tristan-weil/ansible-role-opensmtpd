# Ansible Role: opensmtpd

An Ansible Role that installs and configures `OpenSMTPD`.

Other mailers are removed.

[![Actions Status](https://github.com/tristan-weil/ansible-role-opensmtpd/workflows/molecule/badge.svg?branch=master)](https://github.com/tristan-weil/ansible-role-opensmtpd/actions)

## Role Variables

Available variables are listed below, (see also `defaults/main.yml`).

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |

Optional variables:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |
| opensmtpd_config | {{ _opensmtpd_config_default }} | the configuration of the service |
| opensmtpd_mailname | {{ ansible_facts['nodename'] }} | the mailname |
| opensmtpd_aliases | []   | the configuration of the <*aliases*> |
| opensmtpd_tables_list | []   | a dictionnary of <*table_list*> |
| opensmtpd_tables_mappings | []   | a dictionnary of <*table_mappings*> |

### <*aliases*>

This section allows to configure the *aliases*.

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |

Optional variables:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |
| path          | {{ _opensmtpd_aliases_path }} | the path of the aliases file |
| aliases       | ..      | the list of aliases

### <*table_list*>

A *table_list* represents an entry in a table.

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |
| path          | the path of the file |
| values        | a list of values |

Optional variables:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |
| makemap       | False   | *True / False*: enable the update of a .db file |

### <*table_mappings*>

A *table_list* represents an entry in a table.

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |
| path          | the path of the file |
| mappings      | a dictionnary of alias and its list of values |

Optional variables:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |
| makemap       | False   | *True / False*: enable the update of a .db file |

## Example Playbook

    - hosts: 'all'
      roles:
        - role: 'ansible-role-opensmtpd'
          opensmtpd_aliases: "{{ opensmtpd_aliases_default | combine({'aliases': {'root': ['testinfra']}}, recursive=True) }}"
        
## Todo

None.

## Dependencies

See [requirements_galaxy.yml](https://github.com/tristan-weil/ansible-role-opensmtpd/blob/master/requirements_galaxy.yml)

## Supported platforms

See [meta/main.yml](https://github.com/tristan-weil/ansible-role-opensmtpd/blob/master/meta/main.yml)

## License

See [LICENSE.md](https://github.com/tristan-weil/ansible-role-opensmtpd/blob/master/LICENSE.md)
