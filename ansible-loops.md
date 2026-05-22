# Defining loops
Loops becomes most needed when we have a repeated task 
so we make the ansible task reusable with loops like
Insread of several
```yml
---
- name: Create a basic user
    ansible.builtin.user:
        name: johndoe
        state: present
```
We do
```yml
---
- name: Create a basic user
    ansible.builtin.user:
        name: "{{ item }}"
        state: present
    loop:
    - ali
    - reza
    - javad
```