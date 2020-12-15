# Utilisateurs Debian et Connexion SSH

sudo est une commande utilisée principalement dans les systèmes Unix qui permet aux utilisateurs de lancer des programmes avec les privilèges d'un autre utilisateur, par défault le superuser. À l'origine, sudo vient de "superuser do" car les anciennes versions de sudo permettaient seulement d'exécuter des commandes en tant que superuser. Cependant, dans les dernières versions de sudo, il est maintenant possible d'exécuter des commandes en tant que n'importe quel utilisateur.

[sudo Wikipédia](https://fr.wikipedia.org/wiki/Sudo)
___

## Création d'un utilisateur :

Ajouter un utilisateur :
```
$ sudo adduser [username]
```

Ajouter un utilisateur à un groupe :
```
$ usermod -aG [groupname] [username]
```

Donner les droits sudo :
```
$ usermod -aG sudo [username]
```

Retirer un utilisateur d'un groupe :
```
$ gpasswd -d [group] [username]
```

Retirer les droits sudo :
```
$ gpasswd -d sudo [username]
```

Effacer utilisateur :
```
$ sudo deluser [username]
```

Afficher la liste des utilisateurs :
```
$ cat /etc/passwd
```

[Debian User Management Documentation](https://wiki.debian.org/UserAccounts)
[Linux List All Users In The System](https://www.cyberciti.biz/faq/linux-list-users-command/)
___

## Comment se connecter à un hôte distant

### Sur la machine hôte :

Installer le serveur SSH :
```
$ sudo apt install openssh-server
```

Ouvrir le port 22 :
```
$ sudo ufw allow 22/tcp
```

### Sur le client :

Se connecter en SSH :
```
$ ssh [username]@[server-ip]
```

Exemple :
```
$ ssh theo@192.168.42.129
```

Se connecter avec l'utilisateur root :
```
$ ssh root@[server-ip]
```
[SSH to connect to remote Linux or Windows](https://phoenixnap.com/kb/ssh-to-connect-to-remote-server-linux-or-windows)
___
