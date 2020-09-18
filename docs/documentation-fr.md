# Documentation

Orchestra est un langage textuel de notation musicale construit autour du principe que la créativité est plus importante que la rigueur.

Contrairement aux autres notations, Orchestra n'est pas fait pour générer des partitions de musique mais la musique elle-même. Se libérant ainsi des limitations imposés par les partitions.

> Une fois que l'esprit est libre du concept  
> D'exactitude de l'harmonie et de la musique  
> Tu peux faire tout ce que tu veux.  
> -- Giorgio Moroder


## Exemple

    "Bella Ciao"
    $ E_ #F_ G_
    $ 120bpm
    || {- B_ E F A E}/2 - :|
    {- B_ E F}/2 A {G F}/2 A {G F}/2 B B B/2
    {B A B}/2 C C {- C B A}/2 C B -
    {A G}/2 F B G F 2E


## Notes

Les notes sont écrites avec les lettres de `A` à `G`. `A` correspond à un `la` et donc `G` au `sol`.
Il n'y a pas de sensibilité à la casse, donc `a` est la même chose que `A`.

    A B C D

Les lettres sont uniquement utilisées pour les notes. Les espaces sont optionnels et seulement utilisés pour la lisibilité. `AAA` est équivalent à `A A A`.

Plus généralement, il est possible d'utiliser n'importe quelle lettre entre `A` et `Z`. Cela veut juste dire que `H` est un `A`, un octave au-dessus.


## Silence

Les silences sont écrits utilisant le symbole `-`.

    - B - D

## Rythme

Une simple note `A` dure 1 temps (équivalent à une noire).
Les notes peuvent être multipliées ou divisées par des nombres entiers.

Tout entiers directement devant une note allonge (multiplication) sa durée.

    2A 2B
> Ce qui veut dire que `1A` est équivalent à `A`.

Ajouter un `/` puis un entier directement après une note va réduire (division) sa durée.

    A/2 B/2 C D -/2 F/2
> Ce qui veut dire que `A/1` est équivalent à `A`.

Il est possible d'utiliser les deux pour des rythmes plus complexe.

    3A/2 B/2 C 9-/16 7E/16
> Ce qui veut dire que `A` est équivalent à `2A/2`, `3A/3` ...

Utiliser `0` n'est **pas valide**, que ce soit en multiplication ou division, `0A` et `A/0` vont produire une erreur.


## Altérations

Pour plus légèrement modifier la hauteur d'une note, il est possible d'utiliser les altérations.

 - `#` - Dièse, monte d'un demi-ton
 - `&` - Bémol, descend d'un demi-ton
 - `@` - Bécarre, annule toutes précédentes altérations de cette note

Les altérations sont placées devant une note autant de fois que nécessaire.

    #A B &&C @D

Deux demi-tons sont équivalent à un ton, cela veut dire que `##A` revient au même que `B`.
En raison du fait que `#` et `&` s'annule, `#&&#A` est égale à `A`.
Les altérations n'ont pas d'ordre défini.
Par contre, le bécarre à une priorité supérieure, annulant les autres altérations. Cela veut dire que `##@###A` est équivalent à `A`.

