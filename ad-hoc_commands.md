# How to ping servers ?
```bash
ansible -i {path of the yaml inventory file or all} {server name} -m ping
```


# How to run a ad-hoc command
```bash
ansible -i {path of the yaml inventory file or all} { server name or none } -m command -a "apt install nginx -y"
rgb(59, 28, 121) practice for package installation , works without ansible intell

ansible -i {path of the yaml file that we defined the servers , or we could say all} { server name } -m apt -a "name=nginx state=present update_cache=yes" 

```
Or getting uptime
```bash
ansible -i {path of the yaml inventory file or all} { server name } -m command -a "uptime"
```
# Using ad-hoc to get server info

```
ansible -i {path of the yaml file that we defined the servers , or we could say all} { server name } -m setup
```


