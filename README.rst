Ansible and Docker
========================================

Some tests with Ansible and Docker.

See `Dockerize Django project <https://github.com/alexisbellido/dockerize-django/>`_ for more details.

Docker setup
----------------------------------------

Launch the containers with Docker Compose.

.. code-block:: bash

    $ docker-compose up

This will leave Docker Compose in the foreground and the containers will stop as soon as you exit. To start all containers running in the background use -d for "detached" mode and then start the containers.

.. code-block:: bash

    $ docker-compose up -d
    $ docker-compose start


Then you can get into bash on one of the containers, pytest-3 in this example.

.. code-block:: bash

  $ docker exec -it pytest-3 /bin/bash

And because ``docker-compose.yml`` is set to forward the host's ssh agent you can use it to connect from the container as if you were on the host.

.. code-block:: bash

  root@pytest-3:~# ssh -T git@github.com
  
Extra notes
----------------------------------------

To run without Docker Compose, and for better organization, create a bridge network for your containers on your host.

.. code-block:: bash

  $ docker network create -d bridge project-network

If you want to run some tests in the container, you can pass a parameter not considered by the entrypoint script, like /bin/bash and you will get to a Bash command line. Note the ``-it`` option to run an interactive process in the foreground. This is useful to test Python packages.

.. code-block:: bash

  $ docker run -it --network=project-network -w /root --hostname=pytest-1 --name=pytest-1 alexisbellido/django:1.11 /bin/bash

Because it's running in the foreground, if you exit this container it will stop. Start it and ssh into it again running:

.. code-block:: bash

  $ docker start pytest-1
  $ docker exec -it pytest-1 /bin/bash

This container will have the Python virtual environment of the project activated by default and you can create a new virtual environment with:

.. code-block:: bash
  
  $ /usr/local/bin/python3.6 -m venv /root/.venv/my-project

and activate it with:

.. code-block:: bash

    $ source /root/.venv/my-project/bin/activate

You can deactivate a Python virtual environment running:

.. code-block:: bash

    $ deactivate
    
Note that deactivate is created when sourcing the activate script so it may not be available from the shell when you first ssh into the container. Read more about `venv <https://docs.python.org/3/library/venv.html>`_.
    
To bypass the entrypoint script, use ``--entrypoint``. This also uses ``-it`` and adds ``--rm`` to remove the container automatically after it stops.

.. code-block:: bash

  $ docker run -it --rm --network=project-network -w /root -v ~/.ssh/id_rsa:/root/.ssh/id_rsa -v $SSH_AUTH_SOCK:/run/ssh_agent -e SSH_AUTH_SOCK=/run/ssh_agent -v "$PWD"/django-project:/root/django-project -v "$PWD"/django-apps:/root/django-apps --env PROJECT_NAME=django-project --env SETTINGS_MODULE=locals3 --env POSTGRES_USER=user1 --env POSTGRES_PASSWORD=user_secret --env POSTGRES_DB=db1 --env POSTGRES_HOST=db1 -p 33332:8000 --hostname=app1-dev --name=app1-dev --entrypoint /bin/bash alexisbellido/django:1.11

This goes directly to bash but exits afterwards. -i keeps STDIN open even if not attached and -t allocates a pseudo-tty.

.. code-block:: bash

  $ docker run -it --network=zinibu -w /root -v /Users/alexis/Projects/zinibu/django-project:/root/zinibu -v /Users/alexis/Projects/zinibu/django-apps:/root/django-apps --env PROJECT_NAME=zinibu -p 50001:8000 --hostname=pytest-1 --name=pytest-1 alexisbellido/django:1.11 /bin/bash

This uses detached mode (-d) to keep one service in the container running. Note the *development* command for the entrypoint passed at the end.

.. code-block:: bash
  
  $ docker run -d --network=zinibu -w /root -v /Users/alexis/Projects/zinibu/django-project:/root/zinibu -v /Users/alexis/Projects/zinibu/django-apps:/root/django-apps --env PROJECT_NAME=zinibu -p 50001:8000 --hostname=pytest-1 --name=pytest-1 alexisbellido/django:1.11 development
