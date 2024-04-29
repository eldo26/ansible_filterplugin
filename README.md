# ansible filter plugins

## Directory Structure
```
├── filter_plugins
│   └── myfilter.py
├── inventory
└── myplaybook.yaml
```
## Python code 
```
eldo@eldo-asus:~/lab/ansible$ cat filter_plugins/myfilter.py
class FilterModule(object):
    def filters(self):
        return {
            'my_filter': self.my_filter
        }
    def my_filter(self, get_hostname, get_release):
        print('Machine name is: {}'.format(get_hostname['stdout']))
        print('Machine release is: {}'.format(get_release['stdout']))
```
## Ansibe PlayBook
```
eldo@eldo-asus:~/lab/ansible$ cat myplaybook.yaml
---
- name: check data
  hosts: all
  tasks:
    - ansible.builtin.shell:
        cmd: uname -n
      register: machine_name
    - ansible.builtin.shell:
        cmd: uname -r
      register: machine_release
    - debug:
        msg : "{{ machine_name | my_filter(machine_release) }}"
```

