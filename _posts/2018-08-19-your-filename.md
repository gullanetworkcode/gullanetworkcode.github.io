---
published: false
---
## How to use Ansible to create a table of device information

![_config.yml]({{ site.baseurl }}/images/config.png)

This blog will explain how to use an Ansible playbook to collect inventory information from some devices and publish in a table.

The playbook looks like this:-


```
- name: Test getting facts
  hosts: poc_ios
  gather_facts: no

  tasks:
    - name: Get facts info from device
      ios_facts:
        gather_subset:
          - hardware

    - name: Create MD file
      template: src=version_and_model.j2 dest=files/version_and_model.md
```
New words
