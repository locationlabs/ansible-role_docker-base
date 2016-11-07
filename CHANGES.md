# Release Change Log

Version 4.0:
 - dropped support for "docker_opts", "docker_log_rotate_max_size", and "docker_log_rotate_count"
   variables. Changing docker default configuration is no longer the responsibility of this
   role. (It's intended that this role be used with ansible-role_docker v2.2+, which now
   supports passing options to Docker on startup.)
 - see "Migration Notes" in README.md

Version 3.0.2:
 - add variable to define extra python dependencies in case of sub dependency conflicts

Version 3.0.1:
 - bug fixes for login and pull behavior; fixed error in pull conditional check, and now
   perform login *before* attempting to pull containers.

Version 3.0:
 - use version 3.0+ of docker rotate
 - install docker-py, docker-compose, and dockerrotate in a single pip command to avoid
   them stepping on each other version-wise
 - log into Docker registries according to config variable.
 - pre-pull packages according to config variable, not hardcoded list.
 - do log rotation by size, not by time; use Docker's build-in rotation for Docker version 1.8+
 - no longer install inotify-tools

Version 2.3:
 - Update 'clear-zombie' script

Version 2.2:
 - restart docker with new config before prefetching images

Version 2.1:
 - install docker-compose
 - pre-pull busybox and ubuntu-debootstrap images

Version 2.0:
 - port to github

Version 1.1:
 - Configure `/etc/default/docker`
 - Add docker restart handler

Version 1.0:
 - Initial release
