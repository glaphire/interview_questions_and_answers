# Volumes in Docker

[Official docker documentation for Volumes](https://docs.docker.com/storage/volumes/)

## What is a Volume? 

Volumes are preferred way to to persist data that are generated and used by docker containers.

While bind mounts are dependent on the directory structure and OS of the host machine, 
volumes are completely managed by Docker. 
Volumes have several advantages over bind mounts:
- Volumes are easier to back up or migrate than bind mounts.
- You can manage volumes using Docker CLI commands or the Docker API.
- Volumes work on both Linux and Windows containers.
- Volumes can be more safely shared among multiple containers.
- Volume drivers let you store volumes on remote hosts or cloud providers, to encrypt the contents of volumes, or to add other functionality.
- New volumes can have their content pre-populated by a container.
- Volumes on Docker Desktop have much higher performance than bind mounts from Mac and Windows hosts.

**In addition, volumes are often a better choice than persisting data in a container’s writable layer, 
because a volume does not increase the size of the containers using it, 
and the volume’s contents exist outside the lifecycle of a given container**

![Docker volumes illustration](https://github.com/glaphire/interview_questions_and_answers/blob/main/src/docker/images/types-of-mounts-volume.png)

## How to describe it in docker-compose.yml (on global level and on container level)

[Docker-compose volumes documentation](https://docs.docker.com/storage/volumes/#use-a-volume-with-docker-compose)

WIP - read and write here https://nickjanetakis.com/blog/docker-tip-28-named-volumes-vs-path-based-volumes