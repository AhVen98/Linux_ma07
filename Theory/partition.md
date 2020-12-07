# Partition
## Tables de partition
Même si un disque brut sans partitions est théoriquement utilisable, il est vivement recommandé de le partitionner afin d'héberger un système Linux.  
Partitionner un disque consiste à **diviser l'espace disponible** en plus petites parties. Dans le cadre d'une architecture amd64, chacune de ces parties s'appelle une partition. Celles-ci sont plus faciles à gérer.   
Il existe actuellement 2 technologies standards en terme de partitionnement : **MBR** et **GPT**

### MBR
MBR est un acronyme signifiant **Master Boot Record**. Il utilise du 32 bits pour tout ce qui concerne le secteur de démarrage et la longueur d'addressage des partitions. Ainsi, il existe un nombre très limité de partitions du disque. 
De plus, il supporte 3 types de partitions différentes : 
- **Primaire**  
Les informations présentes sur ces partitions sont stockées directement dans le MBR. Il s'agit d'un espace restreint, qui limite de ce fait le nombre maximum de partitions primaires à 4.
- **Étendue**  
Une partition étendue est une partition qui peut contenir d'autres partitions.  
Si des partitions supplémentaires doivent pouvoir être créées, il faut indiquer une des partitions primaires en tant que partition étendue. La limite de 4 partitions primaires ne sera ainsi pas dépassée, mais il y a la possibilité d'ajouter des partitions logiques.

- **Logique**  
On parle de partition logique quand on crée une partition **dans** une partition. 



### GPT
Il s'agit d'un acronyme signifiant **GUID Partition Table**. Les identifiants pour les partitions sont créées sur un adressage de 64 bits. Cette différence au niveau du nombre de bits permet au GPT d'avoir un nombre de partitions énorme en comparaison au MBR. La taille de ces partitions peut également être beaucoup plus grande (8 zettaoctets [Zo]).
Par souci de compatibilité, lorsque l'interface utilisée est l'UEFI (et non pas le BIOS), la présence de problèmes de compatibilité avec MBR rend l'utilisation du GPT logique et conseillé.  
GPT offre de plus une résistance aux dégâts subis par le GPT principal, grâce à une sauvegarde présente en fin de disque.

## Quand utiliser MBR ?
Bien que GPT semble être mieux sur tous les points, il reste des situations dans lesquelles il est conseillé d'utiliser MBR.   
1. Si on démarre une machine basée sur le BIOS : Windows démarrera automatiquement en mode UEFI s'il détecte des partitions GPT. Pour palier à ça, il faut réaliser un double démarrage.
3. Microprogrammes boggés : Certains programmes, configurés pour démarrer en BIOS/Legacy/... peuvent avoir des soucis s'ils sont démarrés à partir de disques partitionnées en GPT.   
Le problème reste contournable, en ajoutant un indicateur (boot/active) sur une partition de protection MBR [effectué via fdisk grâce à l'option -t dos]. Cela force le microprogramme à lire ce qui se trouve sur la partition à l'aide du format MBR.


## Sources
https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Disks/fr