> Il faut noter que, à cause des limitations de la [gamme tempérée](https://fr.wikipedia.org/wiki/Gamme_temp%C3%A9r%C3%A9e), il n'y qu'un demi-ton entre `B`-`C` et `E`-`F`.
> De ce fait, `#B` est la même note que `C`, et `&C` est la même que `B` (même chose pour `E` et `F`).


## Octave

Par défaut, toutes les notes sont dans l'octave de la clé de sol. Bien sur, cela peut être modifié.

 - `^` - Monte d'une octave
 - `_` - Descend d'une octave
 - `~` - Retourne à l'octave par défaut (C4)

Les octaves sont définies après la note.

    A_ B C^^ D~
    
Pour en revenir au `H` vu plus haut, et bien il est équivalent à `A^`, `I` est équivalent à `B^` ...

Tout comme les altérations, les octaves peuvent être changées dans n'importe ordre et autant de fois que nécessaire.
De la même façon, `_` et `^` s'annulent, `A_^^_` est donc égale à `A`.
Par contre, le retour à une plus haute priorité, donc `A__~___` sera égale à `A`.


## Récapitulation

Toute note peut avoir des altérations, un rythme, une hauteur et un changement d'octave.

![exemple de #2A/3___](./example.png)

 - `#` une liste d'altérations (ici 1 demi-ton plus haut)
 - `2`, `/3` le rythme de la note (ici 2/3 d'un temps)
 - `A` la hauteur d'une note (ici 440Hz)
 - `___` une liste de changement d'octave (ici 3 octaves plus bas)


## Groupement

Il peut être utile d'appliquer des modifications à plusieurs notes en même temps pour réduire les répétitions.
Le rythme, les altérations et les octaves peuvent être appliqué à tout un groupe, ce qui va modifier toutes les notes dans ce groupe.
Il existe plusieurs types de groupe.

### Neutre

Utiliser des accolades `{}` sert uniquement à grouper des notes sans changer la façon dont elles sont jouées.

    A_ B_ C_ D_

Peut être simplifié par :

    {A B C D}_

Les accolades peuvent être imbriquées autant de fois que nécessaire.

    A_ B_ #C_ #D_

Peut être simplifié par :

    {A B #{C D}}_

### Accord

Pour jouer plusieurs notes en même temps, elles peuvent être groupé dans des chevrons `<>`.

    <A B 3C> D
> La duré d'un accord sera défini par la longueur de la note la plus longue. Ici l'accord dure 3 temps.

Les chevrons **ne peuvent pas** être imbriqués et produiront une erreur, `<A <B C>>` est invalide.


### Liaison ou legato

Parfois, les notes doivent être jouées de l'une à l'autre sans interruption. Ceci peut être dénoté par un groupement dans des parenthèses `()`.

    (A B C) D

Les parenthèses **ne peuvent pas** être imbriqués et produiront une erreur, `(A (B C))` est invalide.


## Barres

La notation musical classique délimite chaque mesure par une barre.
Orchestra autorise l'utilisation de la barre vertical `|` dans les compositions, mais cela est purement décoratif et ne sert qu'à la lisibilité.

    || A B C D | E F G H ||


## Répétition

Pour répéter un passage et réduire les duplications, il est possible d'utiliser les deux-points `:`.
Rencontrer ce symbole retourne en arrière jusque la dernière double-barre `||` ou tout au début si aucune n'est présente.
Il est possible d'utiliser autant de fois que nécessaire pour augmenter le nombre de répétitions du passage.

    A B C D | E F G H | E F G H | E F G H

Peut être simplifié par :

    A B C D || E F G H ::

Les répétitions n'ont pas à être suivi d'une barre, mais cela peut être une bonne idée pour améliorer la clarté.
Les boucles plus complexes peuvent être écrites en imbriquant les répétitions.

    A B | C D | C D | E F | C D | C D | E F |

Peut être simplifié par :

    A B || C D :| E F :|


## Global

Certains modificateurs peuvent être appliqués jusqu'à nouvel ordre. Souvent trouvé au début, ils vont définir le tempo ou la signature de la gamme.
Dans Orchestra, il est possible d'utiliser un dollar `$` au début d'une ligne pour les définir.
Chaque modificateur peut être écrit sur la même ligne ou avoir chacun une ligne.
Each globals can be written in the same line or with one line for each.

    $ 120
    $ #F
    $ #D

Peut être simplifié par :

    $ 120 #F #D


### Tempo

Commencer une ligne par un dollar `$` suivi par un entier va décrire le tempo en [BPM]("battements par minutes"). Il est possible d'ajouter le mot-clé `bpm` pour une meilleur clarté.

    $ 120bpm


### Signature de gamme

Commencer une ligne par un dollar `$` suivi d'une note avec modificateurs va appliquer ces modificateurs à toutes les instances de cette note.
Cela peut être des altérations, un rythme, un changement d'octave ou tous à la fois.

    $ #3F/2_ &D
> Toutes les notes `F` seront plus aigu, dureront 3/2 d'un temps et descendu d'une octave.  
> Toutes les notes `D` seront moins aigu.

Ces modificateurs globaux sont absolus. Ils surchageront tous modificateurs similaires défini auparavant.

    $ #A
    A B C
    $ &A
    A B C
> Le premier global rends tout les `A` plus aigu. Le second ne va pas l'annuler, mais le remplacer et rendre tous les `A` moins aigu.

Pour simplement annuler un global, il faut utiliser le symbole d'annulation correspondant (`@`, `~` ou `1`).

    $ @A
> Annule tous les précédentes altérations sur les `A`.

Pour appliquer les modificateurs à toutes les hauteurs de note, il est possible d'utiliser le signe `*`.

    $ #*
> Toutes les notes seront plus aigu.

    $ @1*~
> Retourne toutes les notes à leur valeur par défaut.


## Commentaires

Les commentaires rendent l'expérience plus agréable. Pour insérer des informations contextuelles, il est possible d'entourer du texte par des guillemets `""`.
Orchestra ignorera toutes informations entre deux guillemets, quel que soit le nombre de lignes.

    "
    Title: Ma superbe composition
    Author: Moi
    Date: 08/25/2020
    "
    || A B C D ||
    "La fin"
