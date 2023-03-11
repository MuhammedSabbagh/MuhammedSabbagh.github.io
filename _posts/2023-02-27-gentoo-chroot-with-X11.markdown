---
layout: post
title:  "making a gentoo chroot with X11"
date:   2023-02-22 14:25:27 +0300
tags: code images
description: Because the pride of being an arch user isn't enough you put a gentoo neofetch next to it
categorize: linux 
---

# where did it all start?

By accident in a dark room setting in front of the computer and wondering "I have spare of time why don't I make a gentoo?"

ok this was horrible introduction to the article but to be fair we might sometimes need tools,custom-built software, or for the heck of it, giggles with our online friends on seeing 2 different neofetch setting next to each other, no matter the concern of it; I will show you how to do it

## acquire the ingredients

first you need to download your preferred stage3 tarball from the website, download it according to Gentoo's handbook then:

- create a directory with your distro's name like 'gentoo-fs' in (/)
- move the tarball inside this directory
- extract tar ball inside the gentoo-fs directory with this command

```bash
 ~# tar xpvf stage3-*.tar.xz --xattrs-include='*.*' --numeric-owner
```

- once you are done with this you are almost ready to go, well almost; you will have to go through the entire handbook except the following parts (Kernel, network pre-setting, partitioning) these are unnecessary as they are already included in the stock kernel for your distro *probably*

- all the things you will do in this chroot won't require reboot and if you wanted too you can simply reboot and resume your work on it by mounting proc dev etc…

- to finalize this is the script to automate logging into the chroot, make sure you made users to keep working on the chroot clean

```bash

#!/bin/bash

sudo mount --types proc /proc /gentoo/proc
sudo mount --rbind /sys /gentoo/sys
sudo mount --make-rslave /gentoo/sys
sudo mount --rbind /dev /gentoo/dev
sudo mount --make-rslave /gentoo/dev
sudo mount --bind /run /gentoo/run
sudo mount --make-slave /gentoo/run


xhost +localhost
DISPLAY=localhost:0 sudo chroot /gentoo /bin/login

sudo umount /gentoo/proc
sudo umount /gentoo/sys
sudo umount /gentoo/dev
sudo umount /gentoo/run

```

change the name of the folder above depending on the folder you created and have fun!

## one more thing

I will work on the X server soon in this blog so stay tuned!

peace and have a great day!

support my work on Ko-fi: [![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/S6S1IWSJQ)
