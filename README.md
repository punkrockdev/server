# Server

Ansible playbook making your server practically maintain itself 🛠

Features:

- [unattended package upgrades][unattended-upgrades] – relax, we got
  your security patches ⛱
- one-command OS distribution upgrades - 14.04 → 16.04 → … 🏗
- [Jupyter notebook][jupyter] (optional) – analyse all the things 📈


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


## Components

There are a number of playbook components that are not deployed or run
by default.

### Distribution upgrades

Since upgrading your OS distribution task can take quite some time, it is
available as a separate playbook that you run as:

    ansible-playbook upgrade.yml

### Jupyter notebook

To install the optional [Jupyter notebook][jupyter] either enable it in
*group_vars/servers* or create an individual server's config file in
*host_vars/server-alias* ( matching a server alias from your hosts file).
Example config:

    ---
    install_notebook: True

    notebook_password: sha1:67c9e60bb8b6:9ffede0825894254b2e042ea597d771089e11aed

More info on generating the password and setting up a TLS certificate
[in the documentation][jupyter-security].

[unattended-upgrades]: https://github.com/debops/ansible-unattended_upgrades
[jupyter]: http://jupyter.org/
[jupyter-security]: http://jupyter-notebook.readthedocs.io/en/latest/public_server.html#securing-a-notebook-server
