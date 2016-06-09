# minemeld-ansible

Ansible playbook for minemeld development

## Requirements

Ubuntu 14.04.03 LTS Server

## Howto

    sudo apt-get update
    sudo apt-get install gcc git python2.7-dev libffi-dev libssl-dev
    wget https://bootstrap.pypa.io/get-pip.py
    sudo -H python get-pip.py
    sudo -H pip install ansible
    git config --global credential.helper 'cache --timeout=1800'
    git ls-remote https://github.com/PaloAltoNetworks/minemeld-ansible.git
    ansible-pull -K -C develop -i localhost, -e "i=devel" -U https://github.com/PaloAltoNetworks/minemeld-ansible.git -v -d minemeld-ansible
    
At the end of the installation you should be able to access http://(VMIP)/feeds/inboundfeed and http://(VMIP)/feeds/outboundfeed with your browser.

