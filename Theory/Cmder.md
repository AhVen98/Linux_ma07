# Cmder

![Logo Cmder](https://avatars1.githubusercontent.com/u/11646750?s=400&v=4)  

Cmder, prononcé "commander", est un émulateur de console.
> Cmder is a software package created out of pure frustration over the absence of nice console emulators on Windows. It is based on amazing software, and spiced up with the Monokai color scheme and a custom prompt layout, looking sexy from the start.

La console Cmder est téléchargeable sur le [Site officiel](https://cmder.net/).
Bien que quelques informations y soient présentes, il est conseillé d'aller directement sur le GitHub officiel pour toute documentation.
Le [README](https://github.com/cmderdev/cmder/blob/master/README.md) qui s'y trouve, documente la plupart des fonctionnalités de Cmder.
De l'aide est également disponible sur GitHub via le [Wiki Cmder](https://github.com/cmderdev/cmder/wiki).

### Quelques raccourcis clavier

Gestion des fenêtres :

- `Ctrl` + `T` : Nouvel onglet (configurable)
- `Ctrl` + `W` : Fermer l'onglet
- `Shift` + `Alt` + `1` : Nouvel onglet CMD (ouverture rapide)
- `Shift` + `Alt` + `2` : Nouvel onglet PowerShell (ouverture rapide)
- `Ctrl` + `Tab` : Passer à l'onglet suivant
- `Ctrl` + `Shift` + `Tab` : Passer à l'onglet précédent
- `Ctrl` + `#Numero` : Passer à l'onglet #Numero
- `Alt` + `Enter`: Plein écran

Shell :

- `Ctrl` + `Alt` + `U` : Remonte d'un cran dans la structure de répertoire active
- `Ctrl` + `R` : Recherche dans l'historique des commandes effectuées (reverse-i-search)
- `Shift` + Souris : Sélectionne du texte et le copie. (Possible également uniquement avec la souris)

### Alias

Des alias peuvent être créés avec la commande `alias`. Il est également possible de les ajouter directement dans le fichier correspondant (/config/aliases), un alias par ligne.   
Exemple :
``
alias ls=ls --color $_
``

### Sources
+ Site officiel Cmder (https://cmder.net/)
+ Le README du GitHub Cmder (https://github.com/cmderdev/cmder/blob/master/README.md)