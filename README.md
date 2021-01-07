## Kafka Ansible Role

[![Build Status](https://travis-ci.org/bilalcaliskan/kafka-ansible-role.svg?branch=master)](https://travis-ci.org/bilalcaliskan/kafka-ansible-role)

Installs and configures Kafka cluster on RHEL/CentOS 7/8 servers.

### Requirements

This installation requires Zookeeper; also note that this role requires root access, so either run it in a playbook with a global `become: yes`, or invoke the role in your playbook. Here is the example with the global `become: yes`:

    - hosts: all
      become: yes
      roles:
        - role: bilalcaliskan.kafka
          vars:
            empty_var: true

### Role Variables

See the default values in [defaults/main.yml](defaults/main.yml). You can overwrite them in [vars/main.yml](vars/main.yml) if neccessary.

> Please note that this role will ensure that `firewalld` systemd service on your servers are started and enabled. If your `firewalld` services are stopped and disabled, please modify below variable as false when running playbook:  
> start_firewalld: false

### Dependencies

    That role requires zookeeper role

### Example Inventory File

    [all]
    broker01.example.com
    broker02.example.com
    broker03.example.com

### Example Playbook File For Installation

    - hosts: all
      become: yes
      roles:
        - role: bilalcaliskan.kafka
          vars:
            install_kafka: true
            install_zookeeper: true
            kafka_version: 123.123

* Inside [vars/main.yml](vars/main.yml)*:

    kafka_version: 123.123

### Example Playbook File For `Ununinstallation`

    - hosts: all
      become: yes
      roles:
        - role: bilalcaliskan.kafka
          vars:
            install_kafka: false
            install_zookeeper: false

### License

MIT / BSD
