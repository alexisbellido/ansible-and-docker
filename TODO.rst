TODO
================================================================================

- Do something about the missing znbcache that doesn't leave containers start in background. How can I make it optional or just remove it?

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
