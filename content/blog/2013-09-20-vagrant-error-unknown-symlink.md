+++
title = "Vagrant - Error: UNKNOWN, symlink"
+++

Why does with Windows always has a problem or is more difficult to work with nice non-Microsoft stuff?! I was configuring Vagrant to run a project with Nodejs. Everything was going really well when my host machine was the Mac. But when I had to use the Windows as host machine then I got my first problem to run the command npm install inside the guest Ubuntu machine:

Error: UNKNOWN, symlink

After some attempts the only solution that works was use a parameter to not force use symlink when installing npm dependencies:

sudo npm install --no-bin-link

Take a look on this [post](http://www.conroyp.com/2013/04/13/symlink-shenanigans-nodejs-npm-express-vagrant) to understand better the problem.

