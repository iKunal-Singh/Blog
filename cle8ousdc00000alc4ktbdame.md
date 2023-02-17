# SElinux Installation

# Introduction

SELinux (Security-Enhanced Linux) is a security feature in Linux that can enhance the system's security by providing a mandatory access control mechanism, and this blog series will guide you on how to install it.

# Infrastructure

[Infrastructure Details](https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details)

[Infrastructure Diagram](https://lucid.app/lucidchart/58e22de2-c446-4b49-ae0f-db79a3318e97/view?page=0_0#)

> Signup to [KodeKloud - Engineer](https://kodekloud-engineer.com/#!/login) for practicing this task hands-on.

# Task Details

The xFusionCorp Industries security team recently did a security audit of their infrastructure and came up with ideas to improve the application and server security. They decided to use SElinux for an additional security layer. They are still planning how they will implement it; however, they have decided to start testing with app servers, so based on the recommendations they have the following requirements:

Install the required packages of SElinux on App server 3 in Stratos Datacenter and disable it permanently for now; it will be enabled after making some required configuration changes on this host. Don't worry about rebooting the server as there is already a reboot scheduled for tonight's maintenance window. Also, ignore the status of the SElinux command line right now; the final status after reboot should be disabled.

> Perform the below commands based on your question server, user name & other details might differ. So please read the task carefully before executing it. All the Best 👍

# **Solution:**

| **Server Name** | **IP** | **Hostname** | **User** | **Password** | **Purpose** |
| --- | --- | --- | --- | --- | --- |
| stapp01 | 172.16.238.10 | stapp01.stratos.xfusioncorp.com | tony | Ir0nM@n | Nautilus App 1 |
| stapp02 | 172.16.238.11 | stapp02.stratos.xfusioncorp.com | steve | Am3ric@ | Nautilus App 2 |
| stapp03 | 172.16.238.12 | stapp03.stratos.xfusioncorp.com | banner | BigGr33n | Nautilus App 3 |

## 1\. Log in on the App server as per the task

```plaintext
thor@jump_host /$ ssh  banner@stapp03

The authenticity of host 'stapp03 (172.16.238.12)' can't be established.

ECDSA key fingerprint is SHA256:j8YnnK5M8SCLiVJ/CwgldMmmxz/xQjPEosuM/URmKV4.

ECDSA key fingerprint is MD5:e4:b3:28:7a:4e:5a:e6:3e:9c:b4:6d:5c:25:2a:14:53.

Are you sure you want to continue connecting (yes/no)? yes

Warning: Permanently added 'stapp03,172.16.238.12' (ECDSA) to the list of known hosts.

banner@stapp03's password:

Permission denied, please try again.

banner@stapp03's password:

[banner@stapp03 ~]$ sudo su -

 We trust you have received the usual lecture from the local System

Administrator. It usually boils down to these three things:

     #1) Respect the privacy of others.

    #2) Think before you type.

    #3) With great power comes great responsibility.

 [sudo] password for banner:

[root@stapp03 ~]#
```

> A brief description of the "ssh", "sudo su-" and other commands are given in [Essential Linux Commands](https://ikunalsingh.hashnode.dev/introduction-to-essential-linux-commands)

## 2\. Install the SElinux 

```plaintext
[root@stapp03 ~]# yum -y install selinux*
```

## 3\. Check the existing  SELinux  status

```plaintext
[root@stapp03 ~]# sestatus

SELinux status:                 disabled

[root@stapp03 ~]#

[root@stapp03 ~]# cat /etc/selinux/config | grep SELINUX

# SELINUX= can take one of these three values:

SELINUX=enforcing

# SELINUXTYPE= can take one of three values:

SELINUXTYPE=targeted

[root@stapp03 ~]#
```

## 4\. Edit the **/etc/selinux/config**  file and correct the changes below

```plaintext
[root@stapp03 ~]# vi /etc/selinux/config
[root@stapp03 ~]# cat /etc/selinux/config | grep SELINUX
# SELINUX= can take one of these three values:
SELINUX=disabled
# SELINUXTYPE= can take one of three values:
SELINUXTYPE=targeted

[root@stapp03 ~]#
```

## **5.  V**alidate the task by **sestatus** 

```plaintext
[root@stapp03 ~]# sestatus

SELinux status:                 disabled

[root@stapp03 ~]#
```

Thank you so much for taking your valuable time to read

I took the initiative to learn in public and share my work with others. I tried my level best in squeezing as much information as possible in the easiest manner. Hope you learned something new today :)

**Learn** [Essential Linux Commands](https://ikunalsingh.hashnode.dev/introduction-to-essential-linux-commands)

Signup to [KodeKloud - Engineer](https://kodekloud-engineer.com/#!/login) for practicing these tasks hands-on.

In the next part of this blog, we will study 👇

<iframe src="https://giphy.com/embed/26BkO7k81ry3WP7hu" class="giphy-embed" width="270" height="480"></iframe>

[via GIPHY](https://giphy.com/gifs/mashable-till-next-time-robot-machine-26BkO7k81ry3WP7hu)