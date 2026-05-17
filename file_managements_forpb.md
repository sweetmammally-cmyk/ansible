# echo some the chars into a txt file
```yml
---

- name: Configure systemd-resolved with custom DNS
  tasks:
    ansible.builtin.copy:   # this does copy things into
      dest: /etc/systemd/resolved.conf # a destined file
      content: | # we give the texts to him here
      Raw TXT
        
    owner : root # since its a new file it defines the access privilages here  
    group : root
    mode: '0644'
    backup: yes
    notify: restart systemd-resolve # we do notify a reusable service in handlers

```

# creating a new folder
```yml
---
- name: Create directory
  ansible.builtin.file: # it does create a folder in destinated path
    path: /tmp/tmpdocs
    state: directory    
    mode: '0755'
```

# delete a folder
```yml
---
- name: delete directory
  ansible.builtin.file: # it does delete a folder in destinated path
    path: /tmp/tmpdocs
    state: absent    
    mode: '0755'
```

# apply attributes to destinated folder new folder
```yml
---
- name: atr directory
  ansible.builtin.file: # it does apply atrributes to a file folder in destinated path
    path: /tmp/tmpdocs
    state: file    
    mode: '0755'
```


# touch file
```yml
---
- name: touch file
  ansible.builtin.file: # it does create a file in destinated path
    path: /tmp/tmpdocs
    state: touch    
    mode: '0755'
```

#  Create a symbolic link
```yml
---
- name: Create a symbolic link
  ansible.builtin.file: # creates symbolic link
    src: /etc/myapp/config.conf  # The existing target file
    path: /tmp/config_shortcut   # The symlink you want to create
    state: link
```
# Create a hard link
```yml
---
- name: Create a hard link
  ansible.builtin.file: # Create a hard link
    src: /tmp/original_file.txt  # The existing target file
    path: /tmp/hardlink_copy.txt # The hard link you want to create
    state: hard
```