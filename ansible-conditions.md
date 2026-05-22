# We can write conditions
Implementing conditions helps us filter the tasks based on the server facts

We could filter based various filtwea like

## OS Family

Like debian based , redhat based  and etc.

```yml
---
- name: update apt cache
      ansible.builtin.apt:
        update_cache: yes
      
      when: ansible_os_family == "Debian"
- name: update apt cache
      ansible.builtin.yum:
        update_cache: yes
      
      when: ansible_os_family == "RedHat"

```

## OS distrubitions
Like debian , Ubuntu , almalinux and Rocky based  and etc.


```yml
---
- name: update apt cache
      ansible.builtin.apt:
        update_cache: yes
    when: ansible_distribution == "Debian"
- name: update apt cache
      ansible.builtin.apt:
        update_cache: yes
    when: ansible_distribution == "Ubuntu"
```


### common_enterprise_distributions
```txt
  - RedHat      # RHEL - The enterprise standard
  - Ubuntu      # Cloud & developer favorite  
  - Rocky       # CentOS replacement #1
  - AlmaLinux   # CentOS replacement #2
  - Amazon      # AWS cloud native
  - Debian      # Conservative stable choice
  - SLES        # SAP & European enterprise
  - Windows     # Mixed environments
```
#### other conditions ansible support

##### Operating System & Distribution

```ansible_distribution```: The specific distribution name ```(e.g., Ubuntu, CentOS, RedHat)``` .

```ansible_distribution_major_version```: The major release version of the distribution ```(e.g., 18, 7)``` .

```ansible_distribution_version```: The full distribution version string.

```ansible_kernel```: The version of the operating system kernel.

##### Hardware & Architecture

```ansible_architecture```: The system's CPU architecture ```(e.g., x86_64, aarch64)``` .

```ansible_processor```: Information about the CPU(s), often a list of processor cores.

```ansible_processor_cores```: The number of CPU cores per socket.

```ansible_memtotal_mb```: The total system memory in megabytes.

```ansible_swaptotal_mb```: The total swap space in megabytes.

##### Network & Identity

```ansible_all_ipv4_addresses```: A list of all IPv4 addresses configured on the system .

```ansible_default_ipv4```: A dictionary containing details of the default network interface and its IPv4 address.

```ansible_fqdn```: The Fully Qualified Domain Name of the system.

```ansible_hostname```: The short hostname of the system.

```ansible_nodename```: The system's canonical hostname.

##### System & Ansible Environment

```ansible_os_family```: A broader grouping ```(e.g., RedHat, Debian, Suse, Windows)``` .

```ansible_service_mgr```: The service management system in use ```(e.g., systemd, sysvinit)``` .

```ansible_pkg_mgr```: The package manager for the OS ```(e.g., apt, yum, dnf)``` .

```ansible_virtualization_type```: The type of virtualization technology ```(e.g., docker, kvm, xen)```.






# other types of conditions

## Notify handlers

we can notify a handler to exec if the notifier actualy did some thing.
Like
```yml
---
tasks:
    - name: Configure systemd-resolved with custom DNS
    ansible.builtin.copy:
        dest: /etc/systemd/resolved.conf
        content: |
        [Resolve]
        DNS={{ new_dns_servers | join(' ') }}
        # Fallback to Google DNS if ours fail
        FallbackDNS={{ new_FallbackDNS_servers | join(' ') }}
        # Don't act as DNS stub listener
        DNSStubListener=no
        owner : root
        group : root
        mode: '0644'
        backup: yes
    notify: restart systemd-resolved

handlers:
  - name: restart systemd-resolved
    ansible.builtin.service:
      name: systemd-resolved
      state: restarted
      enabled: yes
```