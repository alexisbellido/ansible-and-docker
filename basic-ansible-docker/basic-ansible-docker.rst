Basic Ansible Setup with Docker
========================================

Create containers individually
--------------------------------------------------------------------------------

Create a network and a few containers on it so that they see each other.

.. code-block:: bash

  $ docker network create -d bridge ansible-network
  $ docker run -it -d --network=ansible-network --name ansible1 python:3.6.5-slim-stretch python
  $ docker run -it -d --network=ansible-network --name ansible2 python:3.6.5-slim-stretch python
  $ docker run -it -d --network=ansible-network --name ansible3 python:3.6.5-slim-stretch python

Connect to one of the containers, install ping, and ping other container.

.. code-block:: bash

  $ docker exec -it ansible1 /bin/bash
  $ apt-get update && apt-get install -y iputils-ping
  $ ping ansible2

TODO
========================

Move this to Docker compose or Kubenertes. 

Mount host directory so that Ansible YAML can be edited outside containers.

Merge docker-composer.yml and docker-compose-2.yml into one coherent compose file.

Update Dockerimage, requirements to include Python, ping and Ansible.