# We can define variables 
for ip addreases , mirros , and any thing changes over time 
We use Variables here

# Instead of hardcoding alike
```yml
---
- name: Set DNS servers
  ansible.builtin.lineinfile:
    path: /etc/systemd/resolved.conf
    regexp: '^DNS='
    line: "DNS=8.8.8.8"
```
# We do

# 1st :
Dedining Vars inside the group vars :
```yml
---
new_dns_servers:
      - "217.218.127.127"
      - "217.218.155.155"
```
# 2nd Implementing the Reusable logic
```yml
---
- name: Set DNS servers
      ansible.builtin.lineinfile:
        path: /etc/systemd/resolved.conf
        regexp: '^DNS='
        line: "DNS={{ new_dns_servers | join(' ') }}"
```
The ``` join(' ') ``` means join with empty space

# Variable Types :

# Strings :
We do have strings
In string we put it inside " , like ``` username: "admin" ```

Ip addreasses are string
We do use " , when if we don't the code may break
For example the ```https://repo.mecan.ir/repository/debian-docker/gpg```
Doesn't break the code but the ```https://example.com:8080``` does

If we have Contains special characters ```(:, @, #, &, *, etc.)```
The code may break without "
If we have white space it may too
If it has ```{{ or }}``` also
And the ```https://192.168.1.1``` Looks like a number for ansible so " is required.

# Numbers
In string we just put numbers and ansible is smart enough
```yml
---
max_connection: 100
```
# booleans
Its just the false/true conditions like ```debug_mode: true```
yes/no is accepted some times.
# lists
```yml
---
packages:
    - nginx
    - git
    - curl
```
As you see in the sample , the common usage is for list of apps beibg installed
# Dictionaries
Sample :
```yml
---
user:
    name: "admin"
    password: "bad-practice"

```
```txt
It is a bad practice , using secrets inside the code
We should try using secret managers like hashicorp vault
some providers provide vaults too , I will write some samples about it.
```
Notice the rows of the Dictionary sample are Strings , since they have "

# Other Vars:
# Ansible python path
It defines with : ```ansible_python_interpreter: /usr/bin/python3.12```

