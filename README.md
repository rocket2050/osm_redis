This palybook for setting master-slave setup

hosts file
[redis-master]
redis-master.example.com

[redis-slave]
redis-slave0[1:3].example.com



site.yml

---
- name: configure the master redis server
  hosts: redis-master
  roles:
    - DavidWittman.redis

- name: configure redis slaves
  hosts: redis-slave
  vars:
    - redis_slaveof: "{{ hostvars['redis-master.example.com'].ansible_eth1.ipv4.address }} {{ redis_port }}" 
  roles:
    - DavidWittman.redis



