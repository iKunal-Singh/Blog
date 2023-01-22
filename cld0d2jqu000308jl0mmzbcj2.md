# Linux File Permissions

# Infrastructure

[Infrastructure Details](https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details)

[Infrastructure Diagram](https://lucid.app/lucidchart/58e22de2-c446-4b49-ae0f-db79a3318e97/view?page=0_0#)

> Signup to [KodeKloud - Engineer](https://kodekloud-engineer.com/#!/login) for practicing this task hands-on.

# Task Details

There are new requirements to automate a backup process that was performed manually by the xFusionCorp Industries system admins team earlier. To automate this task, the team has developed a new bash script xfusioncorp. sh. They have already copied the script on all required servers, however, they did not make it executable on one of the app server i.e App Server 1 in Stratos Datacenter.

Please give executable permissions to the/tmp/xfusioncorp. sh script on App Server 1. Also, make sure every user can execute it.

> Perform the below commands based on your question server,  user name & other details might differ. So please read the task carefully before executing it. All the Best 👍

# **Solution:**

## 1\. Login on the App server as per the task

| **Server Name** | **IP** | **Hostname** | **User** | **Password** | **Purpose** |
| --- | --- | --- | --- | --- | --- |
| stapp01 | 172.16.238.10 | stapp01.stratos.xfusioncorp.com | tony | Ir0nM@n | Nautilus App 1 |
| stapp02 | 172.16.238.11 | stapp02.stratos.xfusioncorp.com | steve | Am3ric@ | Nautilus App 2 |
| stapp03 | 172.16.238.12 | stapp03.stratos.xfusioncorp.com | banner | BigGr33n | Nautilus App 3 |

```plaintext
thor@jump_host /$ ssh tony@stapp01

The authenticity of host 'stapp01 (172.16.238.10)' can't be established.

ECDSA key fingerprint is SHA256:RIRt2SqEVQ3yKDQ+cX5QLPw7mJNJhXUcT5Dpsy4GU1U.
ECDSA key fingerprint is MD5:bd:f7:14:9e:c0:fd:41:0d:2d:e4:30:47:8a:34:35:ae.

Are you sure you want to continue connecting (yes/no)? yes

Warning: Permanently added 'stapp01,172.16.238.10' (ECDSA) to the list of known hosts.

tony@stapp01's password: Ir0nM@n

[tony@stapp01 ~]$ sudo su -

 We trust you have received the usual lecture from the local System

Administrator. It usually boils down to these three things:

     #1) Respect the privacy of others.

    #2) Think before you type.

    #3) With great power comes great responsibility.

 [sudo] password for tony: Ir0nM@n
```

### **Secure Shell (SSH)**

Secure Shell (SSH) is a way for you to securely, remotely access, and control a computer or network from another computer and another location. SSH uses encryption to protect your login information and the data that is transferred between the two computers, making it much more secure than other methods of remote access. It is commonly used to remotely manage servers, network devices, and other systems.

In simple terms, SSH allows you to remotely access and control a computer as if you were sitting in front of it, in a secure way.

**For example**, Imagine you have a website hosted on a server at your office, and you want to make updates to it from your home. Instead of physically going to the office to make the updates, you can use SSH to remotely access the server and make the updates from your home computer. SSH encrypts the connection between your home computer and the server so that any information exchanged between the two is secure and cannot be intercepted by anyone else.

### **sudo su -**

"sudo su -" command allows a user to temporarily gain the privileges of the superuser, also known as the "root" user. The "sudo" part of the command grants the user temporary superuser privileges, and the "su -" part allows the user to switch to the root user account.

**For example**, imagine you are a regular user on a Linux machine and you need to install a new software package. Normally, you would not have permission to do so because the package manager is restricted to the superuser. By using the command "sudo su -", you can temporarily gain superuser privileges and install the package as root.

It's important to note that using the root account can be a security risk, it's recommended to use the least privileged account whenever possible, and use the "sudo" command to temporarily gain superuser privileges only when necessary.

**In summary**, **"sudo su -"** is a command that allows a regular user to temporarily gain superuser privileges, which can be useful for performing certain tasks that are restricted to the root user. However, it's important to use it carefully and with caution.

## **2\. List the file's existing permission**

```plaintext
[root@stapp01 ~]# ls -ltr /tmp/xfusioncorp.sh

---------- 1 root root 40 Jun 17 09:02 /tmp/xfusioncorp.sh  

[root@stapp01 ~]#
```

### **ls -ltr**

"ls -ltr" command is used to list the files and directories in a directory, with additional information, and in a specific order. The **"ls" command stands for "list"** and it is used to display the contents of a directory. The options "l", "t" and "r" added to the command change the way the files and directories are displayed.

The option **"l" stands for "long format"** and it shows additional information about the files and directories, such as permissions, owner, size, and date of last modification.

The option **"t" stands for "sort by modification time"** and it will sort the files and directories based on when they were last modified.

The option **"r" stands for "reverse"** and it will reverse the order of the files and directories.

**For example**, if you are in a directory and you want to see the contents of the directory with additional information sorted by the time they were last modified in reverse order, you can use the command "ls -ltr". This will display all the files and directories in the directory, including hidden files, with details like permissions, owner, size, and date of last modification and will be sorted in reverse order of modification time.

**In summary**, "ls -ltr" is a command that lists the files and directories in a directory with additional information, sorted by the time they were last modified in reverse order.

## 3\. Giving all Other users execute permissions

```plaintext
[root@stapp01 ~]# chmod o+rx /tmp/xfusioncorp.sh

[root@stapp01 ~]#
```

> **Bash** is the interpreter that is going to execute the script and the interpreter needs to read the script so even if we have given it only executable permission the interpreter i.e bash will not be able to execute it so we had to give it read permission as well along with execute permission.

### **chmod o+rx**

"chmod o+rx" command is used to change the permissions of a file or directory. The **"chmod" command stands for "change mode"** and it is used to modify the permissions of a file or directory.

**In this specific case**, **"o+rx"** is an argument passed to the chmod command. The **"o" stands for "others", " + " means "add" and "rx" stands for read and execute permission**. This means that the command is adding read and execute permission to the "others" category of users for the specified file or directory.

> There are three types of permissions: read (r), write (w), and execute (x). These permissions can be assigned to three different classes of users: the file's owner, the owner's group, and all other users.

## **4\. Verify the file permissions and execute the script from the user**

```plaintext
[root@stapp01 ~]#ls -lrt /tmp/xfusioncorp.sh

-------r-x 1 root root 40 Jun 17 09:25 /tmp/xfusioncorp.sh 

[root@stapp01 ~]# exit

logout

[tony@stapp01 ~]$ sh /tmp/xfusioncorp.sh

Welcome To KodeKloud

[tony@stapp01 ~]$
```

Thank you so much for taking your valuable time to read

I took the initiative to learn in public and share my work with others. I tried my level best in squeezing as much information as possible in the easiest manner.

Hope you learned something new today :)

**Learn** [Essential Linux Commands](https://ikunalsingh.hashnode.dev/introduction-to-essential-linux-commands)

Signup to [KodeKloud - Engineer](https://kodekloud-engineer.com/#!/login) for practicing these tasks hands-on.

In the next part of this blog, we will study 👇

[Create a Linux user with Non-Interactive Shell](https://ikunalsingh.hashnode.dev/linux-user-with-non-interactive-shell)

<iframe src="https://giphy.com/embed/kwCE5QctkD6rEdR3q6" class="giphy-embed" width="480" height="355"></iframe>

[via GIPHY](https://giphy.com/gifs/showtimesports-showtime-sports-kenny-smith-kwCE5QctkD6rEdR3q6)