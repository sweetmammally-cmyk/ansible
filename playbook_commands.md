

# How to run a playbook
```bash
ansible-playbook {playbook path} --limit {server name} -i  {path of the yaml inventory file or all}
```

# Installing a package in playbook for apt task
```yml
---
- name : Install nginx
  hosts: all
  tasks:
    ansible.builtin.apt:  #we do the ansible.builtin.yum for redhat , easy
      name: nginx
      state = "present" # or "absent" for delete
      update_cache: yes  # do apt update before installation

```

# restarting a service , enable , stop and etc
```yml
---
- name : restarting nginx
  hosts: all
  ansible.builtin.service:
    name: nginx
    state : restarted #for stoping :stoped , for starting : started
    enabled: yes      #for disabling : no ,  
```
you can do everything with combination above 