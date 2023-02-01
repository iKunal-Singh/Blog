# Create A Cron Job

# Infrastructure

[Infrastructure Details](https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details)

[Infrastructure Diagram](https://lucid.app/lucidchart/58e22de2-c446-4b49-ae0f-db79a3318e97/view?page=0_0#)

> Signup to [KodeKloud - Engineer](https://kodekloud-engineer.com/#!/login) for practicing this task hands-on.

# Task Details

The Nautilus system admins team has prepared scripts to automate several day-to-day tasks. They want them to be deployed on all app servers in Stratos DC on a set schedule. Before that, they need to test similar functionality with a sample cron job. Therefore, perform the steps below:

a. Install cronie package on all Nautilus app servers and start crond service.

b. Add a cron \**/5\*\*\** \* echo hello &gt; /tmp/cron\_text for root user.

# **Solution:**

## 1\. Login on the App server as per the task

| **Server Name** | **IP** | **Hostname** | **User** | **Password** | **Purpose** |
| --- | --- | --- | --- | --- | --- |
| stapp01 | 172.16.238.10 | stapp01.stratos.xfusioncorp.com | tony | Ir0nM@n | Nautilus App 1 |
| stapp02 | 172.16.238.11 | stapp02.stratos.xfusioncorp.com | steve | Am3ric@ | Nautilus App 2 |
| stapp03 | 172.16.238.12 | stapp03.stratos.xfusioncorp.com | banner | BigGr33n | Nautilus App 3 |

```plaintext
thor@jump_host ~$ ssh tony@stapp01

The authenticity of host 'stapp01 (172.16.238.10)' can't be established.

ECDSA key fingerprint is SHA256:HfSF2lgWTKxzOCOIseoaLzUMcUFiflwRYS+5VfEeADA.

ECDSA key fingerprint is MD5:90:8c:06:3d:71:b2:de:80:4d:45:e2:59:45:77:d2:7c.

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

> A brief description of the commands "ssh" and "sudo su-" is given in Essential [Linux Commands](https://ikunalsingh.hashnode.dev/introduction-to-essential-linux-commands).

## 2. Install the cronie package on the servers.

```plaintext
[root@stapp01 ~]# yum install cronie -y

Loaded plugins: fastestmirror, ovl

Determining fastest mirrors

 * base: mirror.genesisadaptive.com

 * extras: mirrors.gigenet.com

 * updates: centos.mirror.lstn.net

base                                                                                         | 3.6 kB  00:00:00    

extras                                                                                       | 2.9 kB  00:00:00    

updates                                                                                      | 2.9 kB  00:00:00    

(1/4): base/7/x86_64/group_gz                                                                | 153 kB  00:00:00    

(2/4): extras/7/x86_64/primary_db                                                            | 242 kB  00:00:00    

(3/4): updates/7/x86_64/primary_db                                                           | 8.8 MB  00:00:00    

(4/4): base/7/x86_64/primary_db                                                              | 6.1 MB  00:00:00    

Resolving Dependencies

--> Running transaction check

---> Package cronie.x86_64 0:1.4.11-23.el7 will be installed

--> Processing Dependency: dailyjobs for package: cronie-1.4.11-23.el7.x86_64

--> Running transaction check

---> Package cronie-anacron.x86_64 0:1.4.11-23.el7 will be installed

--> Processing Dependency: crontabs for package: cronie-anacron-1.4.11-23.el7.x86_64

--> Running transaction check

---> Package crontabs.noarch 0:1.11-6.20121102git.el7 will be installed

--> Finished Dependency Resolution

 Dependencies Resolved

 ====================================================================================================================

 Package                      Arch                 Version                                 Repository          Size

====================================================================================================================

Installing:

 cronie                       x86_64               1.4.11-23.el7                           base                92 k

Installing for dependencies:

 cronie-anacron               x86_64               1.4.11-23.el7                           base                36 k

 crontabs                     noarch               1.11-6.20121102git.el7                  base                13 k

 Transaction Summary

====================================================================================================================

Install  1 Package (+2 Dependent packages)

 Total download size: 141 k

Installed size: 260 k

Downloading packages:

(1/3): cronie-anacron-1.4.11-23.el7.x86_64.rpm                                               |  36 kB  00:00:00    

(2/3): cronie-1.4.11-23.el7.x86_64.rpm                                                       |  92 kB  00:00:00    

(3/3): crontabs-1.11-6.20121102git.el7.noarch.rpm                                            |  13 kB  00:00:00    

--------------------------------------------------------------------------------------------------------------------

Total                                                                               579 kB/s | 141 kB  00:00:00    

Running transaction check

Running transaction test

Transaction test succeeded

Running transaction

  Installing : cronie-anacron-1.4.11-23.el7.x86_64                                                              1/3

  Installing : cronie-1.4.11-23.el7.x86_64                                                                      2/3

  Installing : crontabs-1.11-6.20121102git.el7.noarch                                                           3/3

  Verifying  : cronie-1.4.11-23.el7.x86_64                                                                      1/3

  Verifying  : cronie-anacron-1.4.11-23.el7.x86_64                                                              2/3

  Verifying  : crontabs-1.11-6.20121102git.el7.noarch                                                           3/3

 Installed:

  cronie.x86_64 0:1.4.11-23.el7                                                                                    

 Dependency Installed:

  cronie-anacron.x86_64 0:1.4.11-23.el7                   crontabs.noarch 0:1.11-6.20121102git.el7                 

 Complete!

[root@stapp01 ~]# 
```

### yum install cronie -y

It is used to install the "cronie" package on a Linux system using the "yum" package manager.

"**yum**" is a package manager for Red Hat-based systems, such as Fedora and CentOS, and is used to install, update, and manage software packages. "**cronie**" is the package that provides the cron daemon, which is responsible for executing scheduled tasks on the system.

The "-y" option tells "yum" to automatically answer "yes" to any prompts, so the installation process will run without any user interaction. The "cronie" package will be installed, along with any dependencies, and will be ready to use for scheduling cron jobs.

## 3.  Start the cron service & check the status 

```plaintext
[root@stapp01 ~]# systemctl start crond.service

[root@stapp01 ~]# systemctl status crond.service

● crond.service - Command Scheduler

   Loaded: loaded (/usr/lib/systemd/system/crond.service; enabled; vendor preset: enabled)

   Active: active (running) since Wed 2021-06-23 09:48:40 UTC; 7s ago

 Main PID: 867 (crond)

   CGroup: /docker/1e182a61c844a4126c21ead9f17af0fd695bfb11e177c1b7d3be38d07ec49c8a/system.slice/crond.service

           └─867 /usr/sbin/crond -n

 

Jun 23 09:48:40 stapp01.stratos.xfusioncorp.com systemd[1]: About to execute: /usr/sbin/crond -n $CRONDARGS

Jun 23 09:48:40 stapp01.stratos.xfusioncorp.com systemd[1]: Forked /usr/sbin/crond as 867

Jun 23 09:48:40 stapp01.stratos.xfusioncorp.com crond[867]: (CRON) INFO (Syslog will be used instead of sendmail.)

Jun 23 09:48:40 stapp01.stratos.xfusioncorp.com systemd[1]: crond.service changed dead -> running

Jun 23 09:48:40 stapp01.stratos.xfusioncorp.com crond[867]: (CRON) INFO (RANDOM_DELAY will be scaled with fact...d.)

Jun 23 09:48:40 stapp01.stratos.xfusioncorp.com systemd[1]: Job crond.service/start finished, result=done

Jun 23 09:48:40 stapp01.stratos.xfusioncorp.com crond[867]: (CRON) INFO (running with inotify support)

Jun 23 09:48:40 stapp01.stratos.xfusioncorp.com systemd[1]: Started Command Scheduler.

Jun 23 09:48:40 stapp01.stratos.xfusioncorp.com systemd[867]: Executing: /usr/sbin/crond -n

Hint: Some lines were ellipsized, use -l to show in full.

[root@stapp01 ~]#
```

## systemctl start crond.service | status crond.service

The "**systemctl start crond.service**" command is used to start the cron daemon service in Linux systems using the systemd init system.

"systemctl" is a command line utility used to manage systemd services, and "crond.service" is the name of the service that provides the cron daemon.

The "**start**" subcommand tells "systemctl" to start the crond.service, which will allow the cron daemon to start running and executing scheduled tasks on the system.

The "**status**" subcommand tells "systemctl" to display the current status of the crond.service, including whether it is running or not, and any relevant error messages or status information.

## 4.  Create a cronjob  as per the task for the root user

```plaintext
root@stapp01 ~]# crontab -e

no crontab for root - using an empty one

crontab: installing new crontab

[root@stapp01 ~]#
```

### **crontab -e**

The command is used to manage cron jobs, which are scheduled tasks that run automatically at specified times on a Linux system. The "**\-e**" option tells "**crontab**" to open the cron table for editing, allowing you to create, modify, or delete cron jobs for the current user.

Cron jobs are specified in a specific format in the cron table, with each line representing a separate job and consisting of six fields: minute, hour, day of the month, month, day of the week, and the command to be executed.

"**crontab -e**" is a useful tool for setting up automated tasks on your system, allowing you to schedule tasks such as running backups, sending emails, or updating software.

## **5.**  Check the cron job for the root user

```plaintext
[root@stapp01 ~]# crontab -l

*/5 * * * * echo hello > /tmp/cron_text

[root@stapp01 ~]#
```

### crontab -l

It is used to display a list of all the cron jobs for the current user in Linux. It shows the schedule and the command for each job, which allows you to view and manage your cron tasks.

## **6.**  Validate the **cron\_text** file

```plaintext
[root@stapp01 ~]# ll /tmp/

total 8

-rw-r--r-- 1 root root   6 Jun 23 09:55 cron_text

-rwx------ 1 root root 836 Aug  1  2019 ks-script-rnBCJB

-rw------- 1 root root   0 Aug  1  2019 yum.log

[root@stapp01 ~]#
```

> I have shown only for **stapp01**. You have to do this in all app server stapp01,**stapp02, stapp03.**

Thank you so much for taking your valuable time to read

I took the initiative to learn in public and share my work with others. I tried my level best in squeezing as much information as possible in the easiest manner.

Hope you learned something new today :)

**Learn** [Essential Linux Commands](https://ikunalsingh.hashnode.dev/introduction-to-essential-linux-commands)

Signup to [KodeKloud - Engineer](https://kodekloud-engineer.com/#!/login) for practicing these tasks hands-on.

In the next part of this blog, we will study 👇

<iframe src="https://giphy.com/embed/TIz6SZ8ByHB0nq7laI" class="giphy-embed" width="480" height="392"></iframe>

[via GIPHY](https://giphy.com/gifs/nfl-rookie-rookies-2018-TIz6SZ8ByHB0nq7laI)