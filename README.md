# Ansible Role: opensmtpd

An Ansible Role that installs and configures `OpenSMTPD` on Debian and OpenBSD.

Other mailers are removed.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    opensmtpd_macros: []                                    # the list of macros
      - name:                                               # the name of the macro
        value:                                              # the value of the macro
   
The list of macros that will be in the `MACROS` part of the configuration file. 
This is where the macros are defined.
Each item must include the name of the `macro` and its `value`.
    
    opensmtpd_listen_interfaces: []                         # the list of listening interfaces
      - listen_on: localhost                                # the bind or socket address
    
The list of listening interfaces that will be in the `LISTEN` part of the configuration file.
This is where interfaces or sockets are defined.
    
    opensmtpd_tables:                                       # the list of tables
      - name: aliases                                       # the name of the table
        type: db                                            # the type (db / file)
        config: "{{ opensmtpd_conf_dir }}/aliases.db"       # the file path
        makemap_type: aliases                               # the makemap type (aliases / set)
        lines: []                                           # the content of the file line by line
          - line:                                           # a line (the content is dependent of the file type)
            state: present                                  # present|absent: if present, add the line

The list of rules that will be in the `TABLES` part of the configuration file.
    
    opensmtpd_rules:                                        # the list of rules 
      - accept for local alias <aliases> deliver to mbox    # deliver to local alias
      - accept for any relay                                # accept to relay to all

The list of rules that will be in the `RULES` part of the configuration file.

    opensmtpd_mailname: "{{ etc_hosts_list | map(attribute='hostname') | unique | first | default(inventory_hostname) }}" # the mailname

OpenSMTPD will present itself with this name.
    
    opensmtpd_firewall_ipv4_rules: []                       # ipv4 firewall rules
    opensmtpd_firewall_ipv6_rules: []                       # ipv6 firewall rules

If there is a need for OpenSMTPD to listen to incoming SMTP requests, the firewall rules need to be configured.

For more information, see https://www.opensmtpd.org/manual.html

## Dependencies

- t18s.fr_pkg
- t18s.fr_firewall_config

## Example Playbook

    - hosts: all
      roles:
        - role: t18s.fr_opensmtpd
          opensmtpd_tables:
            - name: aliases
              type: db
              config: "{{ opensmtpd_conf_dir }}/aliases.db"
              makemap_type: aliases
              lines:
                - line: "root: titou@lab.t18s.fr"
                  regexp: "^root:"
                  state: present
        
## Todo

Make it available for OpenBSD.

## License

```
Copyright (c) 2018 Tristan Weil <titou@lab.t18s.fr>

Permission to use, copy, modify, and distribute this software for any
purpose with or without fee is hereby granted, provided that the above
copyright notice and this permission notice appear in all copies.

THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES
WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF
MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR
ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES
WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN
ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF
OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.
```
