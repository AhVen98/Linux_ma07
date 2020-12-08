# nftables

> Le moteur de noyau nftables ajoute une simple machine virtuelle au noyau Linux, capable d'exécuter du bytecode pour inspecter un paquet réseau et de décider de la manière dont ce paquet doit être traité. Les opérations implémentées par cette machine virtuelle sont intentionnellement rendues basiques. Il peut extraire des données du paquet lui-même, consulter les métadonnées associées (interface entrante, par exemple) et gérer les données de suivi de connexion. Des opérateurs arithmétiques, binaires et de comparaison peuvent être utilisés pour prendre des décisions en fonction de ces données. La machine virtuelle est également capable de manipuler des ensembles de données (généralement des adresses IP), ce qui permet de remplacer plusieurs opérations de comparaison par une recherche d'ensemble unique.
-- [Wikipedia](https://fr.wikipedia.org/wiki/Nftables: 'Article "nftables"')

nftables est donc un sous-système du noyau Linux permettant le filtrage des paquets, le NAT et autres traitements de paquets. Il est le successeur du framework iptables  
La dernière release de nftables (au jour du 07.12.2020) est la 0.9.7, datant du 27.10.2020. Elle est disponible sur le site [netfilter.org](https://netfilter.org/projects/nftables/downloads.html#nftables-0.9.7).

_ATTENTION_ : nftables ne fonctionne qu’avec les versions Linux 3.13 ou supérieures.

## Firewalld
Rapide présentation à faire !
https://firewalld.org/  
https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/securing_networks/using-and-configuring-firewalld_securing-networks#controlling-network-traffic-using-firewalld_using-and-configuring-firewalld

### Firewalld ou nftables ?

Le site [Red Hat](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/securing_networks/using-and-configuring-firewalld_securing-networks) propose une explication simple pour ce choix :

> + firewalld: Use the firewalld utility to configure a firewall on workstations. The utility is easy to use and covers the typical use cases for this scenario.
> + nftables: Use the nftables utility to set up complex firewalls, such as for a whole network.

Dans notre cas, ne sachant pas encore si nous allons devoir toucher notre pare-feu et le configurer de manière plus complexe, nous avons décider de rester sur nftables.

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
nft add rule inet firewall input tcp dport ssh accept comment "Accept SSH"
nft add rule inet firewall input iif lo accept comment "Accept any localhost traffic"
nft add rule inet firewall input ct state invalid drop comment "Drop invalid connections"
nft add rule inet firewall input ct state established,related accept comment "Accept traffic originated from us"

```
Le mot clé `comment` permet d'ajouter un commentaire, renseigné avec des `" "`.
Le résultat, lors de l’affichage de nos tables via l commande `nft list table inet firewall`, correspondra alors à ceci :

```
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

_NOTE_ : La famille `inet` indique que la table sera traversée par les pacquets IPv4 et IPv6. Les règles propre à un type d’adresse n’impacteront pas l’autre type et vice-versa.

### Autre commandes utiles

- Afficher les tables (provoquera une erreur si aucune table n’existe) :`nft list tables`
- Suppression d’une table (et de son contenu depuis la version Linux 3.18) : `nft add table familleDeLaTable nomDeTable`
- Vider une table de ses règles : `nft flush table familleDeLaTable nomDeTable`

### Scripting
A faire !
https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/securing_networks/getting-started-with-nftables_securing-networks

# Sources

https://fr.wikipedia.org/wiki/Nftables
https://wiki.nftables.org/wiki-nftables/index.php/Main_Page
https://netfilter.org/projects/nftable
