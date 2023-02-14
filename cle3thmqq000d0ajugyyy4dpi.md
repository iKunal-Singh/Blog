# Linux User Expiry

# Infrastructure

[Infrastructure Details](https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details)

[Infrastructure Diagram](https://lucid.app/lucidchart/58e22de2-c446-4b49-ae0f-db79a3318e97/view?page=0_0#)

> Signup to [KodeKloud - Engineer](https://kodekloud-engineer.com/#!/login) for practicing this task hands-on.

# Task Details

A developer mark has been assigned to the Nautilus project temporarily as a backup resource. As a temporary resource for this project, we need a temporary user for the mark. It’s a good idea to create a user with a set expiration date so that the user won't be able to access servers beyond that point.

Therefore, create a user named mark on App Server 1. Set expiry date to 2021-04-15 in Stratos Datacenter. Make sure the user is created as per standard and is in lowercase.

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

> A brief description of the "ssh" , "sudo su-" and other commands is given in [Essential Linux Commands](https://ikunalsingh.hashnode.dev/introduction-to-essential-linux-commands).

## 2\. 1st check user exists on the server 

```plaintext
[root@stapp01 ~]# id  mark

id: mark: no such user

[root@stapp01 ~]#
```

## 3\. If the user is not found then you **create a user  as per given in your task** 

```plaintext
[root@stapp01 ~]# adduser mark

[root@stapp01 ~]# id  mark

uid=1002(mark) gid=1002(mark) groups=1002(mark)

[root@stapp01 ~]#  
```

## 4. Validate user is created successfully  with a default expiry date 

```plaintext
[root@stapp01 ~]# chage -l mark

Last password change                                    : Jun 26, 2021

Password expires                                        : never

Password inactive                                       : never

Account expires                                         : never

Minimum number of days between password change          : 0

Maximum number of days between password change          : 99999

Number of days of warning before password expires       : 7

[root@stapp01 ~]#
```

### `chage -l mark`

The `chage -l mark`a command is used to display the password and account aging information for the user "mark", such as when the password was last changed when it will expire, and how many days before expiration the user will be warned.

## 5\. Change the default expires to date as per your task

```plaintext
[root@stapp01 ~]# chage -E 2021-04-15 mark

[root@stapp01 ~]#
```

### `chage -E`

The `chage -E 2021-04-15 mark` command is used to set the account expiration date for the user 'mark' to April 15th, 2021 in Linux.

## 6\. Validate changes

```plaintext
[root@stapp01 ~]# chage -l mark

Last password change                                    : Jun 26, 2021

Password expires                                        : never

Password inactive                                       : never

Account expires                                         : Apr 15, 2021

Minimum number of days between password change          : 0

Maximum number of days between password change          : 99999

Number of days of warning before password expires       : 7

[root@stapp01 ~]#
```

Thank you so much for taking your valuable time to read

I took the initiative to learn in public and share my work with others. I tried my level best in squeezing as much information as possible in the easiest manner. Hope you learned something new today :)

**Learn** [Essential Linux Commands](https://ikunalsingh.hashnode.dev/introduction-to-essential-linux-commands)

Signup to [KodeKloud - Engineer](https://kodekloud-engineer.com/#!/login) for practicing these tasks hands-on.

In the next part of this blog, we will study 👇

<iframe src="https://giphy.com/embed/cfuL5gqFDreXxkWQ4o" class="giphy-embed" width="480" height="480"></iframe>

[via GIPHY](https://giphy.com/gifs/cat-cool-cats-cfuL5gqFDreXxkWQ4o)