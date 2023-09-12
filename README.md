---
- name: Check Power Status via IPMI
  hosts: your_target_hosts
  gather_facts: no
  vars_prompt:
    - name: ipmi_username
      prompt: "Enter IPMI Username"
      private: no

    - name: ipmi_password
      prompt: "Enter IPMI Password"
      private: yes
  tasks:
    - name: Get Power Status
      ipmi_power:
        state: status
        host: "{{ ipmi_host }}"
        username: "{{ ipmi_username }}"
        password: "{{ ipmi_password }}"
        interface: lanplus
      delegate_to: localhost
      register: power_status

    - name: Display Power Status
      debug:
        var: power_status.msg
      
