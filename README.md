## Kafka Ansible Role


[![CI](https://github.com/bilalcaliskan/kafka-ansible-role/workflows/CI/badge.svg?event=push)](https://github.com/bilalcaliskan/kafka-ansible-role/actions?query=workflow%3ACI)

Installs and configures Kafka cluster. By default, this role sets up zookeeper and kafka on the same server. But you can change this behavior by setting below variables while running the playbook. Check the `Examples` section for ready to use examples.

```
seperate_zookeepers: true
zookeepers: 
  - zookeepernode01
```

### Requirements

This role requires minimum Ansible version 2.4 and maximum Ansible version 2.9. You can install suggested version with pip:
```
$ pip install "ansible==2.9.16"
```

This installation requires Zookeeper; also note that this role requires root access, so either run it in a playbook with a global `become: true`, or invoke the role in your playbook. Here is the example with the global `become: true`:

```yaml
- hosts: all
  become: true
  roles:
    - role: bilalcaliskan.kafka
      vars:
        simple_role_var: foo
```

### Role Variables

See the default values in [defaults/main.yml](defaults/main.yml). You can overwrite them in [vars/main.yml](vars/main.yml) if neccessary or you can set them while running playbook.

Also please see the Redhat ansible_os_family specific variables in [vars/redhat.yml](vars/redhat.yml) and Debian ansible_os_family specific variables in [vars/debian.yml](vars/debian.yml).

> Please note that this role will ensure that `firewalld` systemd service on your servers are started and enabled by default. If you want to stop and disable `firewalld` service, please modify below variable as false when running playbook:  
> ```yaml  
> firewalld_enabled: false

### Dependencies

That role requires [bilalcaliskan.zookeeper](https://galaxy.ansible.com/bilalcaliskan/zookeeper) role


### Examples
#### Seperate Zookeeper, Seperate Kafka Installation

*inventory.ini:*
```
[zookeepers]
  zookeepernode01

[brokers]
  brokernode02
```

*playbook.yaml:*
```yaml
---

- name: Zookeeper role execution play
  hosts: zookeepers
  become: true
  roles:
    - role: bilalcaliskan.zookeeper
      vars:
        install: true

- name: Kafka role execution play
  hosts: brokers
  become: true
  roles:
    - role: bilalcaliskan.kafka
      vars:
        install_kafka: true
        install_zookeeper: false
        seperate_zookeepers: true
        zookeepers: 
          - zookeepernode01

```

#### Seperate Zookeeper, Seperate Kafka Uninstallation
*inventory.ini:*
```
[zookeepers]
  zookeepernode01

[brokers]
  brokernode02
```

*playbook.yaml:*
```yaml
---

- name: Zookeeper role execution play
  hosts: zookeepers
  become: true
  roles:
    - role: bilalcaliskan.zookeeper
      vars:
        install: false

- name: Kafka role execution play
  hosts: brokers
  become: true
  roles:
    - role: bilalcaliskan.kafka
      vars:
        install_kafka: false
        install_zookeeper: false
        seperate_zookeepers: true
        zookeepers: 
          - zookeepernode01

```

#### Kafka + Zookeeper on the same server installation

*inventory.ini:*
```
[brokers]
  brokernode01
  brokernode02
  brokernode03
```

*playbook.yaml:*
```yaml
---

- name: Kafka role execution play
  hosts: brokers
  become: true
  roles:
    - role: bilalcaliskan.kafka
      vars:
        install_kafka: true
        install_zookeeper: true
        seperate_zookeepers: false
```

#### Kafka + Zookeeper on the same server uninstallation

*inventory.ini:*
```
[brokers]
  brokernode01
  brokernode02
  brokernode03
```

*playbook.yaml:*
```yaml
---

- name: Kafka role execution play
  hosts: brokers
  become: true
  roles:
    - role: bilalcaliskan.kafka
      vars:
        install_kafka: false
        install_zookeeper: false
        seperate_zookeepers: false
```

### License

MIT / BSD
