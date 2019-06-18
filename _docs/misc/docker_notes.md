---
title: Notes on Docker
tags: [misc]
category: misc
summary: ""
---

# Installation

<https://github.com/docker/docker-install>

"Manage Docker as a non-root user": <https://docs.docker.com/install/linux/linux-postinstall/>

Command line: <https://docs.docker.com/engine/reference/commandline/docker/>

## Volumes

Default under `/var/lib/docker/volumes`

* `docker volume ls`: list volumes (<https://docs.docker.com/engine/reference/commandline/volume_ls/>)
* `docker volume inspect <volume_name>` : detailed informatin  (<https://docs.docker.com/engine/reference/commandline/volume_inspect/>)
* `docker inspect -f '{{ .Mounts }}' containerId`: Volume mounts for container with containerId  
 

