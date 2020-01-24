openshift-ipsec
===============

An Ansible role for configuring ipsec to secure communications between nodes of an OpenShift cluster.
Role supports Opportunistic & Non-Opportunistic setups.

Forked from: https://github.com/jkupferer/ansible-role-openshift-ipsec

Installation
------------

Clone repo and run from your own playbook

Requirements
------------

Openshift cluster >= 3.11 (tested only on that version)
Inventory file with Openshift nodes `[nodes]`, can re-use openshift install file.

IPSec (libreswan based) config allowed (values to be used with `ipsec_configuration` var):
* `opportunistic`
* `nonopportunistic`

This role is read to be used with `firewalld` (not tested) and `iptables` (tested).
Variable `os_firewall_use_firewalld`, if loaded openshift install file, would be replaced by default value established at clusters installation time. Otherwise will run `iptables` config.

The this role runs a task for `firewalld` as this should be the default with OCP >= 3.7, otherwise the below IPtables
example should be used.


Role Variables
--------------

| Variable                     | Description                                      | Defaults         |
|------------------------------|--------------------------------------------------|------------------|
| ipsec_ca_dir                 | Directory to store ipsec CA on masters           | /etc/ipsec-ca    |
| ipsec_ca_countryName         | Default country name value for certificates      | *Optional*       |
| ipsec_ca_stateOrProvinceName | Default state or province value for certificates | *Optional*       |
| ipsec_ca_localityName        | Default locality value for certificates          | *Optional*       |
| ipsec_ca_organizationName    | Default organization value for certificates      | *Optional*       |
| ipsec_ca_emailAddress        | Default email address value for certificates     | *Optional*       |
| ipsec_ca_key                 | CA key if one should not be auto-generated       | *Optional*       |
| ipsec_ca_cert                | CA cert if one should not be auto-generated      | *Optional*       |
| ipsec_configuration          | How IPSec should be configured                   | nonopportunistic |
| os_firewall_use_firewalld    | Openshift cluster configured with firewalld?     | false            |

Example Playbook
----------------

~~~yaml
    - name: Play configure IPSEC on Openshift nodes
      hosts: nodes
      become: true
      gather_facts: true

      vars:
        ipsec_configuration: nonopportunistic

      roles:
        - openshift-ipsec
~~~

**Note**:
This role also uses [Ansible tags](http://docs.ansible.com/ansible/playbooks_tags.html):
* Run your playbook with the `--list-tags` flag for more information.
* Run your playbook with the `--list-tasks` flag for more information.

License
-------

BSD

Author Information
------------------

Joao Santos (morgado.santos@gmail.com)
Kevin Chung (kevin.chung@redhat.com)
Johnathan Kupferer (jkupfere@redhat.com)
Freddy Montero (fmontero@redhat.com)
