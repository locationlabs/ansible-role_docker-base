# Release Change Log
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
