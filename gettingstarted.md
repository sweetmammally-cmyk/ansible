# How to ping servers ?
```bash
ansible -i {path of the yaml file that we defined the servers} {server name} -m ping
```

# How to run a playbook
```bash
ansible-playbook {playbook path} --limit {server name} -i  {path of the yaml file that we defined the servers}
```

# How to run a ad-hoc command
```bash
ansible -i {path of the yaml file that we defined the servers , or we could say all} { server name } -m command -a "apt install nginx -y"  #bad practice for package installation , works without ansible intell

ansible -i {path of the yaml file that we defined the servers , or we could say all} { server name } -m apt -a "name=nginx state=present update_cache=yes" 

```
Or getting uptime
```bash
ansible -i {path of the yaml file that we defined the servers , or we could say all} { server name } -m command -a "uptime"
```