# Server

Ansible playbook making your server practically maintain itself 🛠

Features:

- [unattended package upgrades][unattended-upgrades] – relax, we got
  your security patches ⛱
- one-command OS distribution upgrades - 14.04 → 16.04 → … 🏗
- [Jupyter notebook][jupyter] (optional) – analyse all the things 📈

The playbook should generally work on Debian / Ubuntu servers, but it was mostly
tested on Ubuntu 16.04.


## Usage

Install [Ansible](http://docs.ansible.com/). An easy way if you have Python locally:

    pip install ansible

Prepare your *hosts* file by adding your servers
and prepare the *config.yml* file where you can optionally
change the global defaults:

    cp hosts.example hosts
    cp config.yml.example config.yml

Make sure:

 - you can ssh into the added servers using only your ssh
   public key (no password) first. You can push your public key to the server using:

        ssh-copy-id user@host

 - the user you ssh as into the servers is on the sudoers list

There are additional settings in *group_vars/servers* which can be overriden
for all the servers. Also, you can override settings for individual servers
if you create a file in the *host_vars* folder
(filename must match the server alias from the *hosts* file).

Test that you can access the servers:

    ansible -m ping all

**Finally to run the playbooks execute:**

    ansible-playbook server.yml

Or if you can't remember the command, run:

    ./server.sh


## Components

There are a number of playbook components that are not deployed or run
by default.

### Distribution upgrades

Since upgrading your OS distribution task can take quite some time, it is
available as a separate playbook that you run as:

    ansible-playbook upgrade.yml

Note that depending on what packages you have installed, you might have to
ssh to the server once the process completes to finish some manual steps
(mostly choosing what to do with config files that were edited).

### Jupyter notebook

To install the optional [Jupyter notebook][jupyter] either enable it in
*group_vars/servers* or create an individual server's config file in
*host_vars/server-alias* ( matching a server alias from your hosts file).
Example config:

    ---
    install_notebook: True

    notebook_password: 'sha1:d88452b18fd9:735e6a786d17709a131198f0c0edf217eadc55bf'

If you don't set the password (you should!) the default is *donthackme*.
Once you run the *server.yml* playbook,
Jupyter notebook should be running on port 8888.

You can use the default password to connect to the notebook and generate
a new password hash from within a notebook if you haven't already:

    In [1]: from notebook.auth import passwd

    In [2]: passwd()
    Enter password:
    Verify password:
    Out[2]: 'sha1:d88452b18fd9:735e6a786d17709a131198f0c0edf217eadc55bf'

You then set this hash in your host file and rerun the playbook.
More info on generating the password and optionally setting up a TLS certificate
[in the documentation][jupyter-security].

[unattended-upgrades]: https://github.com/debops/ansible-unattended_upgrades
[jupyter]: http://jupyter.org/
[jupyter-security]: http://jupyter-notebook.readthedocs.io/en/latest/public_server.html#securing-a-notebook-server
