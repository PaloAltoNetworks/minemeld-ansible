# WARNING!

This will install from the ``develop`` branch; if you want to install
from another branch or forked repositories see **MineMeld version**
and **Install from forks** below.

# minemeld-ansible

``minemeld-ansible`` is an Ansible playbook for installing MineMeld on
a Linux instance directly from the git repositories. This is useful for
development, or installing MineMeld on Linux distributions where
pre-built MineMeld packages are not provided.

## MineMeld version

By default the Ansible playbook installs MineMeld from the ``develop``
branch; this is the latest **unstable** version.

To install the latest stable release you can do one of the following
steps:

- uncomment the ``minemeld_version`` and ``group_permissions``
  variables in ``local.yml``

- specify an extra var while launching the playbook, using:

    ``$ ansible-playbook -K -e "minemeld_version=master" -i 127.0.0.1, local.yml``

## Install from forks

If you have forked the ``minemeld-core``, ``minemeld-webui`` or
``minemeld-node-prototypes`` repos and you want to use the forks for
development, you can edit the corresponding variables in the ``local.yml``
file to specify the URLs of the forks of the main MineMeld repos.

## Disable Send-a-suggestion check

Starting with MineMeld 0.9.43b1, MineMeld instances use an API call
over HTTPS to check if they can reach the Send-A-Suggestion API
endpoint.  The API sends the UUID4 of the MineMeld installation and
the running version of MineMeld. If you want to disable it just add
this line in the file ``roles/minemeld/templates/20-local.yml.j2``:

    SNS_ENABLED: false

## Howto on Ubuntu 16.04

    $ sudo apt-get update
    $ sudo apt-get upgrade
    $ sudo apt-get install -y gcc git python-minimal python2.7-dev libffi-dev libssl-dev make
    $ wget https://bootstrap.pypa.io/get-pip.py
    $ sudo -H python get-pip.py
    $ sudo -H pip install ansible
    $ git clone https://github.com/PaloAltoNetworks/minemeld-ansible.git
    $ cd minemeld-ansible
    $ ansible-playbook -K -i 127.0.0.1, local.yml
    $ sudo usermod -a -G minemeld <your user> # add your user to minemeld group, useful for development

## Howto on Ubuntu 18.04

**Support for Ubuntu 18.04 is still experimental**

    $ sudo apt update
    $ sudo apt upgrade
    $ sudo apt install -y gcc git python-minimal python2.7-dev libffi-dev libssl-dev make
    $ wget https://bootstrap.pypa.io/get-pip.py
    $ sudo -H python get-pip.py
    $ sudo -H pip install ansible
    $ git clone https://github.com/PaloAltoNetworks/minemeld-ansible.git
    $ cd minemeld-ansible
    $ ansible-playbook -K -i 127.0.0.1, local.yml
    $ sudo usermod -a -G minemeld <your user> # add your user to minemeld group, useful for development

## Howto on CentOS 7/RHEL 7

**Support for CentOS 7 and RHEL 7 is still experimental**

    $ sudo yum install -y wget git gcc python-devel libffi-devel openssl-devel zlib-dev sqlite-devel bzip2-devel
    $ wget https://bootstrap.pypa.io/get-pip.py
    $ sudo -H python get-pip.py
    $ sudo -H pip install ansible
    $ git clone https://github.com/PaloAltoNetworks/minemeld-ansible.git
    $ cd minemeld-ansible
    $ ansible-playbook -K -i 127.0.0.1, local.yml
    $ sudo usermod -a -G minemeld <your user> # add your user to minemeld group, useful for development
    
## Howto on Debian 9 (Stretch)

**Support for Debian 9 is still experimental**

    $ sudo apt-get update
    $ sudo apt-get upgrade # optional
    $ sudo apt-get install -y gcc git python2.7-dev libffi-dev libssl-dev
    $ wget https://bootstrap.pypa.io/get-pip.py
    $ sudo -H python get-pip.py
    $ sudo -H pip install ansible
    $ git clone https://github.com/PaloAltoNetworks/minemeld-ansible.git
    $ cd minemeld-ansible
    $ ansible-playbook -K -i 127.0.0.1, local.yml
    $ sudo usermod -a -G minemeld <your user> # add your user to minemeld group, useful for development

## Check if everything is running

Check if all the MineMeld services are up and running:

    $ sudo -u minemeld /opt/minemeld/engine/current/bin/supervisorctl -c /opt/minemeld/supervisor/config/supervisord.conf status
    minemeld-engine                  RUNNING   pid 37583, uptime 1:10:12
    minemeld-supervisord-listener    RUNNING   pid 37582, uptime 1:10:12
    minemeld-traced                  RUNNING   pid 37584, uptime 1:10:12
    minemeld-web                     RUNNING   pid 37585, uptime 1:10:12

## Upgrade MineMeld

``minemeld-ansible`` can be used to upgrade MineMeld.  The steps are:

1. Stop MineMeld
1. Remove the following directories:
   - ``/opt/minemeld/engine``
   - ``/opt/minemeld/prototypes``
   - ``/opt/minemeld/www``
1. Update ``minemeld-ansible``
1. Run the Ansible playbook
1. Verify MineMeld status

For example:

    $ sudo service minemeld stop
    $ cd /opt/minemeld/
    $ rm -rf engine prototypes www

    Then change to the directory where you performed the "git clone" above.

    $ cd minemeld-ansible
    $ git pull
    $ ansible-playbook -K -i 127.0.0.1, local.yml
    $ sudo -u minemeld /opt/minemeld/engine/current/bin/supervisorctl -c /opt/minemeld/supervisor/config/supervisord.conf status

## Porting to a new distribution

Distribution specific tasks and variables are isolated in specific
files. These files are dynamically loaded by the Ansible playbook
based on the values of ``ansible_distribution``,
``ansible_distribution_version`` and
``ansible_distribution_major_version``.

Check ``roles/infrastructure/vars`` and ``roles/minemeld/vars`` for
examples.

### Order

1. ``{{ ansible_distribution }}-{{ ansible_distribution_version }}.yml``
1. ``{{ ansible_distribution }}-{{ ansible_distribution_major_version }}.yml``
1. ``{{ ansible_distribution }}.yml``

Example: On Ubuntu 16.04 the playbook will look for:

1. ``Ubuntu-16.04.yml`` then
1. ``Ubuntu-16.yml`` then
1. ``Ubuntu.yml``
