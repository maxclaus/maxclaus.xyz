+++
title = "Docker Build Error"
+++

I was playing around Docker this week using this Dockerfile to build my container:

{% gist maxcnunes/9956342 Dockerfile %}

But during the build I was getting this error message:

{% gist maxcnunes/9956342 docker_build_error.sh %}

I was using a Linux Mint 16 and after some research to solve the problem I figured out was missing install the [LXC](https://linuxcontainers.org/).

```bash
sudo apt-get install lxc
```
