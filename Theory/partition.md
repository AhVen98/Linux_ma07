# Partition
## Tables de partition
Même si un disque brut sans partitions est théoriquement utilisable, il est vivement recommandé de le partitionner afin d'héberger un système Linux.  
Partitionner un disque consiste à **diviser l'espace disponible** en plus petites parties. Dans le cadre d'une architecture amd64, chacune de ces parties s'appelle une partition. Celles-ci sont plus faciles à gérer.   
Il existe actuellement 2 technologies standards en terme de partitionnement : **MBR** et **GPT**

### MBR
MBR est un acronyme signifiant **Master Boot Record**. Il utilise du 32 bits pour tout ce qui concerne le secteur de démarrage et la longueur des partitions. De plus, il supporte 3 types de partitions différentes : 
- **Primaire**  
Les informations présentes sur ces partitions sont stockées directement dans le MBR. Il s'agit d'un espace restreint, qui limite de ce fait le nombre maximum de partitions primaires à 4.
- **Étendue**  
Une partition étendue est une partition qui peut contenir d'autres partitions.  
Si des partitions supplémentaires doivent pouvoir être créées, il faut indiquer une des partitions primaires en tant que partition étendue. La limite de 4 partitions primaires ne sera ainsi pas dépassée, mais il y a la possibilité d'ajouter des partitions logiques.

- **Logique**  
On parle de partition logique quand on crée une partition **dans** une partition. 



### GPT

## Sources
https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Disks/fr