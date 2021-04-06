# ENV vs ARG in Dockerfile

[ENV in documentation](https://docs.docker.com/engine/reference/builder/#env)

[ARG in documentation](https://docs.docker.com/engine/reference/builder/#arg)

## ARG
```
ARG <name>[=<default value>]
```

The `ARG` instruction defines a variable that users can pass at build-time 
to the builder with the docker build command using the `--build-arg <varname>=<value>` flag. 
If a user specifies a build argument that was not defined in the Dockerfile, the build outputs a warning.

``` 
[Warning] One or more build-args [foo] were not consumed.
```

It is not recommended to use build-time variables for passing secrets like github keys, user credentials etc. 
Build-time variable values are visible to any user of the image with the `docker history` command.

A Dockerfile may include one or more ARG instructions. 
For example, the following is a valid Dockerfile:

```dockerfile
FROM busybox
ARG user1
ARG buildno
#...

```

An `ARG` instruction can optionally include a default value:

```dockerfile
FROM busybox
ARG user1=someuser
ARG buildno=1
```

If an `ARG` instruction has a default value and if there is no value passed at build-time, 
the builder uses the default.

**Scope**

An `ARG` variable definition comes into effect from the line on which it is defined 
in the Dockerfile not from the argument’s use on the command-line or elsewhere. 
For example, consider this Dockerfile:

```dockerfile
FROM busybox
USER ${user:-some_user}
ARG user
USER $user
```

A user builds this file by calling:
```dockerfile
 docker build --build-arg user=what_user .
```

The `USER` at line 2 evaluates to `some_user` as the user variable is defined on the subsequent line 3. 
The `USER` at line 4 evaluates to `what_user` as user is defined and the `what_user` 
value was passed on the command line. Prior to its definition by an `ARG` instruction, 
any use of a variable results in an empty string.

An `ARG` instruction goes out of scope at the end of the build stage where it was defined. 
To use an arg in multiple stages, each stage must include the `ARG` instruction.

```dockerfile
FROM busybox
ARG SETTINGS
RUN ./run/setup $SETTINGS

FROM busybox
ARG SETTINGS
RUN ./run/other $SETTINGS
```

**Using ARG variables**

You can use an ARG or an ENV instruction to specify variables that are available to the RUN instruction. 
***Environment variables defined using the ENV instruction 
always override an ARG instruction of the same name***. 

**Predefined ARGs**

Docker has a set of predefined ARG variables that you can use 
without a corresponding ARG instruction in the Dockerfile.
```dockerfile
HTTP_PROXY
http_proxy
HTTPS_PROXY
https_proxy
FTP_PROXY
ftp_proxy
NO_PROXY
no_proxy
```
To use these, simply pass them on the command line using the flag:
```
--build-arg <varname>=<value>
```
By default, these pre-defined variables are excluded from the output of docker history. 
Excluding them reduces the risk of accidentally leaking sensitive authentication information 
in an HTTP_PROXY variable.

## ENV
```
ENV <key>=<value> ...
```

The ENV instruction sets the environment variable <key> to the value <value>. 
This value will be in the environment for all subsequent instructions 
in the build stage and can be replaced inline in many as well. 
The value will be interpreted for other environment variables, 
so quote characters will be removed if they are not escaped. 
Like command line parsing, quotes and backslashes can be used to include spaces within values.

Example:
```dockerfile
ENV MY_NAME="John Doe"
ENV MY_DOG=Rex\ The\ Dog
ENV MY_CAT=fluffy
```

**The environment variables set using ENV will persist when a container is run from the resulting image**. 
You can view the values using `docker inspect`, and change them using `docker run --env <key>=<value>`.

Environment variable persistence can cause unexpected side effects. 
For example, setting `ENV DEBIAN_FRONTEND=noninteractive` changes the behavior of apt-get, 
and may confuse users of your image.
If an environment variable is only needed during build, 
and not in the final image, consider setting a value for a single command instead:
```dockerfile
RUN DEBIAN_FRONTEND=noninteractive apt-get update && apt-get install -y ...
```
Or use ARG, because it's not persisted after build.

## TLDR
ARG is used only for passing arguments to build container and it's not persisted after building process is finished.
ENV is used to set environment variables for the container. 
ENV variables will be persisted after building process and available for access while container is running.

#### Notes

[Source](https://vsupalov.com/docker-arg-env-variable-guide/)

Here’s a list of easy takeaways:

- The **.env file**, is only used during a pre-processing step when working 
  	with docker-compose.yml files. Dollar-notation variables like `$HI` are substituted for values 
  	contained in an “.env” named file in the same directory.
- **ARG** is only available during the build of a Docker image (`RUN` etc), not after the image is created 
	and containers are started from it (`ENTRYPOINT`, `CMD`). You can use ARG values to set ENV 
	values to work around that ([source](https://vsupalov.com/docker-build-time-env-values/)).
- **ENV** values are available to containers, but also RUN-style commands during 
  	the Docker build starting with the line where they are introduced.
- If you set an environment variable in an **intermediate container** using bash
  	 (`RUN export VARI=5 &&` …) it will not persist in the next command. 
  	 There’s [a way](https://vsupalov.com/set-dynamic-environment-variable-during-docker-image-build/) to work around that.
- An **env_file**, is a convenient way to pass many environment variables 
to a single command in one batch. This should not be confused with a **.env** file.
- Setting ARG and ENV values leaves **traces** in the Docker image. Don’t use them for secrets 
which are not meant to stick around (well, you kinda can with 
[multi-stage builds](https://vsupalov.com/build-docker-image-clone-private-repo-ssh-key/)).
