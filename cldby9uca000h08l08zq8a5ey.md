# Linux SSH Authentication

# Introduction to Linux SSH Authentication

SSH (Secure Shell) authentication is the process of verifying a user's identity when connecting to a remote machine over SSH. SSH uses a combination of a username and password or a private and public key pair to authenticate the user. This ensures that only authorized users can connect to the remote machine and access its resources.

One of the most common methods of SSH authentication is using a private and public key pair. The user generates a key pair on their local machine, and the public key is then copied to the remote device. When the user connects to the remote device, they use their private key to prove their identity. Public key authentication is considered more secure than password-based authentication because it uses a unique key pair instead of a password that can be guessed or cracked.

SSH also supports other authentication methods such as Kerberos and two-factor authentication. Kerberos is a network authentication protocol that uses tickets to authenticate users. Two-factor authentication adds a layer of security by requiring users to provide two forms of identification, such as a password and a security token.

It's important to note that SSH authentication should be properly configured to ensure the security of the remote connection. It's recommended to use public key authentication and to disable password-based authentication if possible. Additionally, it's important to keep the SSH server and the client software up to date with the latest security patches.

In summary, SSH authentication in Linux is the process of verifying a user's identity when connecting to a remote machine over SSH, it can be done using a combination of a username and password, a private and public key pair, or other methods such as Kerberos and two-factor authentication. It's important to properly configure SSH authentication for security and keep the SSH server and the client software up to date with the latest security patches.

# Infrastructure

[Infrastructure Details](https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details)

[Infrastructure Diagram](https://lucid.app/lucidchart/58e22de2-c446-4b49-ae0f-db79a3318e97/view?page=0_0#)

> Signup to [KodeKloud - Engineer](https://kodekloud-engineer.com/#!/login) for practicing this task hands-on.

# Task Details

The system admins team of xFusionCorp Industries has set up some scripts on jump host that run on regular intervals and perform operations on all app servers in Stratos Datacenter. To make these scripts work properly we need to make sure thor user on the jump host has password-less SSH access to all app servers through their respective sudo users. Based on the requirements, perform the following:

Set up a password-less authentication from user thor on jump host to all app servers through their respective sudo users.

> Perform the below commands based on your question server, user name & other details might differ. So please read the task carefully before executing it. All the Best 👍

# **Solution:**

| **Server Name** | **IP** | **Hostname** | **User** | **Password** | **Purpose** |
| --- | --- | --- | --- | --- | --- |
| stapp01 | 172.16.238.10 | stapp01.stratos.xfusioncorp.com | tony | Ir0nM@n | Nautilus App 1 |
| stapp02 | 172.16.238.11 | stapp02.stratos.xfusioncorp.com | steve | Am3ric@ | Nautilus App 2 |
| stapp03 | 172.16.238.12 | stapp03.stratos.xfusioncorp.com | banner | BigGr33n | Nautilus App 3 |

## 1\. Create a key

```plaintext
thor@jump_host /$ ssh-keygen -t rsa

Generating public/private rsa key pair.

Enter file in which to save the key (/home/thor/.ssh/id_rsa):

Created directory '/home/thor/.ssh'.

Enter passphrase (empty for no passphrase):

Enter same passphrase again:

Your identification has been saved in /home/thor/.ssh/id_rsa.

Your public key has been saved in /home/thor/.ssh/id_rsa.pub.

The key fingerprint is:

SHA256:qCu+Pf9OsZJsta1rySmK7/6aqSShPS6bSkjbqp2RA5M thor@jump_host.stratos.xfusioncorp.com

The key's randomart image is:

+---[RSA 3072]----+
|                 |
|                 |
|                 |
| .     .         |
|E.    . S        |
|+=o. o o =       |
|+oB.. =.+o.      |
|o*oB.=.o=.       |
|B=OB@*o==.       |
+----[SHA256]-----+

thor@jump_host /$
```

### ssh-keygen

"ssh-keygen -t rsa" command is used to generate a new SSH key pair. The "ssh-keygen" command is used to create, manage and convert SSH keys, and the option "-t rsa" is used to specify the type of the key to generate, in this case, RSA.

RSA is the algorithm used for public-key encryption and digital signatures.

It's important to note that the private key should be kept safe, and never shared with anyone. The public key can be shared with others and added to remote servers to allow for passwordless login.

**In summary,** "ssh-keygen -t rsa" is a command in Linux that is used to generate a new SSH key pair, it prompts for a file name and passphrase to encrypt the private key, RSA is the algorithm used to generate the key and the private key should be kept safe, while the public key can be shared with others to allow for passwordless login.

## 2\. Copy the public key on all the APP servers

```plaintext
thor@jump_host /$ ssh-copy-id  tony@stapp01

/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/thor/.ssh/id_rsa.pub"

The authenticity of host 'stapp01 (172.16.238.10)' can't be established.

ECDSA key fingerprint is SHA256:xOTel/1W6Mx6p3t62XZazZrYXBOpWE/zS9+OL2HeNgY.

ECDSA key fingerprint is MD5:a2:2e:85:cc:9e:0c:da:b0:f8:d9:dc:03:c2:3f:27:50.

Are you sure you want to continue connecting (yes/no)? yes

/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed

/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys

tony@stapp01's password:

 Number of key(s) added: 1

 Now try logging into the machine, with:   "ssh 'tony@stapp01'"

and check to make sure that only the key(s) you wanted were added.

 thor@jump_host /$
```

## 3.  Now try logging into the APP server without a password.

```plaintext
thor@jump_host /$ ssh tony@stapp01
[tony@stapp01 ~]$
```

> I have showed only for **stapp01**. You have to do steps 2 -3 in all app server stapp01,**stapp02, stapp03.**

Thank you so much for taking your valuable time to read

I took the initiative to learn in public and share my work with others. I tried my level best in squeezing as much information as possible in the easiest manner. Hope you learned something new today :)

**Learn** [Essential Linux Commands](https://ikunalsingh.hashnode.dev/introduction-to-essential-linux-commands)

Signup to [KodeKloud - Engineer](https://kodekloud-engineer.com/#!/login) for practicing these tasks hands-on.

In the next part of this blog, we will study 👇

[Linux String Substitute](https://ikunalsingh.hashnode.dev/linux-string-substitute)

<iframe src="https://giphy.com/embed/BoHCeLmEKytt7oFxyR" class="giphy-embed" width="480" height="384"></iframe>

[via GIPHY](https://giphy.com/gifs/tiktok-bye-positive-dj-khaled-BoHCeLmEKytt7oFxyR)