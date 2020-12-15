# Commandes de vérification

## Configuration système

- Afficher les informations système : `hostnamectl`
    - Permet de voir (entre autre) le nom d'hôte ainsi que la version Linux utilisée
```bash
   Static hostname: debian-paola
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 3628fab9b4034be48d9c977cd21e9668
           Boot ID: 1103bad81b0b4758ac2245b8f562300c
    Virtualization: vmware
  Operating System: Debian GNU/Linux 10 (buster)
            Kernel: Linux 4.19.0-13-amd64
      Architecture: x86-64
```
- Affichage de la release : `lsb_release -a`
- Affichage de la version de Linux : `cat /etc/debian_version`
- Affichage du nom de domaine : `hostname -d`
```bash
CPNV-MA-08
```
- Affichage de la configuration : `lscpu`
```bash
Architecture:        x86_64
CPU op-mode(s):      32-bit, 64-bit
Byte Order:          Little Endian
Address sizes:       45 bits physical, 48 bits virtual
CPU(s):              1
On-line CPU(s) list: 0
Thread(s) per core:  1
Core(s) per socket:  1
Socket(s):           1
NUMA node(s):        1
Vendor ID:           GenuineIntel
CPU family:          6
Model:               158
Model name:          Intel(R) Core(TM) i5-9500 CPU @ 3.00GHz
Stepping:            10
CPU MHz:             2999.996
BogoMIPS:            5999.99
Hypervisor vendor:   VMware
Virtualization type: full
L1d cache:           32K
L1i cache:           32K
L2 cache:            256K
L3 cache:            9216K
NUMA node0 CPU(s):   0
Flags:               fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon nopl xtopology tsc_reliable nonstop_tsc cpuid pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch cpuid_fault invpcid_single pti ssbd ibrs ibpb stibp fsgsbase tsc_adjust bmi1 avx2 smep bmi2 invpcid rdseed adx smap clflushopt xsaveopt xsavec xgetbv1 xsaves arat md_clear flush_l1d arch_capabilities
```
- Affichage des partitions : `lsblk` ou `fdisk -l`
```bash
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0    8G  0 disk
|-sda1   8:1    0  476M  0 part /boot
|-sda2   8:2    0  1.9G  0 part [SWAP]
L-sda3   8:3    0  5.7G  0 part /
sr0     11:0    1 1024M  0 rom
```
OU
```bash
Disk /dev/sda: 8 GiB, 8589934592 bytes, 16777216 sectors
Disk model: VMware Virtual S
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xf539ec0d

Device     Boot   Start      End  Sectors  Size Id Type
/dev/sda1  *       2048   976895   974848  476M 83 Linux
/dev/sda2        976896  4882431  3905536  1.9G 82 Linux swap / Solaris
/dev/sda3       4882432 16775167 11892736  5.7G 83 Linux
```

- Verification du fonctionnement du swap
    - Note : il est nécessaire d'avoir `stress-ng` d'installé pour cetta manipulation
    - Afficher l'utilisation de la memoire :`free -m`
    - Stresser la mémoire pour utiliser le swap : `stress-ng --vm 2 --vm-bytes 1G --timeout 60s &`
    - Afficher l'utilisation de la memoire :`free -m`
```bash
debianadmin@debian-paola:~$ free -m
              total        used        free      shared  buff/cache   available
Mem:            962          94         828           0          40         778
Swap:          1906          23        1883
```
- Affichage des information complètes concernant la mémoire : `cat /proc/meminfo`

- Affichage de la langue utilisée par l'OS : `locale -a`
```bash
C
C.UTF-8
en_GB.utf8
POSIX
```
- Affichage de la langue du clavier : `cat /etc/default/keyboard`
```bash
# KEYBOARD CONFIGURATION FILE

# Consult the keyboard(5) manual page.

XKBMODEL="pc105"
XKBLAYOUT="ch"
XKBVARIANT="fr"
XKBOPTIONS=""

BACKSPACE="guess"
```

- Affichage de l'adresse IP : `ip addr`
```bash
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:d4:fc:81 brd ff:ff:ff:ff:ff:ff
    inet 10.229.41.173/20 brd 10.229.47.255 scope global ens33
       valid_lft forever preferred_lft forever
    inet6 fe80::20c:29ff:fed4:fc81/64 scope link
       valid_lft forever preferred_lft forever
```
## Utilisateurs

- Voir l'utilisateur actuellement utilisé : `whoami`
```bash
debianadmin@debian-paola:~$ whoami
debianadmin
```
- Voir les utilisateurs actuellement connectés : `users`
```bash
debianadmin
```
- Voir les groupes utilisateurs: `groups`
```bash
debianadmin sudo
```
- Voir les ID d'un utilisateur : `id [username]'
	- uid = ID de l'utilisateur
	- gid = ID du groupe principal de l'utilisateur
	- groups = ID des groupes secondaires de l'utilisateur
```bash
debianadmin@debian-paola:~$ id debianadmin
uid=1001(debianadmin) gid=1001(debianadmin) groups=1001(debianadmin),27(sudo)
```

- Voir les utilisateurs membres d'un groupe : `getent group [groupname]`
	- Note : Si le nom du groupe n'est pas précisé, cette commande va lister tout les groupes présents, avec leurs utilisateurs respectifs.
```bash
debianadmin@debian-paola:~$ getent group sudo
sudo:x:27:debianadmin
```
- Voir la liste des utilisateurs : `cat /etc/passwd`

## Pare-feu
- Montre le status du service nftables : `systemctl status nftables`
```bash
● nftables.service - nftables
   Loaded: loaded (/lib/systemd/system/nftables.service; enabled; vendor preset: enabled)
   Active: active (exited) since Mon 2020-12-14 16:38:53 CET; 16h ago
     Docs: man:nft(8)
           http://wiki.nftables.org
  Process: 247 ExecStart=/usr/sbin/nft -f /etc/nftables.conf (code=exited, status=0/SUCCESS)
 Main PID: 247 (code=exited, status=0/SUCCESS)
```
- Afficher la liste des tables existantes : `sudo nft list tables`
```bash
table inet firewall
```

- Afficher l'ensemble des règles existantes : `sudo nft list ruleset`
```bash
table inet firewall {                                              
        chain input {                                              
                type filter hook input priority 0; policy drop;    
                tcp dport ssh accept                               
                iif "lo" accept                                    
                ct state invalid drop                              
                ct state established,related accept                
        }                                                          
                                                                   
        chain output {                                             
                type filter hook output priority 0; policy accept; 
        }                                                          
                                                                   
        chain forward {                                            
                type filter hook forward priority 0; policy accept;
        }                                                          
}                                                                  
```

- Afficher la liste des règles pour une table précise : `sudo nft list table [tablefamily] [tablename]`
- Afficher le fichier de configuration nft : `nano /etc/nftables.conf`