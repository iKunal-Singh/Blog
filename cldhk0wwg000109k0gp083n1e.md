# Linux String Substitute

# Infrastructure

[Infrastructure Details](https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details)

[Infrastructure Diagram](https://lucid.app/lucidchart/58e22de2-c446-4b49-ae0f-db79a3318e97/view?page=0_0#)

> Signup to [KodeKloud - Engineer](https://kodekloud-engineer.com/#!/login) for practicing this task hands-on.

# Task Details

There is some data on Nautilus App Server 3 in Stratos DC. Data needs to be altered in several of the files. On Nautilus App Server 3, modify the /home/BSD.txt file as per the details given below:

a. Delete all lines containing word software and save results in /home/BSD\_DELETE.txt file. (Please be aware of case sensitivity)

b. Replace all occurrences of the word and to is and save results in /home/BSD\_REPLACE.txt file.

> Note: Let's say you are asked to replace the word to with from. In that case, make sure not to alter any terms containing this string; for example upto, contributor, etc.
> 
> Perform the below commands based on your question server, user name & other details might differ. So please read the task carefully before executing it. All the Best 👍

# **Solution:**

| **Server Name** | **IP** | **Hostname** | **User** | **Password** | **Purpose** |
| --- | --- | --- | --- | --- | --- |
| stapp01 | 172.16.238.10 | stapp01.stratos.xfusioncorp.com | tony | Ir0nM@n | Nautilus App 1 |
| stapp02 | 172.16.238.11 | stapp02.stratos.xfusioncorp.com | steve | Am3ric@ | Nautilus App 2 |
| stapp03 | 172.16.238.12 | stapp03.stratos.xfusioncorp.com | banner | BigGr33n | Nautilus App 3 |

## 1\. Login on the App server as per the task

```plaintext
thor@jump_host ~$ ssh banner@stapp03

The authenticity of host 'stapp03 (172.16.238.12)' can't be established.

ECDSA key fingerprint is SHA256:jQBzPUeKPaiyjrpKdy6e4PG/2IswOUaZYIogb7o1SHE.

ECDSA key fingerprint is MD5:1f:11:06:a6:ec:6c:f5:37:38:31:79:ad:a6:70:94:37.

Are you sure you want to continue connecting (yes/no)? yes

Warning: Permanently added 'stapp03,172.16.238.12' (ECDSA) to the list of known hosts.

banner@stapp03's password: BigGr33n

[banner@stapp03 ~]$ sudo su -

 We trust you have received the usual lecture from the local System

Administrator. It usually boils down to these three things:

     #1) Respect the privacy of others.

    #2) Think before you type.

    #3) With great power comes great responsibility.

 [sudo] password for banner: BigGr33n

[root@stapp03 ~]#
```

> A brief description of the commands "ssh" and "sudo su-" is given in Essential [Linux Commands](https://ikunalsingh.hashnode.dev/introduction-to-essential-linux-commands).

## 2\. List the existing file and cat the word given in the task which needs to delete.

```plaintext
[root@stapp03 ~]# ll /home/

total 20

drwx------ 1 ansible ansible 4096 Oct 15  2019 ansible

drwx------ 1 banner  banner  4096 Jan 25  2020 banner

-rw-r--r-- 1 root    root    9919 Jun 27 12:27 BSD.txt

[root@stapp03 ~]#

[root@stapp03 ~]# cat /home/BSD.txt |grep software

3. All advertising materials mentioning features or use of this software

   This product includes software developed by the <organization>.

   derived from this software without specific prior written permission.

3. All advertising materials mentioning features or use of this software

   This product includes software developed by the <organization>.

   derived from this software without specific prior written permission.

3. All advertising materials mentioning features or use of this software

   This product includes software developed by the <organization>.

   derived from this software without specific prior written permission.

3. All advertising materials mentioning features or use of this software

   This product includes software developed by the <organization>.

   derived from this software without specific prior written permission.

3. All advertising materials mentioning features or use of this software

   This product includes software developed by the <organization>.

   derived from this software without specific prior written permission.

3. All advertising materials mentioning features or use of this software

   This product includes software developed by the <organization>.

   derived from this software without specific prior written permission.

[root@stapp03 ~]#
```

> A brief description of the commands "cat" and "grep" is given in Essential [Linux Commands](https://ikunalsingh.hashnode.dev/introduction-to-essential-linux-commands).

> As per the task, the BSD.txt file should be intact. But using "**sed -i**" will change the file in place. So if you want to use "**sed -i"**, make the copy of the BSD.txt to respective files and do the changes on newly created files.
> 
> Another option is which task required is just applied "sed" on the original file and redirects the output to respective files. This won't affect the original place. As shown below.

## 3\. Delete all lines containing word **software** and save results in /home/BSD\_DELETE.txt file

```plaintext
[root@stapp03 ~]# sed '/\<software\>/d' /home/BSD.txt > /home/BSD_DELETE.txt

[root@stapp03 ~]#

[root@stapp03 ~]# cat /home/BSD_DELETE.txt |grep software

[root@stapp03 ~]#
```

### **sed '/\\&lt;software\\&gt;/d' /home/BSD.txt &gt; /home/BSD\_DELETE.txt**

The "sed" command in the given example is used to perform a string substitution by deleting all instances of the word **"software"** in the file "/home/BSD.txt" and writing the modified output to a new file "/home/BSD\_DELETE.txt". The syntax **"/&lt;software&gt;/d"** is used to search for the exact word "software" and the **"d"** flag specifies deleting the line that contains the matching word. The **"&gt;"** symbol is used to redirect the output of the command to the specified file.

## 4.  Cat the output for the word which is going to be replaced

```plaintext
[root@stapp03 ~]# cat /home/BSD.txt |grep and
Redistribution and use in source and binary forms, with or without

   notice, this list of conditions and the following disclaimer.

   notice, this list of conditions and the following disclaimer in the

   documentation and/or other materials provided with the distribution.

Redistribution and use in source and binary forms, with or without

   notice, this list of conditions and the following disclaimer.

   notice, this list of conditions and the following disclaimer in the

   documentation and/or other materials provided with the distribution.

Redistribution and use in source and binary forms, with or without

   notice, this list of conditions and the following disclaimer.

   notice, this list of conditions and the following disclaimer in the

   documentation and/or other materials provided with the distribution.

Redistribution and use in source and binary forms, with or without

   notice, this list of conditions and the following disclaimer.

   notice, this list of conditions and the following disclaimer in the

   documentation and/or other materials provided with the distribution.

Redistribution and use in source and binary forms, with or without

   notice, this list of conditions and the following disclaimer.

   notice, this list of conditions and the following disclaimer in the

   documentation and/or other materials provided with the distribution.

Redistribution and use in source and binary forms, with or without

   notice, this list of conditions and the following disclaimer.

   notice, this list of conditions and the following disclaimer in the

   documentation and/or other materials provided with the distribution.

[root@stapp03 ~]#
```

## 5.  Now replace  all occurrences of the word and redirect to a new file

```plaintext
[root@stapp03 ~]# sed 's/\band\b/is/g' /home/BSD.txt > /home/BSD_REPLACE.txt

[root@stapp03 ~]#
```

## 6.  Validate the word replaced successfully

```plaintext
[root@stapp03 ~]# cat /home/BSD_REPLACE.txt |grep and

[root@stapp03 ~]# cat /home/BSD_REPLACE.txt |grep is

Redistribution is use in source is binary forms, with or without

1. Redistributions of source code must retain the above copyright

   notice, this list of conditions is the following disclaimer.

2. Redistributions in binary form must reproduce the above copyright

   notice, this list of conditions is the following disclaimer in the

   documentation is/or other materials provided with the distribution.

3. All advertising materials mentioning features or use of this software

   must display the following acknowledgement:

   This product includes software developed by the <organization>.

   derived from this software without specific prior written permission.

Redistribution is use in source is binary forms, with or without

1. Redistributions of source code must retain the above copyright

   notice, this list of conditions is the following disclaimer.

2. Redistributions in binary form must reproduce the above copyright

   notice, this list of conditions is the following disclaimer in the

   documentation is/or other materials provided with the distribution.

3. All advertising materials mentioning features or use of this software

   must display the following acknowledgement:

   This product includes software developed by the <organization>.

   derived from this software without specific prior written permission.

Redistribution is use in source is binary forms, with or without

1. Redistributions of source code must retain the above copyright

   notice, this list of conditions is the following disclaimer.

2. Redistributions in binary form must reproduce the above copyright

   notice, this list of conditions is the following disclaimer in the

   documentation is/or other materials provided with the distribution.

3. All advertising materials mentioning features or use of this software

   must display the following acknowledgement:

   This product includes software developed by the <organization>.

   derived from this software without specific prior written permission.

Redistribution is use in source is binary forms, with or without

1. Redistributions of source code must retain the above copyright

   notice, this list of conditions is the following disclaimer.

2. Redistributions in binary form must reproduce the above copyright

   notice, this list of conditions is the following disclaimer in the

   documentation is/or other materials provided with the distribution.

3. All advertising materials mentioning features or use of this software

   must display the following acknowledgement:

   This product includes software developed by the <organization>.

   derived from this software without specific prior written permission.

Redistribution is use in source is binary forms, with or without

1. Redistributions of source code must retain the above copyright

   notice, this list of conditions is the following disclaimer.

2. Redistributions in binary form must reproduce the above copyright

   notice, this list of conditions is the following disclaimer in the

   documentation is/or other materials provided with the distribution.

3. All advertising materials mentioning features or use of this software

   must display the following acknowledgement:

   This product includes software developed by the <organization>.

   derived from this software without specific prior written permission.

Redistribution is use in source is binary forms, with or without

1. Redistributions of source code must retain the above copyright

   notice, this list of conditions is the following disclaimer.

2. Redistributions in binary form must reproduce the above copyright

   notice, this list of conditions is the following disclaimer in the

   documentation is/or other materials provided with the distribution.

3. All advertising materials mentioning features or use of this software

   must display the following acknowledgement:

   This product includes software developed by the <organization>.

   derived from this software without specific prior written permission.

[root@stapp03 ~]#
```

Thank you so much for taking your valuable time to read

I took the initiative to learn in public and share my work with others. I tried my level best in squeezing as much information as possible in the easiest manner. Hope you learned something new today :)

**Learn** [Essential Linux Commands](https://ikunalsingh.hashnode.dev/introduction-to-essential-linux-commands)

Signup to [KodeKloud - Engineer](https://kodekloud-engineer.com/#!/login) for practicing these tasks hands-on.

In the next part of this blog, we will study 👇

<iframe src="https://giphy.com/embed/9u4TXfoIM7wQHnfR1p" class="giphy-embed" width="480" height="270"></iframe>

[via GIPHY](https://giphy.com/gifs/fallontonight-jimmy-fallon-see-ya-tonightshow-9u4TXfoIM7wQHnfR1p)