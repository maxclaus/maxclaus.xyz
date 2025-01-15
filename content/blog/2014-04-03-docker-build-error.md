+++
title = "Docker Build Error"
+++

I was playing around Docker this week using this Dockerfile to build my container:

```
docker build .
Uploading context 4.096 kB
Uploading context
Step 0 : FROM ubuntu
 ---> 9cd978db300e
Step 1 : MAINTAINER Max Claus Nunes
 ---> Using cache
 ---> 987fcecff08b
Step 2 : RUN apt-get install -y python-software-properties python
 ---> Running in b9ceb62a54d3
2014/04/03 14:09:14 no such file or directory
2014/04/03 11:09:14 The command [/bin/sh -c apt-get install -y python-software-properties python] returned a non-zero code: 1
```

But during the build I was getting this error message:

```Dockerfile
FROM ubuntu
MAINTAINER Max Claus Nunes

RUN apt-get install -y python-software-properties python
RUN add-apt-repository ppa:chris-lea/node.js
RUN echo "deb http://us.archive.ubuntu.com/ubuntu/ precise universe" >> /etc/apt/sources.list
RUN apt-get update
RUN apt-get install -y nodejs
#RUN apt-get install -y nodejs=0.6.12~dfsg1-1ubuntu1
RUN mkdir /var/www

ADD app.js /var/www/app.js

CMD ["/usr/bin/node", "/var/www/app.js"]
```

I was using a Linux Mint 16 and after some research to solve the problem I figured out was missing install the [LXC](https://linuxcontainers.org/).

```bash
sudo apt-get install lxc
```
