---
title: "Linux on the MacBook Pro (mid-2009)"
date: 2020-08-15
layout: post
---

A few months ago, I bought a mid 2009 MacBook Pro for only £125 with free postage on eBay, and I have to say, for a mid 2009 laptop, 8GBs RAM and a Kingston SSD, it makes for a great development machine. Sadly, the latest macOS version available (without using unofficial patches) is El Capitan 10.11. This gave me the incentive to make the machine a dual boot development machine capable of using macOS software (32-bit and 64-bit software - beat that Catalina!) such as Ableton Live 10, Final Cut Pro and also some more development-heavy applications such as an updated GCC compiler, VirtualBox/QEMU for homebrew/mainstream OS kernel development with the latest development libraries and headers available.  

Coming from a background of using mainly Wintel computers and keyboards, the MacBook having the Eject key instead of Delete, and Super/Windows key and Alt/Option keys swapped is really starting to get on my nerves now. Everything else about it is great however. Thankfully the open nature of Linux (Xubuntu in my case, which has always been my go-to distribution for new hardware) gives me the ability to remap the keys to behave much more like a bog-standard PC keyboard, which I feel will aid in my productivity and development, even if it's a miniscule change it will just feel a lot better. 

Searching the web came up with numerous solutions, from some recompiling kernel modules with new behaviour implemented in the module and the new kernel module being used. That put me off this entire thing, until I came across this solution which just involved editing a single file in a /etc/ directory under the root user, which will make my day a lot easier. After all, I must say I am quite comfortable using the commandline, more so than any GTK# configuration program which probably only works in a specific Linux distribution.
As Gautam Iyer else has done the heavy lifting, I'm giving them all due credit for doing it this way. You can see their blog post at [this website link](http://www.math.cmu.edu/~gautam/sj/blog/20171207-keyboard-settings.html) if you want to do the same with your machine. 

What I've had to do is create a new file as root called '/etc/modprobe.d/hid-apple-local.conf', so you can run  

```  
sudo touch /etc/modprobe.d/hid-apple-local.conf  
```  

and then paste the following into that file, using your favorite text editor (I'm just using GNU Nano under CLI):  

```
options hid_apple swap_opt_cmd=1
```  
and that should solve the issue! I can't confirm whether this works with all distributions, but as it's a modprobe.d file, I can imagine it working well across numerous different distributions. If you have any issues regarding this, Gautam Iyer does also have a few other solutions on his blog post, so I recommend you look on there for more.

Edit: I forgot to mention the Eject key, which is useless on my machine 'cause I broke the DVD-drive!
In your home folder, create a new file called .Xmodmap using 
```
touch .Xmodmap
```
, and then paste the following into that file, reboot your machine and it will work just as a Delete key, which Apple failed to put on their machines >:-)  
```
keycode 169 = Delete
```
