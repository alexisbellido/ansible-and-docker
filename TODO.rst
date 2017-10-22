TODO
================================================================================

Do something about the missing znbcache that doesn't leave containers start in background. How can I make it optional or just remove it?

Maybe I need to find a way to tell Docker how to handle different Python packages installed via pip, from PiPy, from Git repository, or from source directory in the file system. Or maybe just use Ansible to download (git clone or other method) the packages to directories on the host and then map those to the containers and install from there.

As I'm forwarding the host's ssh agent to the container, I can git clone from there.

.. code-block:: bash

  $ docker-compose up
  Creating network "ansibleanddocker_default" with the default driver
  Creating pytest-1
  Creating pytest-4
  Creating pytest-2
  Creating pytest-3
  Attaching to pytest-1, pytest-2, pytest-3, pytest-4
  pytest-1    | /usr/local/bin/docker-entrypoint.sh: line 30: cd: /root/project: No such file or directory
  pytest-1    | Traceback (most recent call last):
  pytest-1    |   File "<string>", line 1, in <module>
  pytest-1    | ModuleNotFoundError: No module named 'znbcache'

==

logs

.. code-block:: bash

  $ docker-compose up
  Starting pytest-1 ... 
  Starting pytest-1 ... done
  Attaching to pytest-1
  pytest-1    | /usr/local/bin/docker-entrypoint.sh: line 30: cd: /root/project: No such file or directory
  pytest-1    | Traceback (most recent call last):
  pytest-1    |   File "<string>", line 1, in <module>
  pytest-1    | ModuleNotFoundError: No module named 'znbcache'
  pytest-1    | /root/django-apps/django-zinibu-cache/ should either be a path to a local project or a VCS url beginning with svn+, git+, hg+, or bzr+
  pytest-1 exited with code 0

==

