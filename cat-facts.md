# Getting data from target servers
```yml
---
- name: print the vars
    debug:
    msg:    "Hello World!"
```

## Ansible builtin stat

Here we do create a task , with the ```ansible.builtin.stat``` , checking stats of a file
then we save the output in to a variable via ```register```

In the secons task we report the variable to the ansible server via : ```ansible.builtin.debug```

```yml
---
- name: Simple stat example
  hosts: localhost
  
  tasks:
    # Check if a file exists and save the result
    - name: Check if test.txt exists
      ansible.builtin.stat:
        path: /tmp/test.txt
      register: file_info   # Save results into 'file_info' variable

    # Show what we found
    - name: Display file status
      ansible.builtin.debug:
        msg: "File exists? {{ file_info.stat.exists }}"
      #                    ^^^^^^^^^^^^^^^^^^^^^^^^
      #                    Accessing the 'exists' property from saved results

    # Create file only if it doesn't exist
    - name: Create file if missing
      ansible.builtin.file:
        path: /tmp/test.txt
        state: touch
      when: not file_info.stat.exists
      #     ^^^ Only run this task if file does NOT exist

```
look at the condition ```file_info.stat.exists```

see the server variables , can be used as conditions
in this case the output that we store via ```ansible.builtin.stat``` , is readable
for the ```stat``` module , and ansible can use it as conditions

It would be a good practice to check this sample in a server , and print the whole 
```msg: "File exists? {{ file_info }}"``` so seeing what conditions are supported.
