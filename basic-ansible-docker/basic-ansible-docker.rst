Basic Ansible Setup with Docker
========================================

Create containers individually
--------------------------------------------------------------------------------

Create a network and a few containers on it so that they see each other.

.. code-block:: bash

  $ docker network create -d bridge ansible-network
  $ docker run -it -d --network=ansible-network --mount type=bind,source="$(pwd)",target=/root/ansible -w=/root/ansible --name ansible1 python:3.6.5-slim-stretch python
  $ docker run -it -d --network=ansible-network --mount type=bind,source="$(pwd)",target=/root/ansible -w=/root/ansible --name ansible2 python:3.6.5-slim-stretch python
  $ docker run -it -d --network=ansible-network --mount type=bind,source="$(pwd)",target=/root/ansible -w=/root/ansible --name ansible3 python:3.6.5-slim-stretch python

Install ping and ping other containers just to confirm the containers can see each other.

.. code-block:: bash

  $ docker exec ansible1 apt-get update
  $ docker exec ansible1 apt-get install -y iputils-ping
  $ docker exec -it ansible1 apt-get install -y python-apt
  $ docker exec -it ansible1 /bin/bash
  $ ping ansible2

Installing python-apt is necessary for Debian to avoid this error. 

.. code-block:: bash

  "module_stderr": "/bin/sh: 1: /usr/bin/python: not found\n",

Now you can run Ansible, after installing it via pip on the host and creating an inventory file.

.. code-block:: bash

$ ansible -i inventory.yaml all -m ping

Note the all group.

Run some commands on targets. Note -a allows arguments for the default command module (-m).

.. code-block:: bash

  $ ansible -i inventory.yaml all -a "date"
  $ ansible -i inventory.yaml all -a "cat /etc/hosts"

Targeting hosts from ad-hoc command. Note the comma at the end of the list in quotes and the all group.

.. code-block:: bash

  $ ansible -i "zinibu.local," all -m ping
  zinibu.local | SUCCESS => {
      "changed": false, 
      "ping": "pong"
  }

And now with containers.

.. code-block:: bash

  $ ansible -i "ansible1,ansible2" --connection=docker all -m ping
  ansible2 | SUCCESS => {
      "changed": false, 
      "ping": "pong"
  }
  ansible1 | SUCCESS => {
      "changed": false, 
      "ping": "pong"
  }

TODO
========================

Move this to Docker compose or Kubenertes. 

Mount host directory so that Ansible YAML can be edited outside containers.

Merge docker-composer.yml and docker-compose-2.yml into one coherent compose file.

Update Dockerimage, requirements to include Python, ping and Ansible.