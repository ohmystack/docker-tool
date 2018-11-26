# dt (docker-tool)

![logo](docs/img/logo.png)

It is a convenient tool for many Docker operations.

```plain
    __              __                      __                __
.--|  |.-----.----.|  |--.-----.----.______|  |_.-----.-----.|  |
|  _  ||  _  |  __||    <|  -__|   _|______|   _|  _  |  _  ||  |
|_____||_____|____||__|__|_____|__|        |____|_____|_____||__|

dt (docker-tool)

Usage:
sudo ./docker-tool <action> [args...]

Actions:
  clean-img                        | Clean dangling images
  clean-vol                        | (DANGEROUS!!!) Clean dangling volumes, who are not referenced by any containers
  clean-container                  | Remove exited containers
  img [name]                       | Grep image by given <name>
  logs <container-id/name>         | == docker logs --tail=50 -f <container-id/name>
  net <container-id/name>          | Show network info (mode, veth pair, ...) of a container
  ns-net <container-id/name>       | Enter the net namespace of a container, with all host's tools available. Use "exit" to exit
  pid <container-id/name>          | Get the pid of a container
  ps [-a] [name]                   | Grep from docker ps
  run ...                          | == docker run -it --rm ...
  ssh <container-id/name or image> | Enter the bash/sh of a container or image
  config [-l|--list] <key> <value> | config docker-tool, save under /etc/docker-tool/ (available keys: tag-prefix)
                                   |   '-l', '--list' to show all the current configurations.
  tag [-p] [--rm] <image>          | tag the image by configured prefix.
                                   |   '-p' to push the new image after tagging it.
                                   |   '--rm' to remove the new local image after pushing it. (must use --rm with -p)

Other actions:
  clean-k8s                        | Clean dangling k8s containers
  self-upgrade                     | Upgrade "docker-tool" to the latest version

```

## Installation

```bash
INSTALL_DIR='/usr/local/bin'
sudo wget -O ${INSTALL_DIR}/docker-tool https://raw.githubusercontent.com/ohmystack/docker-tool/master/docker-tool
sudo chmod a+x ${INSTALL_DIR}/docker-tool
sudo ln -sf ${INSTALL_DIR}/docker-tool ${INSTALL_DIR}/dt
```

You can use command-line `docker-tool` or `dt` for short.

> Upgrade:
> 
> ```bash
> dt self-upgrade
> ```


## Key Features

### "ssh" into a container or an image easily

* `dt ssh`

This is not a real "ssh", but most users want such kind of experience.

> It is complicated to do this without this tool, you will type `docker exec -it xxx /bin/bash`, and then find that there is no `bash` in the container, then change to `sh`. Or, type `docker run -it --rm --entrypoint /bin/bash xxx` to get into an image.

```bash
dt ssh <image>
```

![dt-ssh-image](docs/img/dt-ssh-image.png)

```bash
dt ssh <container>
```

![dt-ssh-container](docs/img/dt-ssh-container.png)


### Search

* Images: `dt img`

```bash
dt img <keyword>
```

* Containers: `dt ps`

```bash
dt ps <keyword>
dt ps -a <keyword>
```


### Short-cut for docker commands

* `dt logs`

Short for `docker logs --tail=50 -f <container-id/name>`.

```bash
dt logs <container-name>
```

* `dt pid`

Get the pid of a container.

```bash
dt pid <container-name>
```


### Clean

* `dt clean-img`

Clean the dangling images. May save lots of your disk space.

* `dt clean-container`

Clean the exited containers.

* `dt clean-vol`

Clean the dangling volumes.


### Network

* `dt net`

Get the network type and info.  
You can even get the **veth pair info** of a container if it is using the "default" NetworkMode. This is very helpful when debugging the network.

![dt-net-default](docs/img/dt-net-default.png)
![dt-net-host](docs/img/dt-net-host.png)

* `dt ns-net`

Enter the network namespace of a container.  
So that you can use the utils installed on your server to debug the container inside network.

![dt-ns-net](docs/img/dt-ns-net.png)


## Development

If you have any good ideas about `docker-tool`, welcome to submit your PRs.

One simple rule: Keep this `docker-tool` a single file bash program.

[@ohmystack](https://github.com/ohmystack) 
