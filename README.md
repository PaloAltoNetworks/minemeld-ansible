# minemeld-ansible

Ansible playbook for installing MineMeld on a Linux instance directly from the git repos. This is useful for development or
installing MineMeld on Linux distributions where prebuilt MineMeld packages are not provided. 

## MineMeld version

By default this Ansible playbook installs MineMeld from the ``devel`` branch, that is the latest **unstable** version.

To install the latest stable release you can:
- or uncomment the ``minemeld_version`` and ``group_permissions`` variables in ``local.yml``
- or specify an extra var while launching the playbook, using ``ansible-playbook -K -e "minemeld_version=master" -i 127.0.0.1, local.yml``

## Install from forks

If you have forked minemeld-core, minemeld-webui or minemeld-node-prototypes repos and you want to use the forks for development,
you can edit the corresponding variables in ``local.yml`` file to specify the URL of a fork of the main MineMeld repos.

## Howto on Ubuntu 14.04

    $ sudo apt-get update
    $ sudo apt-get upgrade # optional
    $ sudo apt-get install -y gcc git python2.7-dev libffi-dev libssl-dev
    $ wget https://bootstrap.pypa.io/get-pip.py
    $ sudo -H python get-pip.py
    $ sudo -H pip install ansible
    $ git clone https://github.com/PaloAltoNetworks/minemeld-ansible.git
    $ cd minemeld-ansible
    $ ansible-playbook -K -i 127.0.0.1, local.yml
    $ usermod -a -G minemeld <your user> # add your user to minemeld group, useful for development

## Howto on Ubuntu 16.04

**Support for Ubuntu 16.04 is still experimental**

    $ sudo apt-get update
    $ sudo apt-get upgrade # optional
    $ sudo apt-get install -y gcc git python-minimal python2.7-dev libffi-dev libssl-dev
    $ wget https://bootstrap.pypa.io/get-pip.py
    $ sudo -H python get-pip.py
    $ sudo -H pip install ansible
    $ git clone https://github.com/PaloAltoNetworks/minemeld-ansible.git
    $ cd minemeld-ansible
    $ ansible-playbook -K -i 127.0.0.1, local.yml
    $ usermod -a -G minemeld <your user> # add your user to minemeld group, useful for development

## Howto on CentOS 7

**Support for CentOS 7 is still experimental**

    $ sudo yum install -y wget git gcc python-devel libffi-devel openssl-devel
    $ wget https://bootstrap.pypa.io/get-pip.py
    $ sudo -H python get-pip.py
    $ sudo -H pip install ansible
    $ git clone https://github.com/PaloAltoNetworks/minemeld-ansible.git
    $ cd minemeld-ansible
    $ ansible-playbook -K -i 127.0.0.1, local.yml
    $ usermod -a -G minemeld <your user> # add your user to minemeld group, useful for development

## Check if everything is running

Check if all the MineMeld services are up and running:

    $ sudo -u minemeld /opt/minemeld/engine/current/bin/supervisorctl -c /opt/minemeld/supervisor/config/supervisord.conf status
    minemeld-engine                  RUNNING   pid 37583, uptime 1:10:12
    minemeld-supervisord-listener    RUNNING   pid 37582, uptime 1:10:12
    minemeld-traced                  RUNNING   pid 37584, uptime 1:10:12
    minemeld-web                     RUNNING   pid 37585, uptime 1:10:12

## Porting to a new distribution

Distribution specific tasks and variables are isolated in specific files. These files are dynamically loaded by the Ansible playbook
based on the values of the ``ansible_distribution``, ``ansible_distribution_version`` and ``ansible_distribution_major_version``.

Check ``roles/infrastructure/vars`` and ``roles/minemeld/vars`` for examples.

### Order

- ``{{ ansible_distribution }}-{{ ansible_distribution_version }}.yml``
- ``{{ ansible_distribution }}-{{ ansible_distribution_major_version }}.yml``
- ``{{ ansible_distribution }}.yml``

Example: for Ubuntu 14.04 the playbook will look for Ubuntu-14.04.yml, then for Ubuntu-14.yml and then for Ubuntu.yml
