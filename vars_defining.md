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
