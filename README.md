# card-rocker

Docker images built on the top of [rocker/r-base](https://registry.hub.docker.com/u/rocker/r-base/) at [CARD.com](http://card.com):

* `r-java`: minimal Debian-based R container with Java 8 and `rJava` package
* `r-pandoc`: minimal Debian-based R container with `pandoc` 1.13.1, plus `knitr` and `pander` packages
* `r-ssh`: minimal Debian-based R container with SSH client and some workarounds to be able to mount SSH keys from the host

For more details, see the README files in the related sub-directories.

Automated builds are available at [Docker Hub](https://registry.hub.docker.com/u/cardcorp/).
