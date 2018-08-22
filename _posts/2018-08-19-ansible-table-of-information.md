---
published: true
---
## How to use Ansible to create a table of device information


This blog will explain how to use an Ansible playbook to collect inventory information from some devices and publish in a table.

### Quick Summary

The playbook looks like this:-


```
---

- name: Gather facts and create table
  hosts: your_inventory_group
  gather_facts: no

  tasks:
    - name: Get facts info from device
      ios_facts:
        gather_subset:
          - hardware

    - name: Create MD file
      template: src=ios_facts_table.j2 dest=files/ios_facts_table.md
```

and then a Jinja template is required like this:-

{% raw %}
```
|Device |Model |IOS Version |Serial Number |
|----------|-----------|-----------|----------|
{% for device in groups['your_inventory_group'] %}
|{{hostvars[device]['ansible_net_hostname']}}|{{hostvars[device]['ansible_net_model']}}|{{hostvars[device]['ansible_net_version']}}|{{hostvars[device]['ansible_net_serialnum']}}|
{% endfor %}
```
{% endraw %}

**Note: You will need to change 'your_inventory_group' to a group in your inventory.**

When the playbook is run the output will look like this (obviously with your own device names etc - the ones here are from my GNS3 test bed):-

```

ansible-playbook -i your_inventory_group ios_facts_table.yml --vault-id @prompt     
Vault password (default): 

PLAY [Gather facts and create table] *******************************************

TASK [Get facts info from device] **********************************************
ok: [10.1.1.23]
ok: [10.1.1.20]

TASK [Create MD file] **********************************************************
changed: [10.1.1.20]
changed: [10.1.1.23]

PLAY RECAP *********************************************************************
10.1.1.20                  : ok=2    changed=1    unreachable=0    failed=0   
10.1.1.23                  : ok=2    changed=1    unreachable=0    failed=0   

```
Then in the /files directory there will be a new markdown file that will look something like this:- 

|Device |Model |IOS Version |Serial Number |
|----------|-----------|-----------|----------|
|test_iosv|IOSv|15.6(2)T|9FC4YBHUJMHEPZWZYTVPD|
|test_iosv_23|IOSv|15.6(2)T|996M55W0XPK9F7O8S5IBP|

### More detail

The playbook uses the _iosfacts_ ansible core module but is only gathering the **hardware** subset. As it isn't mentioned in the documentation I'll list what is included in the hardware subset:-

```

"ansible_net_hostname": " ",
"ansible_net_image": " ",
"ansible_net_memfree_mb":  ,
"ansible_net_memtotal_mb":  ,
"ansible_net_model": " ",
"ansible_net_serialnum": " ",
"ansible_net_version": " "

```

### Links

[https://docs.ansible.com/ansible/2.5/modules/ios_facts_module.html]( "ios_facts ansible documentation")