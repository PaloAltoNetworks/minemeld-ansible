# minemeld-ansible

Ansible playbook for minemeld development

## Requirements

Ubuntu 14.04.03 LTS Server

## Howto

    sudo apt-get install gcc git python2.7-dev
    wget https://bootstrap.pypa.io/get-pip.py
    sudo python get-pip.py
    sudo pip install ansible
    git config --global credential.helper 'cache --timeout=300'
    git ls-remote https://github.com/PaloAltoNetworks-BD/minemeld-ansible.git
    ansible-pull -K -C develop -i localhost, -U https://github.com/PaloAltoNetworks-BD/minemeld-ansible.git -v -d minemeld-ansible
    
At the end of the installation you should be able to access http://<VM iP>/feeds/inboundfeed and http://<VM IP>/feeds/outboundfeed with your browser.

