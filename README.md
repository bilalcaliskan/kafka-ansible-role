# Kafka Ansible Role
[![CI](https://github.com/bilalcaliskan/kafka-ansible-role/workflows/CI/badge.svg?event=push)](https://github.com/bilalcaliskan/kafka-ansible-role/actions?query=workflow%3ACI)
[![GitHub tag](https://img.shields.io/github/tag/bilalcaliskan/kafka-ansible-role.svg)](https://GitHub.com/bilalcaliskan/kafka-ansible-role/tags/)

Installs and configures [Apache Kafka](https://kafka.apache.org/) on Redhat/Debian based hosts.

By default, this role sets up [Apache Zookeeper](https://zookeeper.apache.org/) and [Apache Kafka](https://kafka.apache.org/) on the same server. But you can change this behavior by setting below variables while running the playbook. Check the **Examples** section for ready to use examples.

```
seperate_zookeepers: true
zookeepers:
  - zookeepernode01
```

## Requirements
This role has below requirements:
- [Python 3.x](https://www.python.org/downloads/)
- [Ansible](https://docs.ansible.com/) (min 2.4, suggested 2.9.16)

You can install suggested version with pip3:
```
$ pip3 install "ansible==2.9.16"
```

Note that this role requires root access, so either run it in a playbook with a global `become: true`, or invoke the role in your playbook.

## Role Variables

See the default values in [defaults/main.yml](defaults/main.yml). You can overwrite them in [vars/main.yml](vars/main.yml) if neccessary or you can set them while running playbook.

Also please see the Redhat ansible_os_family specific variables in [vars/redhat.yml](vars/redhat.yml) and Debian ansible_os_family specific variables in [vars/debian.yml](vars/debian.yml).

> Please note that this role can ensure that `firewalld` systemd service on your servers are started and enabled by default. If you want to start and enable `firewalld` service, please modify below variable as true while running playbook:
> ```yaml
> firewalld_enabled: true
> ```

## Dependencies

That role requires [bilalcaliskan.zookeeper](https://galaxy.ansible.com/bilalcaliskan/zookeeper) role


## Examples
### Seperate Zookeeper, Seperate Kafka Installation

***inventory.ini:***
```
[zookeepers]
  zookeepernode01

[brokers]
  brokernode02
```

***playbook.yaml:***
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

### Seperate Zookeeper, Seperate Kafka Uninstallation
***inventory.ini:***
```
[zookeepers]
  zookeepernode01

[brokers]
  brokernode02
```

***playbook.yaml:***
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

### Kafka + Zookeeper on the same server installation

***inventory.ini:***
```
[brokers]
  brokernode01
  brokernode02
  brokernode03
```

***playbook.yaml:***
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

### Kafka + Zookeeper on the same server uninstallation

***inventory.ini:***
```
[brokers]
  brokernode01
  brokernode02
  brokernode03
```

***playbook.yaml:***
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

## Development
This project requires below tools for development:
- [Python 3.x](https://www.python.org/downloads/)
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) - (min 2.4, suggested 2.9.16)
- [pre-commit](https://pre-commit.com/)
- [ansible-lint](https://ansible-lint.readthedocs.io/en/latest/installing.html#using-pip-or-pipx) - required by [pre-commit](https://pre-commit.com/)
- [Bash shell](https://www.gnu.org/software/bash/) - required by [pre-commit](https://pre-commit.com/)

After you install all the tools above, you can simply configure [pre-commit](https://pre-commit.com/) by typing:
```shell
$ pre-commit install
```
## License
Apache License 2.0
