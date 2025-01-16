+++
title = "Linux command line to check the directory's size"
+++

Using the Nitrous.io I got this message "**disk quota exceeded**" after try install mongoid. The initial plan available for free accounts on Nitrous provides 750MB storage.

But I was asking myself how could my application in rails, that I just started to code, get all this space used? Try to guess could no help me to solve this problem then I learned this new linux command line to check the directories size:

```
du -h --max-depth=1 ~/ | sort -n -r
```

And I understood exactly where the problem was, my .rvm directory was too much big:

```
405M    /home/action/
222M    /home/action/.rvm
144M    /home/action/.gem
24M     /home/action/.nvm
24K     /home/action/.subversion
15M     /home/action/.vim
8.0K    /home/action/.ssh
1.8M    /home/action/easy_order
0       /home/action/workspace
0       /home/action/.cache
```

_ps: I got this output after remove a ruby version not used_

Explaning the command line:

- `du` : shows how much space one ore more files or directories is using
- `-h` : format output to show the size in MB
- `--max-depth=1` : consider just the top directories
- `\~/` : direcoty's path to exam
- `| sort -n -r`: sort by size

