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
> [!WARNING]
> It is a **bad practice** using secrets inside the code.
> We should try using secret managers like **HashiCorp Vault**.
> Some providers offer vaults too — I will write samples about it.

Notice the rows of the Dictionary sample are Strings , since they have "

# Other Vars:
# Ansible python path
It defines with : ```ansible_python_interpreter: /usr/bin/python3.12```

## Variable Precedence

Variables can be defined in multiple places with different values. Understanding the precedence order helps you predict which value will be used.

> [!WARNING]
> Variable precedence determines **which definition wins** when the same variable is defined in multiple locations.
> Lower numbers = lower priority (overwritten by higher numbers).

### Precedence Order (Least to Most Important)

| Priority | Level | Scope | Example |
|:--------:|-------|-------|---------|
| 1 | **Default values** | Built-in defaults | `variable "timeout" { default = 30 }` |
| 2 | **Inventory variables** | Host/group vars | `group_vars/all.yml` |
| 3 | **Playbook variables** | Play level | `vars:` block in playbook |
| 4 | **Role defaults** | Role level | `roles/role_name/defaults/main.yml` |
| 5 | **Role vars** | Role level | `roles/role_name/vars/main.yml` |
| 6 | **Block variables** | Task block | `block:` with `vars:` |
| 7 | **Task variables** | Individual task | `vars:` inside a task |
| 8 | **Include variables** | Dynamic includes | `include_vars:` |
| 9 | **Set_fact** | Runtime facts | `set_fact:` module |
| 10 | **Extra vars** | Command line | `ansible-playbook -e "var=value"` |

> [!IMPORTANT]
> **Highest precedence (10)** : `--extra-vars` or `-e` flags on the command line will **always** override all other definitions.