========================
Team and repository tags
========================

.. image:: http://governance.openstack.org/badges/openstack-ansible-os_adjutant.svg
    :target: http://governance.openstack.org/reference/tags/index.html

.. Change things from this point on

OpenStack-Ansible Adjutant
############################
:tags: openstack, adjutant, cloud, ansible
:category: \*nix

This Ansible role installs and configures OpenStack adjutant.

This role will install the following Upstart services:
    * adjutant-api
    * adjutant-processor

Adding The Service to Your OpenStack-Ansible Deployment
---------------------------------------------------------

To add a new service to your OpenStack-Ansible (OSA) deployment:

* Define ``registration_hosts`` in your ``conf.d`` or ``openstack_user_config.yml``.
  For example:

  .. code-block:: yaml

      registration_hosts:
        infra1:
          ip: 172.20.236.111
        infra2:
          ip: 172.20.236.112
        infra3:
          ip: 172.20.236.113


* Create respective LXC containers (skip this step for metal deployments):

  .. code-block:: console

     openstack-ansible openstack.osa.containers_lxc_create --limit adjutant_all,registration_hosts

* Run service deployment playbook:

  .. code-block:: console

     openstack-ansible openstack.osa.adjutant

For more information, please refer to the `OpenStack-Ansible project documentation <https://docs.openstack.org/project-deploy-guide/openstack-ansible/latest/>`_.

Always verify that the integration is successful and that the service behaves
correctly before using it in a production environment.

Required Variables
==================

.. code-block:: yaml

    adjutant_service_password
    adjutant_rabbitmq_password
    adjutant_galera_password
    adjutant_galera_address

Example Playbook
================

.. code-block:: yaml

    - name: Install adjutant server
      hosts: adjutant_all
      user: root
      roles:
        - { role: "os_adjutant", tags: [ "os-adjutant" ] }
      vars:
        external_lb_vip_address: 172.16.24.1
        internal_lb_vip_address: 192.168.0.1
        adjutant_galera_address: "{{ internal_lb_vip_address }}"
        adjutant_galera_password: "SuperSecretePassword1"
        adjutant_service_password: "SuperSecretePassword2"
        adjutant_rabbitmq_password: "SuperSecretePassword3"
