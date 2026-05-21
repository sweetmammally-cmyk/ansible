

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
      state: "present" # or "absent" for delete
      install_recommends: no # doing for minimal installation and etc
      update_cache: yes  # do apt update before installation
      ignore_errors: no # critical maybe , if not doing yes (bad practive)
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

# defining tasks in other folder for modularity and scalability
```yml
---
tasks:
    - name: Updating DNS Servers
      ansible.builtin.import_tasks: tasks/dns_update.yml
```

