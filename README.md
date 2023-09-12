---
- name: Check and Restart HTTPS Service Interactively
  hosts: your_target_hosts
  gather_facts: no
  become: yes
  tasks:
    - name: Check HTTPS Service Status
      systemd:
        name: httpd  # Replace with 'nginx' if using Nginx
      register: service_status
      ignore_errors: yes  # To prevent playbook failure if the service is not found

    - name: Prompt for Service Restart
      pause:
        prompt: "The HTTPS service is not running. Do you want to restart it? (yes/no): "
      register: user_input

    - name: Restart HTTPS Service
      systemd:
        name: httpd  # Replace with 'nginx' if using Nginx
        state: restarted
      when: user_input.user_input | lower == "yes" or service_status.failed
      
