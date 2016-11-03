# docker base

This role installs baseline needs for machines running Docker, beyond the Docker Engine itself,
including:

 -  Requirements for running Ansible's `docker` module
 -  Utilities for garbage collecting unused Docker resources
 -  Customized configurations of the Docker Engine
 -  Installs docker-compose (optional)
 -  Pulls some common images that we anticipate using in many circumstances

## Role Variables

This role expects the following variables:

 - `docker_py_version` defines the version of the Docker Python library to install, may be
   "latest", default 1.9.0
 - `docker_rotate_version` defines the version of the Docker Rotate library to install, default 3.0
 - `docker_compose_version` defines the version of Docker Compose to install, may be "latest",
   default 1.8.0. To skip installation of docker_compose, specify `null` or `""`
 - `docker_base_extra_python_requirements`: defines a list of extra python requirements you may need
   in the event of a version conflict. Each item of the list is a extra entry in the generated requirements.txt
   Latest docker-py and docker-compose needed different versions of `requests` so we added this.
   See Also: `pip requirements sub dependency resolution`

   Example:

     docker_base_extra_python_requirements:
       - "requests==2.7.0"

 - `docker_rotate_images_arguments`: arguments to pass to "docker-rotate images" when cleanup cron
   job runs. If blank, job will not be run. Default is "--keep 3 --name ~busybox --tag ~latest".
   For documentation, see the
   [docker rotate github page](https://github.com/locationlabs/docker-rotate)
 - `docker_rotate_untagged_images`: if True, then untagged images will be periodically cleaned up.
   Default False
 - `docker_rotate_exited_containers`: if non-empty, exited containers at least this old will be
   removed when daily cleanup runs. Default 1h.
   `docker_rotate_created_containers`: if non-empty, created containers - that is, containers that
   have been created but never started - at least this old will be removed when daily cleanup runs.
   Default is empty.
 - `docker_images_to_pull`: list of Docker images; images with these names will be "docker pull"ed
   when the role is run. Default is empty.
 - `docker_registries_to_login`: list of maps, each map containing login credentials for a Docker
   registry. Expected keys are `username`, `password`, `email`, and `url`. The role will attempt to
   log into each registry described in the list. Default is empty.

Example for `docker_registries_to_login`:

    docker_registries_to_login:
      - username: foo
        password: foopwd
        email: foo@foo.com
        url: https://registry.foo.com

### Docker version notes
Note that certain versions of the Docker Engine and Docker Python library are incompatible. Note
also that the Ansible "docker" module has compatibility issues.

Specifically:
 - `docker-py` version 1.1.0 is the highest version that works with `docker` 1.5.0
 - `docker-py` version 1.2.3 is the highest version that works with the `docker` module in
   Ansible < 2.0.

## Dependencies
In order to run this role:
 - python and pip must be installed.
 - docker engine must be installed. For a role that installs docker engine, see
   [here](https://github.com/locationlabs/ansible-role_docker)

## pip requirements sub dependency resolution
There are times when we will need to be explicit about versions of sub dependencies
installed so that we ensure that the versions installed are compatible with both
docker_py and docker_compose as well as any other requirements. We do this since
pip doesn't have proper sub dependency resolution:
    https://github.com/pypa/pip/issues/988

We should update `docker_base_extra_python_requirements` as needed when changing
what we install with pip. Especially if we change default versions of
`docker_py_version` or `docker_compose_version` since the version of `requests`
compatible with both could change when this happens.