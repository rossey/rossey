---
layout: post
title: "Ansible Provision Slow on CentOS Host"
modified:
excerpt: "Are you finding provisioning your Ansible hosts over SSH slow?"
tags: ['Ansible', 'Provisioning', 'Linux', 'SSH', 'CentOS']
---

By default the CentOS SSH server configuration comes with `GSSAPIAuthentication` enabled - this supports SSH login via [Kerberos](https://en.wikipedia.org/wiki/Kerberos_(protocol)).

Disabling GSSAPIAuthentication will stop the SSH server trying to connect to a Kerberos server for authentication.

## Server Side Solution
Here are a few Ansible Playbook examples for you to use.

#### GSSAPIAuthentication
You need to change the `GSSAPIAuthentication yes` line in `/etc/sshd/sshd_config` to `GSSAPIAuthentication no`:

    name: "Disable GSSAPIAuthentication for SSH login"
    lineinfile:
        regexp: "^GSSAPIAuthentication"
        line: "GSSAPIAuthentication no"
        state: "present"
        dest: "/etc/sshd/sshd_config"

#### useDNS
You can also try disabling reverse DNS lookup on SSH login:

    name: "Disable reverse DNS lookup on SSH login"
    lineinfile:
        line: "useDNS no"
        state: "present"
        dest: "/etc/sshd/sshd_config"


## Client Side Solution

You can try disabling GSSAPIAuthentication in your `~/.ssh/config` file:

    Host *
      GSSAPIAuthentication no


Resources:

- https://coderwall.com/p/fukoew/speed-up-ssh-logon-by-disabling-gssapiauthentication
- http://www.njcrawford.com/2011/09/01/delayed-ssh-login-on-centos-6

