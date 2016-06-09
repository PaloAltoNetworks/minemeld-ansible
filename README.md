# minemeld-ansible

Ansible playbook for minemeld development

## Requirements

Ubuntu 14.04 LTS Server

## Howto

    $ sudo apt-get update
    $ sudo apt-get upgrade (optional)
    $ sudo apt-get install gcc git python2.7-dev libffi-dev libssl-dev
    $ wget https://bootstrap.pypa.io/get-pip.py
    $ sudo -H python get-pip.py
    $ sudo -H pip install ansible
    $ sudo su -
    # git config --global credential.helper 'cache --timeout=1800'
    # git clone -b develop https://github.com/PaloAltoNetworks/minemeld-ansible.git
    # cd minemeld-ansible
    # ansible-playbook -i localhost, -e "i=devel" local.yml
    # usermod -a -G minemeld <your user>
    # exit

At the end of the installation you should be able to access http://(VMIP)/feeds/inboundfeed and http://(VMIP)/feeds/outboundfeed with your browser.
