## Kafka Ansible Role

[![CI](https://github.com/bilalcaliskan/kafka-ansible-role/workflows/CI/badge.svg?event=push)](https://github.com/bilalcaliskan/kafka-ansible-role/actions?query=workflow%3ACI)

Installs and configures Kafka cluster on RHEL/CentOS 7/8 servers.

### Requirements

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

> Please note that this role will ensure that `firewalld` systemd service on your servers are started and enabled by default. If you want to stop and disable `firewalld` service, please modify below variable as false when running playbook:  
> ```yaml  
> firewalld_enabled: false

### Dependencies

That role requires [bilalcaliskan.zookeeper](https://galaxy.ansible.com/bilalcaliskan/zookeeper) role

### Example Inventory File

```
[kafka]
broker01.example.com
broker02.example.com
broker03.example.com
```

### Example Playbook File For Installation

```yaml
- hosts: all
  become: true
  roles:
    - role: bilalcaliskan.kafka
      vars:
        install_kafka: true
        install_zookeeper: true
        kafka_version: 123.123
```

Inside [vars/main.yml](vars/main.yml)*:
```yaml
kafka_version: 123.123
```

### Example Playbook File For `Ununinstallation`

```yaml
- hosts: all
  become: true
  roles:
    - role: bilalcaliskan.kafka
      vars:
        install_kafka: false
        install_zookeeper: false
```

### License

MIT / BSD
