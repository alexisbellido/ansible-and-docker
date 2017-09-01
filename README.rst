Ansible and Docker
========================================

Some tests with Ansible and Docker

Docker setup
----------------------------------------

.. code-block:: bash

  $ docker run -it -w /root -v "$PWD":/root --name python-test-1 --hostname python-test-1 python:3.6.1-slim
