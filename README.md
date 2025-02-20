**NOTE: ce TP est une copie du TP ["graphes" du groupe 4](https://github.com/boisgera/python-advanced-companion/tree/master/tps/graphs), placé dans un dépôt dédié afin de vous faire utiliser git.**

Labyrinthes
================================================================================

Un labyrinthe de taille 50 x 25 :

![Un labyrinthe dense de taille 50 x 25](images/dense_random_maze.png)

Ce labyrinthe est généré aléatoirement en respectant deux propriétés :

  - on peut explorer tout le labyrinthe quel que soit son point de départ,

  - si l'on rajoute un mur où que ce soit cette propriété disparaît.

Graphes
--------------------------------------------------------------------------------

Un graphe orienté et pondéré est représenté par le triplet composé 

  - d'un ensemble `vertices` de sommets,

  - d'un ensemble `edges` d'arêtes, représentées comme des paires de sommets,

  - d'un dictionnaire `weights` associant à chaque arête une valeur numérique.

Labyrinthes
--------------------------------------------------------------------------------

Un labyrinthe est une collection de cellules caractérisées par leurs coordonnées
(deux entiers positifs ou nuls) ainsi qu'une collection de murs entre
les cellules adjacentes (au nord, à l'est, au sud ou à l'ouest d'une cellule 
donnée) qui s'ajoutent aux murs entre l'intérieur et l'extérieur du labyrinthe.

Le graphe associé :

  - représente les cellules par la paire de leur coordonnées,

  - considère qu'une arête est présente dans le graphe si les deux cellules
    sont adjacentes, dans le labyrinthe et qu'aucun mur ne les sépare.

  - associe à chaque arête le poids 1 : le coût de l'action qui consiste à
    se déplacer d'une cellule à une cellule adjacente.

### Labyrinthes élémentaires

Développez une fonction `full_maze(width, height)` qui produit le graphe
d'un labyrinthe rectangulaire contenant la cellule `(0, 0)`, large de 
`width` cellules, haut de `height` cellules et contenant un mur entre
chaque paire de cellules adjacentes.

Puis, développez une fonction `empty_maze(width, height)` qui produit le
graphe rectangulaire similaire mais sans aucun mur interne.

### Visualisation

Utilisez la fonction `display_maze` dont le code est fourni en annexe de
ce document pour visualisez vos labyrinthes, par exemple :

``` pycon
>>> maze = full_maze(50, 25)
>>> display_maze(maze)
```

### Autres labyrinthes

Chargez et visualisez les labyrinthes disponibles dans le dossier :

  - 📁 <https://github.com/boisgera/python-advanced-companion/tree/master/tps/graphs/mazes>

Le format de ces fichier est simplement la représentation `repr` du graphe
associé à un labyrinthe.

Essayez de créer vos propres labyrinthes rectangulaires (en forme de
serpent, de spirale, etc.) ; éventuellement, essayez de déterminer une
méthode pour créer un labyrinthe "dense" similaire à celui représenté 
[ici](images/dense_random_maze.png).


Chemins
--------------------------------------------------------------------------------

### Ensemble atteignable

Implémentez une fonction `reachable_set(maze, origin)` qui renvoie l'ensemble
des cellules d'un labyrinthes accessible depuis la cellule `origin`.

Vous pourrez tester votre résultat graphiquement de la façon suivante :

``` pycon
>>> cells = reachable_set(maze, origin)
>>> display_maze(maze, map=cells)
```

Par exemple dans le labyrinthe ci-dessous, les cellules accessibles
depuis le coin en bas à gauche, représentées en jaune, sont toutes les
cellules du labyrinthe :

![Un labyrinthe dense de taille 50 x 25](images/dense_random_maze-reachable.png)


### Chemins associés

Implémentez une fonction `reachable_paths(maze, origin)` qui renvoie un 
dictionnaire dont les clés sont les cellules atteignables depuis l'origine
et les valeurs des chemins associés qui joignent l'origine et la destination.
Un chemin `path` sera représentés par une liste de cellules telles que
`path[0]` est l'origine, `path[i]` et `path[i+1]` sont adjacentes pour tout
`i` et `path[-1]` est la destination.

Vous pourrez tester votre résultat graphiquement de la façon suivante :

``` pycon
>>> paths = reachable_paths(maze, origin)
>>> destination = (???, ???) # a cell reachable from origin
>>> display_maze(maze, path=paths[destination])
```

Par exemple dans le labyrinthe ci-dessous, le chemin représenté en rouge joint
la cellule en bas à gauche et la cellule en haut à droite du labyrinthe :

![Un labyrinthe dense de taille 50 x 25](images/dense_random_maze-path.png) 

### Chemin optimal associé

Implémentez une fonction `shortest_paths(maze, origin)` qui renvoie un 
dictionnaire dont les clés sont les cellules atteignables depuis l'origine
et les valeurs un des chemins associés le plus courts (nécessitant le moins
de déplacements) qui joignent l'origine et la destination.

Vous pourrez tester votre résultat graphiquement en invoquant `display_maze`
comme à la question précédente.

Performance
--------------------------------------------------------------------------------

Plusieurs stratégies permettent d'améliorer les performances de la recherche
des plus courts chemins, un point qui devient critique quand la taille des
labirynthes augmente ; notamment le choix de structures de données plus 
efficaces, et choix d'algorithmes plus efficaces.

### Mesure de la performance

Dans tous les cas, pour mesurer les (éventuels) progrès réalisés,
nous pourrons afficher le temps passé à déterminer les chemins optimaux ;
par exemple :

``` python
start = time.time()
paths = shortest_paths(maze, origin)
stop = time.time()
print(f"elapsed time (secs): {stop - start}")
```

Pour obtenir une image plus précise de ce qui se passe, et savoir dans quelle 
partie du code le temps est passé, on pourra utiliser le projet

  - 🐍 <https://github.com/pyutils/line_profiler>

### Structure de données

La structure de données choisie initialement pour représenter les graphes
n'est pas nécessairement la mieux choisie. Déterminez dans votre algorithme
quelles sont les opérations les plus fréquemment utilisées ; adaptez 
votre représentation des graphes en conséquence et mesure le résultat.

### Algorithmes

Améliorez ensuite l'algorithme lui-même. On pourra notamment étudier le
classique : 

  - 🎓 <https://fr.wikipedia.org/wiki/Algorithme_de_Dijkstra>
