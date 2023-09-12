# GitLearning
This is a Readme file
---
- name: Check Kernel Version
  hosts: your_target_hosts
  gather_facts: yes
  tasks:
    - name: Get Kernel Version
      command: uname -r
      register: kernel_version

    - name: Display Kernel Version
      debug:
        var: kernel_version.stdout
