---
layout: default
title: "Resources"
category: "resources"
---

This page has links to useful resources.

# Software

## Linux

For the programming assignments, you will need to use a recent x86-64 (64 bit) version of Linux.

Any of the computers that are part of the [Ugrad net](https://support.cs.jhu.edu/wiki/Linux_Clients_on_the_CS_Undergrad_Net) will work.  You can use ssh to connect to one of these computers via the network.

If you use [Ubuntu 18.04](http://releases.ubuntu.com/18.04/), or a distribution based on Ubuntu 18.04 (such as [Linux Mint 19.1](https://linuxmint.com/release.php?id=34)), then you are using the same Linux version that [Gradescope](https://www.gradescope.com/) uses to run autograding scripts.  [Fedora Workstation](https://getfedora.org/en/workstation/) is also a good option.  Make sure you get the 64 bit version of whichever distribution you run!

[VirtualBox](https://www.virtualbox.org/) is a good way to run Linux if you don't want to (or can't) run Linux directly on your computer.  You'll want to allocate a reasonably-large amount of memory (2 GB minimum, 4 GB ideally) to the virtual machine.

## Tools

Some of the tools you'll want to have are:

* gcc
* make
* ruby
* valgrind
* git

All of these are available by default on the Ugrad computers.

To install on an Ubuntu-based system:

> <code class="cmd">sudo apt-get install gcc make ruby valgrind git</code>

To install on a Fedora system:

> <code class="cmd">sudo yum install gcc make ruby valgrind git</code>
