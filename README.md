docker base
===========

This role installs baseline needs for machines running Docker, beyond the Docker Engine itself,
including:

 -  Requirements for running Ansible's `docker` module
 -  Requirements for running Docker containers via `upstart` (optional)
 -  Utilities for garbage collecting unused Docker resources
 -  Customized configurations of the Docker Engine
 -  Installs docker-compose
 -  Pulls some common images that we anticipate using in many circumstances

Role Variables
--------------

This role expects the following variables:

 -  `docker_log_rotate_interval` controls the frequency of Docker log rotation
 -  `docker_log_rotate_count` controls the number of Docker logs to retain
 -  `docker_uses_upstart` defines whether `upstart` support will be installed
 -  `docker_py_version` defines the version of the Docker Python library being used
 -  `docker_compose_version` defines the version of docker-compose to install (defaults to newest)

Note that certain versions of the Docker Engine and Docker Python library are incompatible.


Dependencies
------------

Depends on `docker` and `python`
