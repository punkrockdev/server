# Server

Ansible playbook making your server practically maintain itself 🛠

Features:

- unattended package upgrades

TODO:

- one-command dist upgrades
- Jupyter notebook setup

## Usage

Install [Ansible](http://docs.ansible.com/).

Prepare your hosts file (add your servers) and the config file (optionally
override defaults):

    cp hosts.example hosts
    cp config.yml.example config.yml

Test that you can access the servers

    ansible -m ping all

Finally to run the playbooks execute

    ansible-playbook server.yml
