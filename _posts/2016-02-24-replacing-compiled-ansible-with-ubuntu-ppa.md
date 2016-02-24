---
layout: post
title: "Replacing compiled Ansible with the Ubuntu PPA"
modified:
excerpt: "Ansible doesn't provide a make uninstall if you've compiled from source"
tags: ['Ansible', 'Provisioning', 'Linux', 'Ubuntu', 'DevOps']
---

When I first starting using Ansible the installation was compiled from source and Ansible doesn't provide a `make uninstall` step. These are the steps that I took to replace a compiled version of Ansible with the official Ubuntu PPA but this is equally valid for any of the other [recommended ways of installing Ansible on your system](http://docs.ansible.com/ansible/intro_installation.html).

## Remove Ansible executables

The compiled Ansible executables were installed to the `/usr/local/bin` directory on my machine.

    $ which ansible
    /usr/local/bin/ansible


    $ ls -l /usr/local/bin/ansible*
    -rwxr-xr-x 1 root root 175 Jun 16  2015 /usr/local/bin/ansible
    -rwxr-xr-x 1 root root 183 Jun 16  2015 /usr/local/bin/ansible-doc
    -rwxr-xr-x 1 root root 189 Jun 16  2015 /usr/local/bin/ansible-galaxy
    -rwxr-xr-x 1 root root 193 Jun 16  2015 /usr/local/bin/ansible-playbook
    -rwxr-xr-x 1 root root 185 Jun 16  2015 /usr/local/bin/ansible-pull
    -rwxr-xr-x 1 root root 187 Jun 16  2015 /usr/local/bin/ansible-vault


    $ rm /usr/local/bin/ansible*

## Remove Ansible python egg

Removing the Ansible python egg will remove package conflicts that old library packages.

    $ ls -l /usr/local/lib/python2.7/dist-packages/
    drwxr-sr-x 4 root staff   4096 Jun 16  2015 ansible-1.9.2-py2.7.egg

    
    $ rm -r /usr/local/lib/python2.7/dist-packages/ansible-1.9.2-py2.7.egg

## Install Ansible from the Official Ubuntu PPA

    $ sudo apt-get install software-properties-common
    $ sudo apt-add-repository ppa:ansible/ansible
    $ sudo apt-get update
    $ sudo apt-get install ansible

## Running Ansible 1.9 alongside Ansible 2.0

If you want to run both Ansible 1.9 with Ansible 2.0 checkout [Larry Smith's](https://twitter.com/mrlesmithjr) article on [Setting up an Ansible Control Machine](http://everythingshouldbevirtual.com/ansible-setting-up-an-ansible-control-machine-part-1)

### Resources:

- <http://docs.ansible.com/ansible/intro_installation.html#latest-releases-via-apt-ubuntu>
- <http://stackoverflow.com/questions/30605302/ansible-not-recognizing-deb-parameter-for-aptitude>

