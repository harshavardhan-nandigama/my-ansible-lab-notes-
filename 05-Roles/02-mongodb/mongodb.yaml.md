Playbook snippet

  - name: configure mongodb component
    become: yes
    hosts: mongodb
    roles:
        - mongodb
        
This playbook calls the mongodb role for all hosts under the mongodb inventory group, running tasks with elevated privileges.