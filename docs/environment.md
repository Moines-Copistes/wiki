# My Setup

This page will give you an overview of the environment I use in my daily hacking activites.

To give you some context, my job focuses primarly on web applications and infrastructure penetration testing, but I am also active on Hack the Box and CTFs.


## Distribution

For work related reasons, I am using Windows 11 as my main OS. The main benefit of using Windows in my point of view is its reliability.

Of course, it would be though to rely on Windows for hacking/pentesting and I need a Linux distribution.


## WSL

I am therefore using a [WSL](https://learn.microsoft.com/en-us/windows/wsl/about) distribution on top of Windows running Arch Linux. Why Arch ? Mainly because of the ease of installing packages and because of [BlackArch](https://blackarch.org/index.html). Blackarch is a community-driven repository that contain over 2900+ tools dedicated to penetration testing. It can be installed on top of Arch Linux very easily.

There is just one caveat of using a WSL distribution instead of a Virtual Machine, it is not isolated from the host, where I store sensitive professional information. Therefore, I am using another layer of tooling on top of WSL.


## Exegol

You might have heard of [Exegol](https://exegol.readthedocs.io/en/latest/), a hacking enviornment that differentiates itself because it consists of Docker containers that can be quickly deployed and destroyed.

This brings many advantages, the main one being that every test or other activity is isolated from one another and from my machine. In addition of including many tools and useful scripts, exegol allows users to bring along their tools, scripts, python packages and many more. All of this will then be available when you spin up a new container for a project.

My typical workflow is to create a new container for each project I start working on. This can go from a usual pentest to a CTF or a HackTheBox pro lab.


## Virtual Machines

I always have a virtual machine ready to go in the event I have an issue with my usual environment. I use VMWare and usually have 2 VMs ready to go: a [Kali Linux](https://www.kali.org/) machine, and a [Commando VM](https://github.com/mandiant/commando-vm).


## Why those choices ?

I have received the question: Why don't you simply use a Kali VM ?

Well, first of all, I like having isolation between projects and in the case I break an environment, I can simply spin up another container. This is not the case with a VM, or I did not find a convenient way to do it personnaly.

I am also used to do most of what I do in a terminal, give me ZSH, tmux and vim, and I'm an happy man. Using WSL and Exegol also means that I can pretty much stay in my cosy terminal for most of the time.
