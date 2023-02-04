# Linux Services

# Infrastructure

[Infrastructure Details](https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details)

[Infrastructure Diagram](https://lucid.app/lucidchart/58e22de2-c446-4b49-ae0f-db79a3318e97/view?page=0_0#)

> Signup to [KodeKloud - Engineer](https://kodekloud-engineer.com/#!/login) for practicing this task hands-on.
> 
> A brief description of commands is given in Essential Linux Commands.

# Task Details

As per details shared by the development team, the new application release has some dependencies on the back end.

There are some packages/services that need to be installed on all app servers under Stratos Datacenter. As per requirements please perform the following steps.

   a. Install the squid package on all the application servers.

  
  b. Once installed, make sure it is enabled to start during boot.

> Perform the below commands based on your question server,  user name & other details that might differ as per the task.

# **Solution:**

## 1. At first login on App server & Switch to  the root user

```plaintext
thor@jump_host ~$ ssh tony@stapp01

The authenticity of host 'stapp01 (172.16.238.10)' can't be established.

ECDSA key fingerprint is SHA256:BEmbndWZwybHvgohbP2hOAD0j2ErgvbEi3IpLZuaOKU.

ECDSA key fingerprint is MD5:27:54:d2:2f:14:bf:27:b6:4a:70:ac:a7:34:1d:c2:cb.

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

## 2\. Install the squid service package ( **refer to your task** )

```plaintext
[root@stapp01 ~]# yum install   squid -y

Loaded plugins: fastestmirror, ovl

Determining fastest mirrors

 * base: atl.mirrors.clouvider.net

 * extras: mirrors.gigenet.com

 * updates: mirrors.liquidweb.com

base                                                                                                                    | 3.6 kB  00:00:00    

extras                                                                                                                  | 2.9 kB  00:00:00    

updates                                                                                                                 | 2.9 kB  00:00:00    

(1/4): extras/7/x86_64/primary_db                                                                                       | 242 kB  00:00:00    

(2/4): base/7/x86_64/group_gz                                                                                           | 153 kB  00:00:00    

(3/4): base/7/x86_64/primary_db                                                                                         | 6.1 MB  00:00:00    

(4/4): updates/7/x86_64/primary_db                                                                                      | 8.8 MB  00:00:00    

Resolving Dependencies

--> Running transaction check

---> Package squid.x86_64 7:3.5.20-17.el7_9.6 will be installed

--> Processing Dependency: squid-migration-script for package: 7:squid-3.5.20-17.el7_9.6.x86_64

Verifying  : perl-Digest-1.17-245.el7.noarch                                                                                           40/41

  Verifying  : 4:perl-libs-5.16.3-299.el7_9.x86_64                                                                                       41/41

 Installed:

  squid.x86_64 7:3.5.20-17.el7_9.6                                                                                                            

 Dependency Installed:

    perl-Exporter.noarch 0:5.68-3.el7                   perl-File-Path.noarch 0:2.09-2.el7         perl-File-Temp.noarch 0:0.23.01-3.el7        

  perl-Filter.x86_64 0:1.49-3.el7                     perl-Getopt-Long.noarch 0:2.40-3.el7       perl-HTTP-Tiny.noarch 0:0.033-3.el7           

  perl-IO-Compress.noarch 0:2.061-2.el7               perl-Net-Daemon.noarch 0:0.48-5.el7        perl-PathTools.x86_64 0:3.40-5.el7           

  perl-PlRPC.noarch 0:0.2020-14.el7                   perl-Pod-Escapes.noarch 1:1.04-299.el7_9   perl-Pod-Perldoc.noarch 0:3.20-4.el7         

  perl-Pod-Simple.noarch 1:3.28-4.el7                 perl-Pod-Usage.noarch 0:1.63-3.el7         perl-Scalar-List-Utils.x86_64 0:1.27-248.el7 

  perl-Socket.x86_64 0:2.010-5.el7                    perl-Storable.x86_64 0:2.45-3.el7          perl-Text-ParseWords.noarch 0:3.29-4.el7     

  perl-Time-HiRes.x86_64 4:1.9725-3.el7               perl-Time-Local.noarch 0:1.2300-2.el7      perl-constant.noarch 0:1.27-2.el7            

  perl-libs.x86_64 4:5.16.3-299.el7_9                 perl-macros.x86_64 4:5.16.3-299.el7_9      perl-parent.noarch 1:0.225-244.el7           

  perl-podlators.noarch 0:2.5.1-3.el7                 perl-threads.x86_64 0:1.87-4.el7           perl-threads-shared.x86_64 0:1.43-6.el7      

  squid-migration-script.x86_64 7:3.5.20-17.el7_9.6 

 Complete!

[root@stapp01 ~]#
```

## 3\. Start & enable the service

```plaintext
[root@stapp01 ~]# systemctl  start  squid

[root@stapp01 ~]# systemctl  enable  squid

Created symlink from /etc/systemd/system/multi-user.target.wants/squid.service to /usr/lib/systemd/system/squid.service.

[root@stapp01 ~]#
```

## 4\. **V**alidate the task

```plaintext
[root@stapp01 ~]# systemctl status  squid

● squid.service - Squid caching proxy

   Loaded: loaded (/usr/lib/systemd/system/squid.service; enabled; vendor preset: disabled)

   Active: active (running) since Sat 2021-06-26 15:06:48 UTC; 1min 37s ago

 Main PID: 907 (squid)

   CGroup: /docker/10d9c7b82ed12cacb53bddf6953c3da3d792077999899e21e6b3f0057bffa04c/system.slice/squid.service

           ├─907 /usr/sbin/squid -f /etc/squid/squid.conf

           ├─909 (squid-1) -f /etc/squid/squid.conf

           └─910 (logfile-daemon) /var/log/squid/access.log

 

Jun 26 15:06:48 stapp01.stratos.xfusioncorp.com systemd[906]: Executing: /usr/sbin/squid -f /etc/squid/squid.conf

Jun 26 15:06:48 stapp01.stratos.xfusioncorp.com squid[907]: Squid Parent: will start 1 kids

Jun 26 15:06:48 stapp01.stratos.xfusioncorp.com squid[907]: Squid Parent: (squid-1) process 909 started

Jun 26 15:06:48 stapp01.stratos.xfusioncorp.com systemd[1]: Child 906 belongs to squid.service

Jun 26 15:06:48 stapp01.stratos.xfusioncorp.com systemd[1]: squid.service: control process exited, code=exited status=0

Jun 26 15:06:48 stapp01.stratos.xfusioncorp.com systemd[1]: squid.service got final SIGCHLD for state start

Jun 26 15:06:48 stapp01.stratos.xfusioncorp.com systemd[1]: Main PID guessed: 907

Jun 26 15:06:48 stapp01.stratos.xfusioncorp.com systemd[1]: squid.service changed start -> running

Jun 26 15:06:48 stapp01.stratos.xfusioncorp.com systemd[1]: Job squid.service/start finished, result=done

Jun 26 15:06:48 stapp01.stratos.xfusioncorp.com systemd[1]: Started Squid caching proxy.

[root@stapp01 ~]#
```

> **I have shown only for stapp01.**  You have to do this in all app server stapp01, **stapp02, stapp03.** 

Thank you so much for taking your valuable time to read

I took the initiative to learn in public and share my work with others. I tried my level best in squeezing as much information as possible in the easiest manner. Hope you learned something new today :)

**Learn** [Essential Linux Commands](https://ikunalsingh.hashnode.dev/introduction-to-essential-linux-commands)

Signup to [KodeKloud - Engineer](https://kodekloud-engineer.com/#!/login) for practicing these tasks hands-on.

In the next part of this blog, we will study 👇

<iframe src="https://giphy.com/embed/P8yl3NNvMc2sViu8WI" class="giphy-embed" width="480" height="270"></iframe>

[via GIPHY](https://giphy.com/gifs/DefyTVNetwork-duck-dynasty-defytv-P8yl3NNvMc2sViu8WI)