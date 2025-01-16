+++
title = "Moving to Digital Ocean"
+++

Novo server, novo tema, novo idioma. Como a maioria dos assuntos em tecnologia é publicado em inglês e eu preciso praticar mais, vou estar tentando escrever alguns posts em inglês. Se vocês notarem qualquer erro, por favor me avisem :).

### Why change?

I was using a common host on TeHospedo, but sometimes I needed some configurations or process that was not in my hands, I had to ask to support do that for me. Some tasks were not solve fast as I would like and it is so frustrating that I decided move to a new server on [Digital Ocean](https://www.digitalocean.com/) that provide virtual machines with many Linux distributions options. Take a look on their advertising video:

<iframe src="http://www.youtube.com/embed/vHZLCahai4Q" height="315" width="560" allowfullscreen="" frameborder="0"></iframe>

### My Droplet (Virtual Machine)

My VM is a Linux CentOS 6.4 x64 with 512 MB RAM and 20GB SSD. This is amazing for my needs and I will can install, configure and run anything there as I prefer.

### Configuring the Server

My main reason for have this server nowadays is to host my wordpress blog and also my brother’s blog2. So for run the wordpress I had to configure the LAMP (Linux, Apache, MySQL and PHP). As unfortunately I have little experience with that configurations on Linux, I followed some good tutorials provided by Digital Ocean:

1.  [How to Install Linux, Apache, MySQL, PHP (LAMP) stack on CentOS 6](https://www.digitalocean.com/community/articles/how-to-install-linux-apache-mysql-php-lamp-stack-on-centos-6)
2.  [How to Install Wordpress on Centos 6](https://www.digitalocean.com/community/articles/how-to-install-wordpress-on-centos-6--2)
3.  [How to Set Up Apache Virtual Hosts on CentOS 6](https://www.digitalocean.com/community/articles/how-to-set-up-apache-virtual-hosts-on-centos-6) and [How to Serve Multiple Domains Using Virtual Hosts](http://www.rackspace.com/knowledge_center/article/how-to-serve-multiple-domains-using-virtual-hosts)(by rackspace)
4.  [How to Set Up a Host Name with DigitalOcean](https://www.digitalocean.com/community/articles/how-to-set-up-a-host-name-with-digitalocean)

### Extra Configurations

1.  [Getting permalinks working with apache2](http://ingolfzp.com/2009/03/getting-permalinks-working-with-apache2/)
2.  [Optimize website speed with mod_expire and mod_deflate](http://www.linux-faqs.info/apache/optimize-website-speed-with-mod-expire-and-mod-deflate)

Many configurations right? But I configured exactly as I want and I don't depend wait for any staff support to install or configure a new feature in my host.

