This palybook for setting master-slave setup

Below are a few example playbooks and configurations for deploying a variety of Redis architectures.


## Single Redis Instalation 

``` yml
---
- hosts: redis01.example.com
  vars:
    - redis_bind: 127.0.0.1
  roles:
    - DavidWittman.redis
```

``` bash
$ ansible-playbook -i redis01.example.com, redis.yml
```


### Master-Slave replication

Configuring [replication](http://redis.io/topics/replication) in Redis is accomplished by deploying multiple nodes, and setting the `redis_slaveof` variable on the slave nodes, just as you would in the redis.conf. In the example that follows, we'll deploy a Redis master with three slaves.

In this example, we're going to use groups to separate the master and slave nodes. Let's start with the inventory file:

``` ini
[redis-master]
redis-master.example.com

[redis-slave]
redis-slave0[1:3].example.com
```

And here's the playbook:

``` yml
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
```



