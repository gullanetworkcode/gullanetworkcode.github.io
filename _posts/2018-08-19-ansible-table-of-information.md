---
published: true
---
## How to use Ansible to create a table of device information


This blog will explain how to use an Ansible playbook to collect inventory information from some devices and publish in a table.

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




