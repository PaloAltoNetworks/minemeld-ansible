# minemeld-ansible

Ansible playbook for minemeld-core development.

This playbook does not install MineMeld WebUI, see https://github.com/PaloAltoNetworks/minemeld-webui for instructions on how to set up a development environment for MineMeld WebUI.

## Requirements

Ubuntu 14.04 LTS Server

## Howto

    $ sudo su -
    # apt-get update
    # apt-get upgrade (optional)
    # apt-get install gcc git python2.7-dev libffi-dev libssl-dev
    # wget https://bootstrap.pypa.io/get-pip.py
    # python get-pip.py
    # pip install ansible
    # git config --global credential.helper 'cache --timeout=1800'
    # git clone https://github.com/PaloAltoNetworks/minemeld-ansible.git
    # cd minemeld-ansible
    # ansible-playbook -i localhost, local.yml
    # usermod -a -G minemeld <your user>
    # exit

At the end of the procedure you should be able to start MineMeld service:

    $ sudo service minemeld start
    $ sudo -u minemeld /opt/minemeld/engine/current/bin/supervisorctl -c /opt/minemeld/local/supervisor/config/supervisord.conf status
    minemeld-engine                  RUNNING   pid 60201, uptime 0:00:31
    minemeld-traced                  RUNNING   pid 60202, uptime 0:00:31
    minemeld-web                     RUNNING   pid 60203, uptime 0:00:31

### Cloning forks

You can edit the variables inside the ``local.yml`` file to specify the URL of a fork of the main MineMeld repos.

## Where from here

At the end of the installations a clone of the `develop` branch of:

* `minemeld-core` repo can be found in `/opt/minemeld/engine/current`
* `minemeld-node-prototypes` repo can be found in `/opt/minemeld/prototypes/current`
