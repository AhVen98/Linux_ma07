# nftables

> Le moteur de noyau nftables ajoute une simple machine virtuelle au noyau Linux, capable d'exécuter du bytecode pour inspecter un paquet réseau et de décider de la manière dont ce paquet doit être traité. Les opérations implémentées par cette machine virtuelle sont intentionnellement rendues basiques. Il peut extraire des données du paquet lui-même, consulter les métadonnées associées (interface entrante, par exemple) et gérer les données de suivi de connexion. Des opérateurs arithmétiques, binaires et de comparaison peuvent être utilisés pour prendre des décisions en fonction de ces données. La machine virtuelle est également capable de manipuler des ensembles de données (généralement des adresses IP), ce qui permet de remplacer plusieurs opérations de comparaison par une recherche d'ensemble unique.
> -- [Wikipedia](https://fr.wikipedia.org/wiki/Nftables: 'Article "nftables"')

nftables est donc un sous-système du noyau Linux permettant le filtrage des paquets, le NAT et autres traitements de paquets. Il est le successeur du framework iptables  
La dernière release de nftables (au jour du 07.12.2020) est la 0.9.7, datant du 27.10.2020. Elle est disponible sur le site [netfilter.org](https://netfilter.org/projects/nftables/downloads.html#nftables-0.9.7).

_ATTENTION_ : nftables ne fonctionne qu’avec les versions Linux 3.13 ou supérieures.

## Firewalld

Rapide présentation à faire !
https://firewalld.org/  
https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/securing_networks/using-and-configuring-firewalld_securing-networks#controlling-network-traffic-using-firewalld_using-and-configuring-firewalld

## nftables et Debian

A parti de Debian Buster, nftables est le framework de pare-feu utilisé par défaut (et celui recommandé).  
Afin d’activer nftables et que celui-ci soit actif dès le démarrage, il faut procéder comme suit :

```
apt install nftables
systemctl enable nftables.service
```

Les règles seront alors disponibles, par défaut, dans `/etc/nftables.conf`.  
Pour arrêter nftables, il suffit de supprimer toutes les règles avec un `nft flush ruleset`.  
Si nftables ne doit pas se lancer au démarrage : `systemctl mask nftables.service`.  
Pour désinstaller nftables et en supprimer toutes traces : `aptitude purge nftables`.

## Prise en main de nftable

Avant de se lancer dans l’utilisation de nftables, il est important de comprendre certains concepts du framework. Un article anglais a été conçu dans ce but sur le wiki officiel, intitulé "[Quick reference-nftables in 10 minutes](https://wiki.nftables.org/wiki-nftables/index.php/Quick_reference-nftables_in_10_minutes)".  
Pour toute aide concernant nftables, le [wiki officiel](https://wiki.nftables.org/wiki-nftables/index.php/Main_Page) est disponible.

## Création de notre pare-feu avec nftables

Notre pare-feu ne doit autoriser que le port 22 (SSH) pour l’entrée et n’a besoin d’aucune restriction pour la sortie. Pour une question de bonne pratique (sur les conseils de Alexandre Dubrulle), nous avons également ajouté des chaines pour la sortie et le bouclage.  
Il faut commencer par créer une table (appelée ici firewall) avec `nft add table inet firewall `.
Il faut ensuite y ajouter des chaines avec leur règle principale (une ligne à la fois) :

```
nft 'add chain inet firewall input { type filter hook input priority 0; policy drop;}'
nft 'add chain inet firewall output { type filter hook output priority 0; policy accept; }'
nft 'add chain inet firewall forward { type filter hook forward priority 0; policy accept; }'
```

Après la création des chaines, il faut y ajouter la règle qui nous interesse, comme suit :

```
nft add rule inet firewall input tcp dport ssh accept comment "Accept SSH connection (port22)"
nft add rule inet firewall input iif lo accept comment "Accept any localhost traffic"
nft add rule inet firewall input ct state invalid drop comment "Drop invalid connections"
nft add rule inet firewall input ct state established,related accept comment "Accept traffic originated from us"

```

Le mot clé `comment` permet d'ajouter un commentaire, renseigné avec des `" "`.
Le résultat, lors de l’affichage de nos tables via la commande `nft list table inet firewall`, correspondra alors à ceci :

```bash
table inet firewall {

    chain input {
        type filter hook input priority 0; policy drop;
        tcp dport ssh accept #Accept SSH connection (port 22)
        iif lo accept #Accept any localhost traffic
        ct state invalid drop #Drop invalid connections
        ct state established,related accept #Accept traffic originated from us
    }

    chain output {
        type filter hook ouput priority 0; policy accept;
    }

    chain forward {
        type filter hook forward priority 0; policy accept;
    }

}
```

**Attention** : La configuration ne sera pas concervée, si celle-ci n'est pas enregistrée dans le fichier de configuration. `nft list table <nomTable> > /etc/nftables.conf`

_NOTE_ : La famille `inet` indique que la table sera traversée par les pacquets IPv4 et IPv6. Les règles propre à un type d’adresse n’impacteront pas l’autre type et vice-versa.

Ajouter une règle avec `add` l'ajoutera à la suite des autres. Si on veut en ajouter une au début des règles d'une chaine, il faut utiliser `nft inserft rule <nomTable> <nomChaine> <regle>`. Il est également possible de spécifier une position précise pour une nouvelle règle, avec `position <numero>` placé entre le nom de la chaine et la règle. Pour cela, il faut vérifier le numéro des règles présentes avec `nft list table <nomTable> -a`. Dans ce cas, utiliser `add` ajoutera la règle après la position choisie et `insert`l'ajoutera avant.

### Suppression

Une table est supprimable (Depuis Linux 3.18, le contenu est supprimé en même temps) avec la commande `nft delete table <famille> <nomTable>`. Une table peut également être vidée de ses chaines et de ses règles avec `nft flush table <famille> <nomTable>`.

Une chaine est supprimable avec `ntf delete chain <famille> <nomTable> <nomChaine>`. Il faudra cependant la vider de son contenu au préalable avec `ntf flush chain <nomTable> <nomChaine>`.

Il est également possible de supprimer l'intégralité des règles, chaines et tables avec `nft flush ruleset`. Si on y ajoute à la suite une famille (ex: `inet`), seules les règles de cette famille seront effacées.

Une règle seule est supprimable, mais pour cela, il faut récupérer le numéro (handle) de la règle. Pour cela, il faut lister les règles avec le paramètres `-a` : `nft list table <nomTable> -a`. Il sera alors possible de supprimer la règle choisie grâce à `nft delete rule <nomTable> <nomChaine> handle <numeroRegle>`.

### Modification

Une règle peut être modifiée grâce à la commande `replace`. Il faut par contre indiquer le "handle" de cette règle.
La commande complète se constitue comme suit : `nft replace rule <nomTable> <nomChaine> handle <numeroRegle> <nouvelleRegle>`

### Autres informations utiles
Pour configurer nftables, il y a trois manières possibles :
- Avec la commande `nft` (comme montré dans ce document)
- Avec le shell interactif, lancé avec `nft -i` (`nft` ne sera alors plus nécessaire)
- Avec le fichier de configuration `/etc/nftables.conf` (ATTENTION : cette méthode est déconseillée. Les erreurs y sont très faciles.)

### Scripting

Il est également possible de créer des scripts pour diverses configurations. Le point 6.3 de la documentation trouvable sur RedHat ([Getting started with nftables securing networks](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/securing_networks/getting-started-with-nftables_securing-networks)) propose une bonne base sur cette partie là.

# Sources

* https://fr.wikipedia.org/wiki/Nftables
* https://wiki.nftables.org/wiki-nftables/index.php/Main_Page
* https://netfilter.org/projects/nftable
