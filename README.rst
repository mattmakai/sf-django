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



