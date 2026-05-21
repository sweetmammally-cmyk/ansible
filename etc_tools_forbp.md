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

# secure download with checsum 
```yml
---
- name: Download a file securely with a checksum check
  ansible.builtin.get_url:
    url: https://example.com/software/app.bin
    dest: /usr/local/bin/app.bin
    checksum: sha256:9f86d081884c7d659a2feaa0c55ad015a3bf4f1b2b0b822cd15d6c15b0f00a08
    mode: '0755'  # You can also set file permissions directly!
```
# you can download from apis
```yml
---
- name: Download from an API that returns 201 Created
  ansible.builtin.get_url:
    url: https://api.example.com/v1/export/report.csv
    dest: /tmp/report.csv
    # Tells Ansible to treat 201 as a successful download instead of failing
    status_code: [200, 201]

```