## Kafka Ansible Role

[![Build Status](https://travis-ci.org/bilalcaliskan/kafka-ansible-role.svg?branch=master)](https://travis-ci.org/bilalcaliskan/kafka-ansible-role)

Installs and configures Kafka cluster on RHEL/CentOS 7/8 servers.

## Requirements

This installation requires Zookeeper; also note that this role requires root access, so either run it in a playbook with a global `become: yes`, or invoke the role in your playbook. Here is the example with the global `become: yes`:

    - hosts: all
      become: yes
      roles:
        - role: kafka
          vars:
            empty_var: true

## Role Variables

See the default values in 'defaults/main.yml'. You can overwrite them in 'vars/main.yml' if neccessary.

## Dependencies

    That role requires zookeeper role

## Example Inventory File

    [all]
    broker01.example.com
    broker02.example.com
    broker03.example.com

## Example Playbook File

    - hosts: all
      become: yes
      roles:
        - role: kafka
          vars:
            empty_var: true

* Inside `vars/main.yml`*:

    kafka_version: 123.123

## License

MIT / BSD
