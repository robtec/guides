# Sample Ansible Playbook file

`playbook.yml`
```
---
- hosts: all
  become: true
  tasks:
    - name: "Task 1"
      command: echo "Hello, from task one"
        
    - name: "Task 2"
      command: echo "Hello, from task two"

    - name: "Task 3"
      import_tasks: sleep.yml
      throttle: 1
```
`sleep.yml`
```
- name: sleep for five seconds
  command:
    cmd: sleep 5
```
`inventory`
```
[server1]
0.0.0.0

[server2]
1.0.1.0
```

## Ansible Command
`$ ansible-playbook -v -u <USER> -i inventory playbook.yml`
