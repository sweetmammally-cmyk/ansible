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

#### 1 Default values
 are built-in fallback values
#### 2 Inventory variables
 are variables defined in your inventory
#### 3 Playbook variables
 are variables defined directly inside a playbook
#### 4 Role defaults
are the default variables defined inside a reusable role's defaults/main.yml file. These are safe, low-priority defaults intended to be overridden.
#### 5 Role vars
are variables defined inside a role's vars/main.yml file. Unlike defaults/, these cannot be easily overridden by the user.

#### 6 Block variables
are variables defined within a block: directive that apply only to the tasks inside that specific block.

like 
```yml
---
tasks:
  - block:
      - name: Task 1
        command: echo "{{ timeout }}"
      - name: Task 2
        command: check_timeout
      vars:
        timeout: 85  # Only applies to tasks inside this block
    - name: Task 3 outside block
      command: echo "{{ timeout }}"  # Uses previous value (e.g., 75)
```
#### 7 Task variables
defined directly inside an individual task using the vars: keyword, applying only to that specific task.

like 
```yml
---
tasks:
  - name: Special deployment
    command: deploy_app
    vars:
      timeout: 95  # Only affects this one task
---
```

#### 8 Include variables
Variables loaded dynamically at runtime from external files using ```include_vars:``` or ```vars_files:```.

like
```yml
---
tasks:
  - name: Load environment-specific vars
    include_vars: "{{ env }}_config.yml"
    # If env=production, loads production_config.yml
    # which might contain: timeout: 110
```
#### 9. Set_fact
Variables created or modified during playbook execution using the ```set_fact``` module. These persist for the entire playbook run.

like
```yml
---
tasks:
  - name: Calculate dynamic timeout
    set_fact:
      timeout: "{{ 120 + 10 }}"  # Results in 130
  
  - name: Use calculated value
    command: deploy --timeout {{ timeout }}
```
#### 10. Extra vars
Variables passed directly from the command line using the -e or --extra-vars flag. These are the highest precedence—nothing overrides them.

```yml
---
# Command line execution
ansible-playbook deploy.yml -e "timeout=999"
```


