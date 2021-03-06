# GitLab in a Docker on Steroids

[![CircleCI Build Status](https://img.shields.io/circleci/project/pozgo/docker-gitlab-ce/master.svg)](https://circleci.com/gh/pozgo/docker-gitlab-ce)
[![GitHub Open Issues](https://img.shields.io/github/issues/pozgo/docker-gitlab-ce.svg)](https://github.com/pozgo/docker-gitlab-ce/issues)  
[![Stars](https://img.shields.io/github/stars/pozgo/docker-gitlab-ce.svg?style=social&label=Stars)]()
[![Fork](https://img.shields.io/github/forks/pozgo/docker-gitlab-ce.svg?style=social&label=Fork)]()  
[![](https://img.shields.io/github/release/pozgo/docker-gitlab-ce.svg)](http://microbadger.com/images/polinux/gitlab-ce)  
[![Docker build](http://dockeri.co/image/polinux/gitlab-ce)](https://hub.docker.com/r/polinux/gitlab-ce/)

Felling like supporting me in my projects use donate button. Thank You!  
[![](https://img.shields.io/badge/donate-PayPal-blue.svg)](https://www.paypal.me/POzgo)

[Docker Image](https://registry.hub.docker.com/u/polinux/gitlab-ce/) with GitLab Server. 
It's build the same way offcial images but with this image we added missing options into the docker image and will keep adding more features when requested by users. 

Options added so far:  
- Backup Time (crontab based)  
- Puppet Pre-Receive hook with puppet-lint syntax check. Set to false by default. If selected needed packages will be installed on run stage after gitlab is reconfigured and running.
  `*see environmental variables*`

*If you have an idea what should be added please let us know*

**More versions available through [tags](https://hub.docker.com/r/polinux/gitlab-ce/tags/)**

### Environmental Variable

|Variable|Description|
|:--|:--|
|`BACKUP_TIME`|Default set to: `0 12 * * *` <sup>1</sup>|
|`PUPPET_PRE_RECEIVE_HOOK_SUPPORT`|set `false` by default. use `enable` and specify version of puppet server `gem` package to install|
|`PUPPET_SERVER_VERSION`|default set to `5.1.0` <sup>2</sup>|

<sup>1</sup> - Cron based format.  
<sup>2</sup> - To list all vailable version of `gem` package use command below on host with installed `rubygems`  

```bash
gem search '^puppet$' --all | grep -o '\((.*)\)$' | tr -d '() ' | tr ',' "\n" | sort
```

**All options available through official image are available too. [GitLab Docs](https://docs.gitlab.com/omnibus/docker/)**

### Usage

```bash
docker run \
    -d \
    --name gitlab \
    -p 80:80 \
    -p 443:443 \
    -p 1022:22 \
    polinux/gitlab-ce
```

Set backup time to 15:00 and puppet pre-receive hook support for puppet 5.3.3
```bash
docker run \
    -d \
    --name gitlab \
    -p 80:80 \
    -p 443:443 \
    -p 1022:22 \
    -e BACKUP_TIME="0 15 * * *" \
    -e PUPPET_PRE_RECEIVE_HOOK_SUPPORT='true' \
    -e PUPPET_SERVER_VERSION='5.3.3' \
    polinux/gitlab-ce
```

Use `docker-compose.yml` file which contain basic setup of gitlab server with stand alone approach.

```bash
docker-compose up
```

### Build

```bash
docker build -t polinux/gitlab-ce .
```

Docker troubleshooting
======================

Use docker command to see if all required containers are up and running:

```bash
$ docker ps
```

Check logs of gitlab server container:

```bash
$ docker logs gitlab
```

Sometimes you might just want to review how things are deployed inside a running container, you can do this by executing a _bash shell_ through _docker's `exec_` command:

```bash
docker exec -ti gitlab /bin/bash
```

History of an image and size of layers:

```bash
docker history --no-trunc=true polinux/gitlab-ce | tr -s ' ' | tail -n+2 | awk -F " ago " '{print $2}'
```

---

## Author
Przemyslaw Ozgo (<linux@ozgo.info>)

---

### Acknowledgements
I would like to thank [JetBrains](https://www.jetbrains.com/) for supporting me with Open Source endeavours.