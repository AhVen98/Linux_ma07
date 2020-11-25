# MarkDown, résumé

![Markdown Logo](https://grafxflow.co.uk/storage/app/uploads/public/5ad/e5b/d9b/thumb_891_266_0_0_0_auto.png)

> « Markdown est un langage de balisage léger créé en 2004 par John Gruber avec l'aide d'Aaron Swartz. Son but est d'offrir une syntaxe facile à lire et à écrire. » -- [Article Wikipedia](https://fr.wikipedia.org/wiki/Markdown)

Les extensions associées aux documents balisés avec du Markdown sont :

* .md
* .markdown

Seul une partie des balises est présente dans ce document. Pour voir la documentation officielle sur le Markdown, [c'est par ici !](https://daringfireball.net/projects/markdown/)

## Titres

Le niveau du titre correspond au nombre de `#` en début de ligne \(de `<h1>` à `<h6>`\).

```text
# Heading 1
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
###### Heading 6
```

## Emphases

### Italique

Un mot ou un texte peut être mis en italique grâce à un `*` ou `_`. _Ceci est un exemple de texte en italique_

```text
*This text* is italic
_This text_ is italic too
```

### Gras

Un mot ou un texte peut être mis en évidence grâce à `**` ou `__`.  
**Ceci est un exemple de texte en gras**

```text
**This text** is strong
__This text__ is strong too
```

### Italique et gras

Un mot ou un texte peut être mis en évidence grâce à `***` ou `___`. Il est possible de combiner les deux pour le même résultat.  
_**Ceci est un exemple de texte en gras et en italique**_

```text
***This text*** is italic and strong
___This text___ is italic and strong too
*__This text__* is also italic and strong
```

### Barré

Un mot ou un texte peut être tracé grâce à `~~`.  
~~Ceci est un exemple de texte barré~~

```text
~~This text~~ is stricken through
```

## Liens

Les URLs seront automatiquement tranformés et affichés comme des liens. Il est possible de créer des liens hypertextes, avec la syntaxe suivante :

```text
[c'est par ici !](https://daringfireball.net/projects/markdown/)
```

Il est également possible d'y ajouter un message lors du survol du lien :

```text
[Article Wikipedia](https://fr.wikipedia.org/wiki/Markdown "Wikipedia/Markdown")
```

## Images

L'affichage des images est très semblable à celle des liens hypertexte. Il suffit d'avoir un `!` en début de ligne pour signifier que c'est une image et non un lien.

```text
![Markdown Logo](https://markdown-here.com/img/icon256.png)
```

## Citations

Les citations se font grâce à `>` au début de la citation. Si celle-ci s'étale sur plusieurs lignes, il n'est pas nécessaire de répéter la balise.

```text
> This is a quote
```

## Blocs de code

Il est possible de faire des blocs de codes grâce à l'accent grave \` placé autour d'un mot. Si le bloc prend plusieurs lignes, il faudra tripler l'accent \( \`\`\` \). Le langage du code peut également être précisé directement après le triple accent, afin d'avoir une mise en forme liée au langage.  
Les retours à la ligne se font en insérant deux \(ou plus\) espace en fin de ligne.  
Exemple : bout de code en php

```php
  function add($num1, $num2) {
      return $num1 + $num2;
    }
```

## Listes

### Listes à points

Pour les listes à point, les caractères `-`, `*` et `+` permettent de créer un nouveau marqueur. Les sous-points sont également possibles grâce à une tabulation avant le marqueur.  
Exemple :

* Point principal
  * Sous point
    * sous sous point

      \`\`\`
* Red
* Green
  * Light Green
  * Dark Green
* Blue

  \`\`\`

  **Listes ordonées**

  Pour les listes ordonnées, il est nécessaire d'utiliser un chiffre suivit d'un point. Il n'est pas nécessaire que les chiffres se suivent, tant que le premier est un 1 !

  Exemple:

* Premier point
* Second point
* Troisième point

  \`\`\`

* Premier point
* Second point
* Troisième point

  \`\`\`

  Remarque : pour éviter qu'un nombre suivi d'un point soit transformé en liste, il faut précéder le `.` par un backslash `\` \(exemple : `2020.\` \).

  **Listes de tâches**

  Pour créer une liste de tâches avec des caches cochées, il faut précéder la tâche par un `* [ ]` ou `* [x]`.   

  Exemple :

* [x] Tâche faite
* [ ] Tâche non faite

  \`\`\`

* [x] Task 1
* [x] Task 2
* [ ] Task 3

  \`\`\`

