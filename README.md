Role Name
=========

configure pf rules
it should work on FreeBSD and OpenBSD if pf rules respect pf version of the OS

Requirements
------------

none 

Role Variables
--------------

```yaml
* pf_role_enable: False
if role will be applied 

* pf_ftpproxy_enable: False
if ftp proxy service need to be started on localhost

* pf_file_content: ''
all pf rules, see example playbook

* pf_tables: []
pf tables handled by the role, see example playbook

* pf_ip_forward_enable: False
if host will froward packets between interfaces or not

* pf_pflogd_enable: False
if pflogd daemon has to be started
```

Dependencies
------------

none

Example Playbook
----------------

```yaml
- hosts:  pf_compatible_hosts
  vars:
    pf_role_enable: yes
    pf_file_content: |
      # ansible managed, do nto eidt directly !
      set skip on lo
      table <whitelist_from_file> persist file "/etc/pf_tables/whitelist.table"
      table <blacklist_from_file> persist file "/etc/pf_tables/blacklist.table"
      table <whitelist> { 172.16.3.0/24 , 172.16.4.0/24 }
      block in quick from <blacklist_from_file>
      pass in quick from <whitelist_from_file>
      pass in quick from <whitelist>
    pf_tables:
      - name: blacklist
        dest: /etc/pf_tables/blacklist.table
        content: |
          192.168.0.0/16
      - name: whitelist
        dest: /etc/pf_tables/whitelist.table
        content: |
          172.16.1.0/24
          172.16.2.0/24
```

License
-------

BSD

TODO
----------------
restructure pf_tables to be avoid dest and use only name + a variable for tables folder
