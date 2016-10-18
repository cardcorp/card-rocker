## Start with the official rocker image (lightweight Debian)
FROM rocker/r-base:latest

MAINTAINER Gergely Daroczi <gergely.daroczi@card.com>

## Install SSH client
RUN apt-get update \
  && apt-get install -y --no-install-recommends openssh-client \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/

## Install bindfs to be able to mount SSH keys from the host with the right permissions
## NOTE: you stil need to "mount -a" as root when starting the container (no autostart)
RUN apt-get update \
  && apt-get install -y --no-install-recommends bindfs kmod \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/ \
  && mkdir /home/docker/.ssh \
  && mkdir /home/docker/.ssh-external \
  && echo "bindfs#/home/docker/.ssh-external /home/docker/.ssh fuse force-user=docker,force-group=docker,perms=0700 0 0" >> /etc/fstab
RUN chown -R docker:docker /home/docker/.ssh-external
