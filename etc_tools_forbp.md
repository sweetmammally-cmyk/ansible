# how to curl , without installing
```yml
---
- name: Download Docker GPG key (Ubuntu)
  ansible.builtin.get_url:
    url: "{{ ubuntu_docker_gpg }}"
    dest: /etc/apt/keyrings/docker.gpg
    mode: '0644'
  when: ansible_distribution == "Ubuntu"
---
```
by adding ```force``` metric to true , it will do owerwrite it and no idempotency 