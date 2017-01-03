# Server

Ansible playbook making your server practically maintain itself 🛠

Features:

- unattended package upgrades – relax, we got your security patches ⛱
- [Jupyter notebook][jupyter] (optional) – analyse all the things 📈

TODO:

- one-command distribution upgrades

## Usage

Install [Ansible](http://docs.ansible.com/).

Prepare your hosts file (add your servers) and the config file (optionally
change these global defaults):

    cp hosts.example hosts
    cp config.yml.example config.yml

Make sure you can ssh into the added servers using only your ssh
public key (no password) first - `ssh-copy-id user@host` otherwise.

There are additional settings in *group_vars/servers* which can be overriden.

Test that you can access the servers

    ansible -m ping all

Finally to run the playbooks execute

    ansible-playbook server.yml

[jupyter]: http://jupyter.org/
