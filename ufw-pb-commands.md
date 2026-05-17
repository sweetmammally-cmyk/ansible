# reseting the firewall
```yml
---
- name: Reset UFW to factory defaults
  community.general.ufw:
    state: reset
  ignore_errors: true
```
# allowing a port

```yml
---
- name: Allow SSH on a custom port
  community.general.ufw:
    rule: allow
    port: "{{ ssh_port }}"
    proto: tcp
    comment: "Allow custom SSH access"
```

# adding our current ssh port then starting ufw
```yml
---
- name: Allow SSH before enabling UFW
  community.general.ufw:
    rule: allow
    port: '22'
    proto: tcp

- name: Enable UFW and set default incoming policy to deny
  community.general.ufw:
    state: enabled
    direction: incoming
    policy: deny

```

# checking the status of the ufw
```yml
---
- name: Check UFW status without triggering a "changed" state
  ansible.builtin.command: ufw status verbose
  register: ufw_status
  changed_when: false # Tells Ansible this is a read-only command (keeps it green, not yellow)
```