# Docker Cheatsheet

Adapted from:

* https://stackoverflow.com/questions/32723111/how-to-remove-old-and-unused-docker-images

## Prune

Use `docker system prune` to delete all dangling data (containers, networks, and images).

You can remove all unused volumes with the --volumes option and remove all unused images (not just dangling) with the -a option.

* `docker container prune`
* `docker image prune`
* `docker network prune`
* `docker volume prune`

For unused images, use `docker image prune -a` (for removing dangling and ununsed images).
