# Modifier le fichier sshd_config

Ouvrir dans un éditeur de texte le fichier sshd_config

```bash
$ sudo nano /etc/ssh/sshd_config
```

Pour donner l'accès SSH à utilisateur, il suffit de modifier/ajouter cette ligne:

```bash
AllowUsers [username]
```

Pour donner l'accès SSH à un groupe, il suffit de modifier/ajouter cette ligne:

```bash
AllowGroups [groupname]
```

Pour retirer l'accès SSH à utilisateur, il suffit de modifier/ajouter cette ligne:

```bash
DenyUsers [username]
```

Pour retirer l'accès SSH à un groupe, il suffit de modifier/ajouter cette ligne:

```bash
DenyGroups [groupname]
```

Ensuite il faut redémarer le service SSH

```bash
$ sudo systemctl restart sshd
```

Un utilisateur qui n'a plus accès SSH devrait voir ce message quand il essaie de se connecter:

```bash
Permission denied, please try again.
```
