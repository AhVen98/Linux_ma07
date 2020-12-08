# Swap
## Swap et pagination
Le swap est une démarche qui agit comme suit : Lorsque la RAM a atteint un certain pourcentage d'utilisation (par défaut, 40%), les processus non utilisés sur la RAM sont déplacés. Ils sont envoyés sur un disque dur, qui sert de "2ème RAM". Le redimensionnement s'effectue manuellement uniquement.   
La pagination est une séparation de la RAM en cadre. Ceux-ci contiennent les adresses mémoires physiques des données.

## Dimension d'un swap
Le dimensionnement dépend de la quantité de RAM de la machine sur laquelle on se trouve. Il est vivement conseillé !   
Il reste cependant au choix de l'utilisateur.

Les dimensions recommandées pour un swap sont les suivantes (elles varient d'un site à l'autre, ce n'est qu'un autre d'idées)
* \<2Go : 2 fois la quantité de RAM pour le swap
* entre 2Go et 8Go : swap égal à la RAM
* \>8Go : au moins 4 Go de swap

## Tests
#### Est-ce que mon swap est activé ?
* Interface graphique (Linux) : 
    - Gnome Partition Editor, « gparted »
* En lignes de commandes : 
    - « free » (Affiche les informations de mémoire vivre, dont le swap)

##### Si ça n'est pas le cas, pourquoi ? 
* Pas nécessaire (assez de RAM)
    - Lancer des programmes gourmands et refaire la commande free
* Pas de partition swap 
    - Voir les partitions avec « sudo parted --list »
* Swap pas activé

### Activer un swap présent
1. Utiliser la commande « **cat /etc/fstab** ». Celle-ci permet de lire le fichier /etc/fstab qui contient l'intégralité des disques, partitions et options.
2. S'assurer qu'il y ait une ligne semblable à celle qui suit. Cela assure la présence du swap au démarrage.
> /dev/sdb5 none swap sw 0 0
3. Désactiver l'intégralité des swaps, les recréer et les réactiver avec les commandes qui suivent.
> désactiver les swaps : sudo swapoff -a  
> recréer les swaps : sudo /sbin/mkswap /dev/sdb5  
> activer les swaps : sudo swapon -a



## Sources
### pour le swap et la pagination
[Wikipédia sur la mémoire virtuelle](https://fr.wikipedia.org/wiki/M%C3%A9moire_virtuelle)  
[Définition de la pagination](https://www.techno-science.net/definition/11540.html)  
[Définition du swap](https://www.techopedia.com/definition/30467/memory-swapping)  
[Explication globale sur le swap et la pagination](http://www.learnlinux.org.za/courses/build/internals/ch05s03.html)  


### pour le dimensionnement
[dimensionnement du swap](https://access.redhat.com/documentation/fr-fr/red_hat_enterprise_linux/7/html/storage_administration_guide/ch-swapspace)

### pour les tests
[FAQ sur le swap d'Ubuntu](https://help.ubuntu.com/community/SwapFaq)  
[commande cat](https://www.linuxpedia.fr/doku.php/commande/cat)  
[etc\fstab](https://geek-university.com/linux/etc-fstab-file/)
[forum ubuntu : vérifier le fonctionnement de la partition swap](https://forum.ubuntu-fr.org/viewtopic.php?id=109434)  
[forum ubuntu : vérifier l'utilisation de la partition swap (actif ou non)](https://forum.ubuntu-fr.org/viewtopic.php?id=105179)  
