# r-ssh

Dockerfile to install SSH client and [bindfs](http://bindfs.org/) on the top of [rocker/r-base](https://registry.hub.docker.com/u/rocker/r-base/), resulting in a minimal R environment to be able to SSH remote machines with the private keys mounted from the host.

This approach resolves the following two issues:

* you might want to avoid adding the SSH keys (sensitive information) to the Docker image
* mounting the SSH keys from the host results in file permissions not allowing you to actually use those.

This image does the following things to overcome these issues:

* install an SSH client
* install `bindfs` to be able to remount directories and files with updated permissions
* creates an `.ssh` and `.ssh-external` directories in the `docker` home folder
* adds a record in `/etc/fstab` to mount `/home/docker/.ssh-external` to the main SSH key folder with the right permissions

Please note that, unfortunately, it's not trivial to use this setup. You have to:

1. Mount the SSH keys from the host to the `/home/docker/.ssh-external` folder, e.g.:

```sh
docker run \
  -v '/home/username/.ssh/id_rsa:/home/docker/.ssh-external/id_rsa:ro' \
  --rm -ti cardcorp/r-ssh
```

2. And as there's no running daemon to auto-mount, you have to do this manually as root:

```sh
mount -o nonempty -a
```

Alternatively, you can start R as `docker` after re-mounting the keys by:

```sh
docker run \
  -v '/home/username/.ssh/id_rsa:/home/docker/.ssh-external/id_rsa:ro' \
  --rm -ti cardcorp/r-ssh \
  sh -c "mount -o nonempty -a;su docker -c \"R --vanilla\""
```
