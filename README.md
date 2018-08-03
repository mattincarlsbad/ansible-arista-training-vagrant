# ansible-arista-training-vagrant
Check out the Ansible Getting Started documentation here: http://docs.ansible.com/ansible/latest/intro_getting_started.html

To use these playbooks on vagrant vm's, refer to this repo: https://github.com/mattincarlsbad/vagrant-ansible-arista, and ensure all vm's are running prior to running any playbooks.

Try out the "Crawl" and "Walk" playbooks from the vagrant ansible vm.  The playbooks should already be in the ansible vm.  If not, use git to download the playbooks from github to the ansible vm.

vagrant ssh ansible

git clone https://github.qualcomm.com/itnet-dc/ansible-arista-training-vagrant

```bash
mattt@san-itnet-vagrant:~/vagrant-ansible-arista$ vagrant ssh ansible
Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.4.0-87-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage
Last login: Fri Jan 12 20:43:33 2018 from 10.0.2.2
vagrant@ubuntu:~$ ls
ansible-arista-training-vagrant
vagrant@ubuntu:~$ 
```

If you need to download the plabooks:

```bash
vagrant@ubuntu:~$ git clone https://github.com/mattincarlsbad/ansible-arista-training-vagrant
Cloning into 'ansible-arista-training-vagrant'...
remote: Counting objects: 111, done.
remote: Compressing objects: 100% (69/69), done.
remote: Total 111 (delta 37), reused 111 (delta 37), pack-reused 0
Receiving objects: 100% (111/111), 14.70 KiB | 0 bytes/s, done.
Resolving deltas: 100% (37/37), done.
Checking connectivity... done.
vagrant@ubuntu:~$ ls
ansible-arista-training-vagrant
vagrant@ubuntu:~$ 
```
These playbooks asume switches are configured for admin/admin login, and will not work with AAA/TACACS configured.
View the Crawl playbook and hosts file:
```bash
vagrant@ubuntu:~/ansible-arista-training-vagrant/crawl$ cat show-version.yaml 
---
- hosts: arista 
  connection: local
  gather_facts: no

  tasks:
  - name: Gather info from Show Command
    eos_command:
      commands: show version
      username: admin
      password: admin
      transport: eapi
      validate_certs: no
vagrant@ubuntu:~/ansible-arista-training-vagrant/crawl$ 
vagrant@ubuntu:~/ansible-arista-training-vagrant/crawl$ cat hosts             
[arista]
leaf1 ansible_host=10.0.0.1
leaf2 ansible_host=10.0.0.2
spine1 ansible_host=10.0.0.3
spine2 ansible_host=10.0.0.4
vagrant@ubuntu:~/ansible-arista-training-vagrant/crawl$
```
Run the Crawl playbook:
```bash
vagrant@ubuntu:~/ansible-arista-training-vagrant/crawl$ ansible-playbook -i hosts show-version.yaml -v
Using /etc/ansible/ansible.cfg as config file

PLAY [arista] ******************************************************************************************************************************************************************************************************************************

TASK [Gather info from Show Command] *******************************************************************************************************************************************************************************************************
 [WARNING]: argument username has been deprecated and will be removed in a future version

 [WARNING]: argument password has been deprecated and will be removed in a future version

ok: [spine2] => {"changed": false, "failed": false, "stdout": [{"architecture": "i386", "bootupTimestamp": 1514499274.62, "hardwareRevision": "", "internalBuildId": "3d3db99a-d291-4527-86d2-b296f4998d48", "internalVersion": "4.18.5M-6710299.4185M", "isIntlVersion": false, "memFree": 876972, "memTotal": 1893384, "modelName": "vEOS", "serialNumber": "", "systemMacAddress": "08:00:27:b9:5d:fa", "version": "4.18.5M"}], "stdout_lines": [{"architecture": "i386", "bootupTimestamp": 1514499274.62, "hardwareRevision": "", "internalBuildId": "3d3db99a-d291-4527-86d2-b296f4998d48", "internalVersion": "4.18.5M-6710299.4185M", "isIntlVersion": false, "memFree": 876972, "memTotal": 1893384, "modelName": "vEOS", "serialNumber": "", "systemMacAddress": "08:00:27:b9:5d:fa", "version": "4.18.5M"}]}
ok: [spine1] => {"changed": false, "failed": false, "stdout": [{"architecture": "i386", "bootupTimestamp": 1514499158.97, "hardwareRevision": "", "internalBuildId": "3d3db99a-d291-4527-86d2-b296f4998d48", "internalVersion": "4.18.5M-6710299.4185M", "isIntlVersion": false, "memFree": 880336, "memTotal": 1893384, "modelName": "vEOS", "serialNumber": "", "systemMacAddress": "08:00:27:e4:59:7c", "version": "4.18.5M"}], "stdout_lines": [{"architecture": "i386", "bootupTimestamp": 1514499158.97, "hardwareRevision": "", "internalBuildId": "3d3db99a-d291-4527-86d2-b296f4998d48", "internalVersion": "4.18.5M-6710299.4185M", "isIntlVersion": false, "memFree": 880336, "memTotal": 1893384, "modelName": "vEOS", "serialNumber": "", "systemMacAddress": "08:00:27:e4:59:7c", "version": "4.18.5M"}]}
ok: [leaf2] => {"changed": false, "failed": false, "stdout": [{"architecture": "i386", "bootupTimestamp": 1514499049.97, "hardwareRevision": "", "internalBuildId": "3d3db99a-d291-4527-86d2-b296f4998d48", "internalVersion": "4.18.5M-6710299.4185M", "isIntlVersion": false, "memFree": 877500, "memTotal": 1893384, "modelName": "vEOS", "serialNumber": "", "systemMacAddress": "08:00:27:56:7a:23", "version": "4.18.5M"}], "stdout_lines": [{"architecture": "i386", "bootupTimestamp": 1514499049.97, "hardwareRevision": "", "internalBuildId": "3d3db99a-d291-4527-86d2-b296f4998d48", "internalVersion": "4.18.5M-6710299.4185M", "isIntlVersion": false, "memFree": 877500, "memTotal": 1893384, "modelName": "vEOS", "serialNumber": "", "systemMacAddress": "08:00:27:56:7a:23", "version": "4.18.5M"}]}
ok: [leaf1] => {"changed": false, "failed": false, "stdout": [{"architecture": "i386", "bootupTimestamp": 1514498947.03, "hardwareRevision": "", "internalBuildId": "3d3db99a-d291-4527-86d2-b296f4998d48", "internalVersion": "4.18.5M-6710299.4185M", "isIntlVersion": false, "memFree": 861184, "memTotal": 1893384, "modelName": "vEOS", "serialNumber": "", "systemMacAddress": "08:00:27:66:fb:5b", "version": "4.18.5M"}], "stdout_lines": [{"architecture": "i386", "bootupTimestamp": 1514498947.03, "hardwareRevision": "", "internalBuildId": "3d3db99a-d291-4527-86d2-b296f4998d48", "internalVersion": "4.18.5M-6710299.4185M", "isIntlVersion": false, "memFree": 861184, "memTotal": 1893384, "modelName": "vEOS", "serialNumber": "", "systemMacAddress": "08:00:27:66:fb:5b", "version": "4.18.5M"}]}

PLAY RECAP *********************************************************************************************************************************************************************************************************************************
leaf1                      : ok=1    changed=0    unreachable=0    failed=0   
leaf2                      : ok=1    changed=0    unreachable=0    failed=0   
spine1                     : ok=1    changed=0    unreachable=0    failed=0   
spine2                     : ok=1    changed=0    unreachable=0    failed=0   

vagrant@ubuntu:~/ansible-arista-training-vagrant/crawl$ 
```
View the "show hostname" Walk playbook:
```bash
vagrant@ubuntu:~/ansible-arista-training-vagrant/walk$ cat arista-show.yaml 
---
- hosts: arista 
  connection: local
  gather_facts: no

  tasks:
  - name: Gather info from Show Command
    eos_command:
      commands: show hostname
      provider: "{{ eos_connection }}"
vagrant@ubuntu:~/ansible-arista-training-vagrant/walk$
```
Run the "show hostname" Walk playbook:
```bash
vagrant@ubuntu:~$ cd ansible-arista-training-vagrant/walk/
vagrant@ubuntu:~/ansible-arista-training/walk$ ansible-playbook -i hosts arista-show.yaml -v
Using /home/vagrant/ansible-arista-training/walk/ansible.cfg as config file
[DEPRECATION WARNING]: [defaults]hostfile option, The key is misleading as it can also be a list of hosts, a directory or a list of paths . This feature will be removed in version 2.8. Deprecation warnings can be disabled by setting 
deprecation_warnings=False in ansible.cfg.

PLAY [arista] ******************************************************************************************************************************************************************************************************************************

TASK [Gather info from Show Command] *******************************************************************************************************************************************************************************************************
ok: [spine2] => {"changed": false, "failed": false, "stdout": [{"fqdn": "spine2", "hostname": "spine2"}], "stdout_lines": [{"fqdn": "spine2", "hostname": "spine2"}]}
ok: [spine1] => {"changed": false, "failed": false, "stdout": [{"fqdn": "spine1", "hostname": "spine1"}], "stdout_lines": [{"fqdn": "spine1", "hostname": "spine1"}]}
ok: [leaf1] => {"changed": false, "failed": false, "stdout": [{"fqdn": "leaf1", "hostname": "leaf1"}], "stdout_lines": [{"fqdn": "leaf1", "hostname": "leaf1"}]}
ok: [leaf2] => {"changed": false, "failed": false, "stdout": [{"fqdn": "leaf2", "hostname": "leaf2"}], "stdout_lines": [{"fqdn": "leaf2", "hostname": "leaf2"}]}

PLAY RECAP *********************************************************************************************************************************************************************************************************************************
leaf1                      : ok=1    changed=0    unreachable=0    failed=0   
leaf2                      : ok=1    changed=0    unreachable=0    failed=0   
spine1                     : ok=1    changed=0    unreachable=0    failed=0   
spine2                     : ok=1    changed=0    unreachable=0    failed=0   

vagrant@ubuntu:~/ansible-arista-training-vagrant/walk$ 
```
View the "set hostname" Walk playbook, and host file.  This playbook sets the switch host names to the names in the hosts file (leaf01, leaf02, etc.), then saves the configuration.  The original host names for the switches created by vagrant are leaf1, leaf2, etc..
```bash
vagrant@ubuntu:~/ansible-arista-training-vagrant/walk$ cat arista-configure.yaml 
---
- hosts: arista 
  connection: local
  gather_facts: no

  tasks:
  - name: Set Hostname
    eos_config:
      lines: hostname {{ inventory_hostname }}
      provider: "{{ eos_connection }}"

  - name: Save Configuration
    eos_config:
      lines: write memory
      provider: "{{ eos_connection }}"
vagrant@ubuntu:~/ansible-arista-training-vagrant/walk$ cat hosts 
[arista]
leaf01 ansible_host=10.0.0.1
leaf02 ansible_host=10.0.0.2
spine01 ansible_host=10.0.0.3
spine02 ansible_host=10.0.0.4
vagrant@ubuntu:~/ansible-arista-training-vagrant/walk$
```
Run the the "set hostname" Walk playbook.
```bash
vagrant@ubuntu:~/ansible-arista-training-vagrant/walk$ ansible-playbook -i hosts arista-configure.yaml 
[DEPRECATION WARNING]: [defaults]hostfile option, The key is misleading as it can also be a list of hosts, a directory or a list of paths . This feature will be removed in version 2.8. Deprecation warnings can be disabled by setting 
deprecation_warnings=False in ansible.cfg.

PLAY [arista] ******************************************************************************************************************************************************************************************************************************

TASK [Set Hostname] ************************************************************************************************************************************************************************************************************************
changed: [leaf02]
changed: [leaf01]
changed: [spine01]
changed: [spine02]

TASK [Save Configuration] ******************************************************************************************************************************************************************************************************************
changed: [leaf02]
changed: [leaf01]
changed: [spine01]
changed: [spine02]

PLAY RECAP *********************************************************************************************************************************************************************************************************************************
leaf01                     : ok=2    changed=2    unreachable=0    failed=0   
leaf02                     : ok=2    changed=2    unreachable=0    failed=0   
spine01                    : ok=2    changed=2    unreachable=0    failed=0   
spine02                    : ok=2    changed=2    unreachable=0    failed=0   

vagrant@ubuntu:~/ansible-arista-training-vagrant/walk$ 
```
Run the "show hostname" Walk playbook again to verify the hostname change:
```bash
vagrant@ubuntu:~/ansible-arista-training-vagrant/walk$ ansible-playbook -i hosts arista-show.yaml -v   
Using /home/vagrant/ansible-arista-training/walk/ansible.cfg as config file
[DEPRECATION WARNING]: [defaults]hostfile option, The key is misleading as it can also be a list of hosts, a directory or a list of paths . This feature will be removed in version 2.8. Deprecation warnings can be disabled by setting 
deprecation_warnings=False in ansible.cfg.

PLAY [arista] ******************************************************************************************************************************************************************************************************************************

TASK [Gather info from Show Command] *******************************************************************************************************************************************************************************************************
ok: [leaf02] => {"changed": false, "failed": false, "stdout": [{"fqdn": "leaf02", "hostname": "leaf02"}], "stdout_lines": [{"fqdn": "leaf02", "hostname": "leaf02"}]}
ok: [leaf01] => {"changed": false, "failed": false, "stdout": [{"fqdn": "leaf01", "hostname": "leaf01"}], "stdout_lines": [{"fqdn": "leaf01", "hostname": "leaf01"}]}
ok: [spine02] => {"changed": false, "failed": false, "stdout": [{"fqdn": "spine02", "hostname": "spine02"}], "stdout_lines": [{"fqdn": "spine02", "hostname": "spine02"}]}
ok: [spine01] => {"changed": false, "failed": false, "stdout": [{"fqdn": "spine01", "hostname": "spine01"}], "stdout_lines": [{"fqdn": "spine01", "hostname": "spine01"}]}

PLAY RECAP *********************************************************************************************************************************************************************************************************************************
leaf01                     : ok=1    changed=0    unreachable=0    failed=0   
leaf02                     : ok=1    changed=0    unreachable=0    failed=0   
spine01                    : ok=1    changed=0    unreachable=0    failed=0   
spine02                    : ok=1    changed=0    unreachable=0    failed=0   

vagrant@ubuntu:~/ansible-arista-training-vagrant/walk$
```
And/or, ssh into one of the switches to verify the hostname:
```bash
vagrant@ubuntu:~/ansible-arista-training-vagrant/walk$ exit
logout
Connection to 127.0.0.1 closed.
[mattt1-mac:~/vagrant/vagrant-ansible-arista] mattt% vagrant ssh leaf1
Last login: Wed Jan  3 01:00:54 2018 from 10.0.2.2

Arista Networks EOS shell

-bash-4.3# FastCli
leaf01>
```
