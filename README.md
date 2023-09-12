---
- name: Check and Remove Kernel
  hosts: your_target_hosts
  gather_facts: yes
  tasks:
    - name: Get Current Kernel Version
      command: uname -r
      register: current_kernel

    - name: Display Current Kernel Version
      debug:
        var: current_kernel.stdout

    - name: List Installed Kernels
      command: rpm -q kernel --queryformat '%{version}-%{release}.%{arch}\n'
      register: installed_kernels

    - name: Display Installed Kernels
      debug:
        var: installed_kernels.stdout_lines

    - name: Prompt for Kernel Removal
      pause:
        prompt: "Enter the kernel version to remove (e.g., 4.18.0-305.el8.x86_64): "
      register: selected_kernel

    - name: Remove Selected Kernel
      yum:
        name: "kernel-{{ selected_kernel.user_input }}"
        state: absent
      when: selected_kernel.user_input != '' and selected_kernel.user_input in installed_kernels.stdout_lines
      
