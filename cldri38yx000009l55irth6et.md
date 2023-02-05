# Linux NTP Setup

## Infrastructure

[Infrastructure Details](https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details)

[Infrastructure Diagram](https://lucid.app/lucidchart/58e22de2-c446-4b49-ae0f-db79a3318e97/view?page=0_0#)

> Signup to [KodeKloud - Engineer](https://kodekloud-engineer.com/#!/login) for practicing this task hands-on.

# Task Details

The system admin team of xFusionCorp Industries has noticed an issue with some servers in Stratos Datacenter where some servers are not in sync w.r.t time. Because of this, several application functionalities have been impacted. To fix this issue the team has started using common/standard NTP servers. They are finished with most of the servers except App Server 1. Therefore, perform the following tasks on this server:

Install and configure the NTP server on App Server 1.

Add NTP server 1.sg.pool.ntp.org in NTP configuration on App Server 1.

Please do not try to start/restart/stop ntp service, as we already have a restart for this service scheduled for tonight and we don't want these changes to be applied right now.

> Perform the below commands based on your question server, user name & other details might differ. So please read the task carefully before executing it. All the Best 👍

# **Solution:**

| **Server Name** | **IP** | **Hostname** | **User** | **Password** | **Purpose** |
| --- | --- | --- | --- | --- | --- |
| stapp01 | 172.16.238.10 | stapp01.stratos.xfusioncorp.com | tony | Ir0nM@n | Nautilus App 1 |
| stapp02 | 172.16.238.11 | stapp02.stratos.xfusioncorp.com | steve | Am3ric@ | Nautilus App 2 |
| stapp03 | 172.16.238.12 | stapp03.stratos.xfusioncorp.com | banner | BigGr33n | Nautilus App 3 |

## 1\. Log in on the App server as per the task

```plaintext
thor@jump_host ~$ ssh tony@stapp01

The authenticity of host 'stapp01 (172.16.238.10)' can't be established.

ECDSA key fingerprint is SHA256:2DbVbI7xvrQSSIOQD502dx7BHfADaJEecTxNIX/MmQs.

ECDSA key fingerprint is MD5:c6:8e:51:2e:28:dd:bd:cc:e4:b6:b5:c4:d3:dc:13:e6.

Are you sure you want to continue connecting (yes/no)? yes

Warning: Permanently added 'stapp01,172.16.238.10' (ECDSA) to the list of known hosts.

tony@stapp01's password:

 [tony@stapp01 ~]$ sudo su -

 We trust you have received the usual lecture from the local System

Administrator. It usually boils down to these three things:

     #1) Respect the privacy of others.

    #2) Think before you type.

    #3) With great power comes great responsibility.

 [sudo] password for tony:

[root@stapp01 ~]#
```

> A brief description of the commands "ssh" and "sudo su-" is given in Essential [Linux Commands](https://ikunalsingh.hashnode.dev/introduction-to-essential-linux-commands)

## 2\. Check NTP is installed, if not then install it on the server

```plaintext
[root@stapp01 ~]# rpm -qa |grep ntp

[root@stapp01 ~]#

[root@stapp01 ~]# yum install -y ntp

Loaded plugins: fastestmirror, ovl

Determining fastest mirrors

 * base: ftpmirror.your.org

 * extras: bay.uchicago.edu

 * updates: ftpmirror.your.org

base                                                                       | 3.6 kB  00:00:00    

extras                                                                     | 2.9 kB  00:00:00    

updates                                                                    | 2.9 kB  00:00:00    

(1/4): base/7/x86_64/group_gz                                              | 153 kB  00:00:00    

(2/4): extras/7/x86_64/primary_db                                          | 242 kB  00:00:00    

(3/4): base/7/x86_64/primary_db                                            | 6.1 MB  00:00:00    

(4/4): updates/7/x86_64/primary_db                                         | 8.8 MB  00:00:00    

Resolving Dependencies

--> Running transaction check

---> Package ntp.x86_64 0:4.2.6p5-29.el7.centos.2 will be installed

--> Processing Dependency: ntpdate = 4.2.6p5-29.el7.centos.2 for package: ntp-4.2.6p5-29.el7.centos.2.x86_64

--> Processing Dependency: libopts.so.25()(64bit) for package: ntp-4.2.6p5-29.el7.centos.2.x86_64

--> Running transaction check

---> Package autogen-libopts.x86_64 0:5.18-5.el7 will be installed

---> Package ntpdate.x86_64 0:4.2.6p5-29.el7.centos.2 will be installed

--> Finished Dependency Resolution

 

Dependencies Resolved

 

==================================================================================================

 Package                  Arch            Version                             Repository     Size

==================================================================================================

Installing:

 ntp                      x86_64          4.2.6p5-29.el7.centos.2             base          549 k

Installing for dependencies:

 autogen-libopts          x86_64          5.18-5.el7                          base           66 k

 ntpdate                  x86_64          4.2.6p5-29.el7.centos.2             base           87 k

 

Transaction Summary

==================================================================================================

Install  1 Package (+2 Dependent packages)

 

Total download size: 701 k

Installed size: 1.6 M

Downloading packages:

(1/3): autogen-libopts-5.18-5.el7.x86_64.rpm                               |  66 kB  00:00:00    

(2/3): ntpdate-4.2.6p5-29.el7.centos.2.x86_64.rpm                          |  87 kB  00:00:00    

(3/3): ntp-4.2.6p5-29.el7.centos.2.x86_64.rpm                              | 549 kB  00:00:00    

--------------------------------------------------------------------------------------------------

Total                                                             1.2 MB/s | 701 kB  00:00:00    

Running transaction check

Running transaction test

Transaction test succeeded

Running transaction

  Installing : autogen-libopts-5.18-5.el7.x86_64                                              1/3

  Installing : ntpdate-4.2.6p5-29.el7.centos.2.x86_64                                         2/3

  Installing : ntp-4.2.6p5-29.el7.centos.2.x86_64                                             3/3

  Verifying  : ntpdate-4.2.6p5-29.el7.centos.2.x86_64                                         1/3

  Verifying  : ntp-4.2.6p5-29.el7.centos.2.x86_64                                             2/3

  Verifying  : autogen-libopts-5.18-5.el7.x86_64                                              3/3

 Installed:

  ntp.x86_64 0:4.2.6p5-29.el7.centos.2                                                           

 Dependency Installed:

  autogen-libopts.x86_64 0:5.18-5.el7           ntpdate.x86_64 0:4.2.6p5-29.el7.centos.2         

 Complete!
```

## rpm -qa | grep ntp

The "rpm -qa | grep ntp" is used to search the RPM database for any packages that contain the string "ntp". The "rpm -qa" command lists all installed packages on the system, and the "grep ntp" part filters the output to only show packages that have "ntp" in their name or description. The result is a list of all installed NTP (Network Time Protocol) packages.

# 3\. Confirm package install successfully

```plaintext
[root@stapp01 ~]# rpm -qa |grep ntp

ntp-4.2.6p5-29.el7.centos.2.x86_64

ntpdate-4.2.6p5-29.el7.centos.2.x86_64

[root@stapp01 ~]#
```

## 4\. Add the NTP server to the configuration file

```plaintext
[root@stapp01 ~]# vi /etc/ntp.conf

[root@stapp01 ~]#

[root@stapp01 ~]# cat /etc/ntp.conf |grep sg.pool

server 1.sg.pool.ntp.org

[root@stapp01 ~]#
```

## 5\. Check the NTP daemon status

```plaintext
[root@stapp01 ~]# ntpstat

Unable to talk to NTP daemon. Is it running?

[root@stapp01 ~]#

  [root@stapp01 ~]# systemctl status ntpd

● ntpd.service - Network Time Service

   Loaded: loaded (/usr/lib/systemd/system/ntpd.service; disabled; vendor preset: disabled)

   Active: inactive (dead)
```

## 6\. Start the NTP daemon enable and check the status

```plaintext
[root@stapp01 ~]# systemctl enable ntpd

Created symlink from /etc/systemd/system/multi-user.target.wants/ntpd.service to /usr/lib/systemd/system/ntpd.service.

[root@stapp01 ~]# systemctl start  ntpd

[root@stapp01 ~]#

[root@stapp01 ~]# systemctl status ntpd

● ntpd.service - Network Time Service

   Loaded: loaded (/usr/lib/systemd/system/ntpd.service; enabled; vendor preset: disabled)

   Active: active (running) since Sun 2021-06-20 10:40:05 UTC; 4s ago

  Process: 1151 ExecStart=/usr/sbin/ntpd -u ntp:ntp $OPTIONS (code=exited, status=0/SUCCESS)

 Main PID: 1152 (ntpd)

   CGroup: /docker/dc4d87bd2c999a9987e26698b929f742c010e3ec3a5e69c4c92a08017662753a/system.slice/ntpd.service

           └─1152 /usr/sbin/ntpd -u ntp:ntp -g 

Hint: Some lines were ellipsized, use -l to show in full.

[root@stapp01 ~]#
```

## 7\. Validate the task by NTP status

```plaintext
[root@stapp01 ~]# ntpstat

synchronised to NTP server (66.85.78.80) at stratum 3

   time correct to within 235 ms

   polling server every 64 s

[root@stapp01 ~]#
```

Thank you so much for taking your valuable time to read

I took the initiative to learn in public and share my work with others. I tried my level best in squeezing as much information as possible in the easiest manner. Hope you learned something new today :)

**Learn** [Essential Linux Commands](https://ikunalsingh.hashnode.dev/introduction-to-essential-linux-commands)

Signup to [KodeKloud - Engineer](https://kodekloud-engineer.com/#!/login) for practicing these tasks hands-on.

In the next part of this blog, we will study 👇

<iframe src="https://giphy.com/embed/m9eG1qVjvN56H0MXt8" class="giphy-embed" width="295" height="480"></iframe>

[via GIPHY](https://giphy.com/gifs/baby-bye-slide-m9eG1qVjvN56H0MXt8)