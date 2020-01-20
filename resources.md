---
layout: default
title: "Resources"
category: "resources"
---

This page has links to useful resources.

# Software

## Linux

For the programming assignments, you will need to use a recent x86-64 (64 bit) version of Linux.

Any of the computers that are part of the [Ugrad
net](https://support.cs.jhu.edu/wiki/Linux_Clients_on_the_CS_Undergrad_Net)
will work reasonably well, although the compiler versions will not
necessarily be the same ones that Gradescope uses.  You can use ssh to
connect to one of these computers via the network.

If you are planning to use your own computer for development,
we *highly* recommend using some flavor of Ubuntu 18.04.  If your
computer is running Windows or MacOS, and you would still like to do
development on your own machine, one excellent option is to install
[VirtualBox](https://www.virtualbox.org) on your computer, and then
download the following Virtual Machine image:

> [Ubuntu MATE 18.04 for CSF.ova](https://drive.google.com/file/d/13rsQzOWNvAW3AIIC6M-BcoNRDYgdwcYA/view?usp=sharing) (3.6 GB download: username `csf`, password `csf`)

The above VM image has all of the software you will need to work on
assignments in CSF, and it more or less exactly matches the environment
used by Gradescope autograders.

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
