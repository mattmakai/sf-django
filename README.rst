Deploy Django with Ansible
==========================

This project contains an example Ansible Playbook for deploying a Django
project. You should probably use 
`underwear <https://github.com/makaimc/underwear>`_ to deploy instead of
these files. These files are intended as examples for the 
`January 2014 San Fran Django meetup talk <http://www.meetup.com/The-San-Francisco-Django-Meetup-Group/events/151920512/>`_.
 

Background
----------
I build a lot of Django apps and I wanted a way to easily deploy them to
a traditional virtual private server stack instead of just defaulting to
Heroku. I previously wrote extensive Fabric scripts to automate the 
server configuration and deployment process, but that's really the wrong 
tool for this goal. Ansible is a much better way to solve the repeatable 
deployment problem.


First steps
-----------
Ansible runs over SSH, so we need a way to bootstrap SSH connections through
a non-root user. 

One way to automate these first few steps is with Fabric. The 
fabfile.py.template contains one public function, bootstrap_ansible. 
bootstrap_ansible calls the other functions to create a non-root user with 
sudo privileges, upload public keys for deployment, and lock down root from 
logging in.

Copy fabfile.py.template to fabfile.py, fill in the commented fields at
the top of the script, then run the script with::

  fab bootstrap_ansible

Right now the script will prompt you for the password the non-root user should
be created with. I'll automate that manual step away later.

Ansible-Vault
-------------
The database password sits in an ansible-vault `group_vars/db_pass.vault`. Since the password for the vault is not shared, create your own vault. Steps to do this:
 *  Make up a vault passphrase and put it into `./.vault_passphrase` (configured in ansible.cfg).
 *  Hit `ansible-vault create group_vars/dp_pass.vault` or any other name for the file, it does not matter.
 *  The file should contain only one variable pair: `db_password: CHANGE_ME`, with whatever password you choose (in yml).

 Ansible decrypts the vault when you provision the playbook and has access to the variable in the playbooks. And, you can safely commit everything, since the .vault_passphrase is in .gitignore.

