# Linux Setups

sudo is a program for Unix-like computer operating systems that allows users to run programs with the security privileges of another user, by default the superuser. It originally stood for "superuser do" as the older versions of sudo were designed to run commands only as the superuser. However, the later versions added support for running commands not only as the superuser but also as other (restricted) users, and thus it is also commonly expanded as "substitute user do". Although the latter case reflects its current functionality more accurately, sudo is still often called "superuser do" since it is so often used for administrative tasks.
___

## Creating an user :

Add user :
```
$ sudo adduser [username]
```

Give sudo rights :
```
$ usermod -aG sudo [username]
```

Remove user :
```
$ sudo deluser [username]
```
___

## How to connect to a distant Linux server

### To do on distant server :

Install SSH server :
```
$ sudo apt install openssh-server
```

Open port 22 :
```
$ sudo ufw allow 22/tcp
```

### To do on client with ssh service installed :

Connect :
```
$ ssh [username]@[server-ip]
```

Example :
```
$ ssh theo@192.168.42.129
```

Connect as root user :
```
$ ssh root@[server-ip]
```
___