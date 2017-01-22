---
layout: post
title:  Installing Ubuntu 16.04 LTS with Windows 10, NVIDIA drivers and CUDA on Alienware 15 R3 (GTX1070)
date:   2017-01-22 16:37:47 +0100
categories: ubuntu install gtx1070 nvidia cuda gtx pascal linux
---
Despite the fact that NVIDIA Pascal GPUs appeared in the middle of 2016, we faced some compatibility problems while installing Ubuntu and nvidia drivers on it even in 2017. This post is a short description which sums up our 2-days attempts to install Ubuntu on Alienware 15R3 and make it run.

This post will describe installation of Ubuntu as a second system along with Windows10 (already pre-installed) and creating dual boot.

First of all and this is very important, since it took us a while to understand why installation is always failing, is to TURN OFF DISCRETE GPU. On Alienware it can be done by `fn + F7` and rebooting the system. Ubuntu does not have nvidia drivers by default and after installation the system falls into Recovery mode. In Recovery mode we didn't manage to install nvidia drivers, as Secure boot (which is set in BIOS by default) doesn't let network installations.

The second step is optional, but the most important. We created a backup of Windows10, since if during Linux installation some files were damaged, it would be easy to return back to working system.

Download Ubuntu distribution. We downloaded Ubuntu from official [website](http://ubuntu.ru/get).

The next step is to create LiveUSB. We tried multiple different USB with 4/8/16 Gb, both 2.0 and 3.0. In our case only USB 3.0 with 16 Gb Sandisk Ultra Fit worked fine. The software used for creating bootable USB is [Rufus](https://rufus.akeo.ie/).

Afterwards you need to allocate memory for Ubuntu on hard drive. We allocated 200 Gb of hard drive memory for Linux system. It is easy to do in Disk Management in Windows. Detailed explanation with screenshots is provided [here](http://www.everydaylinuxuser.com/2015/11/how-to-shrink-windows-10-to-make-space.html). By the way, same procedure can be done later during Ubuntu installation with a help of Gparted software (pre-installed in Ubuntu).

Than reboot your computer and press `F12` to get into BIOS menu.

Go to `BIOS SETUP`, there to `Advanced settings` and change `SATA` mode from `RAID ON` to `ACHI`. If it is not done, Ubuntu will not detect SSD drive with UEFI partition.

Turn off `Secure boot` mode.

Save all changes.

The system will reboot.

After reboot again press `F12` and choose an option to boot from USB.

In dialog menu choose `Try Ubuntu without installation`. If this option works well, it means that GPU is disabled and doesn't prevent Linux from booting correctly.

On Desktop click `Install Ubuntu` and follow the instructions of the installer.

When the installer asks to choose, whether to install Ubuntu along with Windows or erase Windows and install Ubuntu instead, choose `Something else`. It is the lowest possible option.

When Installer suggests to partition disk space, you need to create 3 partitions:
- `/` for system, use as `Ext4 journaling file system` (We allocated 120 Gb)
- `/home` for user data, use as `Ext4 journaling file system` (We allocated 60 Gb)
- `swap area`(Should be equal to the size of RAM)

In the bottom of the same window choose where to install boot-loader. You should choose the option with `Windows Boot Manager` to enable dual boot option.

All the rest steps are standard. After installation Ubuntu will ask for reboot, so reboot the system.

Wait until system is rebooted it's time now to upgrade the kernel. Current kernel version provided with Ubuntu distribution does not support NVIDIA drivers for GTX1070. We upgraded the kernel to version 4.8 using [this tutorial](http://tipsonubuntu.com/2016/10/03/install-linux-kernel-4-8-in-ubuntu-linux-mint/).

The next step is to install nvidia drivers. We tried multiple options, but [this blog post](https://www.jayakumar.org/linux/gtx-1070-on-ubuntu-16-04-with-cuda-8-0-and-theano/) helped a lot. Since Ubuntu is just installed, first steps from there

{% highlight ruby %}
sudo apt-get purge nvidia-*
sudo apt-get autoremove
sudo reboot
{% endhighlight %}

are not needed.

This is all. After reboot GTX1070 should mount.

If after reboot you get blue screen and Ubuntu does not boot, try [these instructions](
http://askubuntu.com/questions/839279/stuck-on-blue-screen-while-booting-the-system).

Our github profiles are:
- [tazdriver](https://github.com/tazdriver)
- [orech](https://github.com/orech)



[jekyll-docs]: http://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
