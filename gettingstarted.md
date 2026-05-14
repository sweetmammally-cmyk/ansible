# How to ping servers ?
'''bash
ansible -i path of the yaml file that we defined the servers_server name -m ping
'''

# How to run a playbook
'''bash
ansible-playbook playbook path --limit server name -i  path of the yaml file that we defined the servers
'''