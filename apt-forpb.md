# adding an apt rippo
```yml
---
- name: Add Docker repository (Ubuntu)
  ansible.builtin.apt_repository:
    repo: "deb [arch=amd64 signed-by=/etc/apt/keyrings/docker.gpg] {{ docker_mirror_ubuntu }} {{ ansible_distribution_release }} stable"
    filename: docker
    state: present
  when: ansible_distribution == "Ubuntu"
```
this code does create a source file in the /etc/apt/source.list.d/
feturing the defined txt parts
