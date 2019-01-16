## Semaine 2. Notions de base pour écrire son premier programme en Python

**Ce qu'il faut connaitre:**

- Le processus de Décodage (en quatre étapes)
- ASCII vs Unicode (avec les codages UTF-X)
- Python est un langage ```auto-documenté```
- Est-ce que le type ```str``` est mutable ou immutable ?

- Quels sont les quatre types ```Sequences``` 
- L'opération de Shallow Copy sur une liste - comment la faire et définition 
- Qu'est ce que le Slicing ?
- Comment faire du Slicing dans le sens négatif ?

* Quelle est la différence entre ```Append``` et ```Extend``` ?

* Qu'est ce que la Factorisation de code ?
* Module et Librairie Standard



### 1. Codage, jeux de caractères et Unicode

**<u>Le processus de lecture - Décodage</u>**

Lorsqu'on lit la phrase du haut, le cerveau fait un processus de **Décodage** pour découper chacun des mots.

<img src="media\cours2.1.png" height="100px" />

En informatique, nous manipulons des flux de bits, pour passer à la notion de lettre; nous prendrons des blocs de 7 bits (codage ASCII) et nous leurs donnerons un nombre, il faut ensuite savoir dire à quelle lettre corresponds le code; on prend un **jeux de caractères** qui est **une table** qui va donner pour chaque code un caractère. Il faut ensuite afficher un dessin sur un écran via une police de caractères

<img src="media\cours2.2.png" height="200px" />

La police de caractère va definir une glyphe (un dessin) qui corresponds aux caractères

**<u>Le processus d'écriture - Codage</u>**

Lorsqu'on veut ecrire on fait une opération de codage pour transformer les lettres en flux de bits. 

Avec ASCII on est limité à du $2^7$, $150$ caractères, on va définir des codes dit ```ASCII etendus``` codé sur 8 bits. 

Le projet Unicode a été créée pour éviter d'avoir des problèmes d'encodage due à la diversité des langues, on a plus de 120,000 caractères codes. Unicode utilise différents types de codages:

* UTF-8
* UTF-16
* UTF-32

Ils participent à un compromis entre compacité du codage et vitesse de décodage



### 2. Les chaînes de caractères

L'encodage avec les chaînes de caractères, python est un langage ```auto-documenté``` i.e. qu'on peut directement lire la documentation dans l'interpréteur

```python
In [1]: str?
In [2]: dir(str) # integralite des methodes de str
__reduce__ , # le underscore est un groupe de methode speciale
capitalize 
In [3]: s = 'un mooc est bien'
In [4]: s = s.capitalize()
'Un Mooc Est Bien'
```

**Les chaines de caractères** sont **immuables** en Python, c'est pour ça qu'on doit réaffecter le résultat

En **Python 3.6 pour les prints** on utilise les ```f-strings```

```python
In [5]: n = 'sonia'
In [6]: age = 30
In [7]: f"{n} a {age} ans"
'sonia a 30 ans'
```



### 3. Les séquences

**<u>Les quatre types sequences :</u>**

C'est un ensemble fini et ordonnées d'éléments indicés de $0$ a $n-1$

* list
* tuple
* str
* bytes

```python
>>> s = 'egg, bacon'
>>> s + ', and beans' # va retourner un nouvel objet car une chaine de caractere n'est pas mutable

>>> 'x' * 30
'xxxxxxxxxxxxxxxxxxxxxxxxxxx'
# cette operation va creer une shallow copy qui peut avoir des effets de bord, 
# quand la sequence multiplie n'est pas un immuable mais un objet de type mutable
```

**<u>Operations de slicing dans le sens positif :</u>**

l'operation de slicing retourne un nouvel objet

<img src="media\cours2.3.png" height="50px" />

Pour cette operation si on ne spécifie ni la borne de gauche ni la borne de droite, cette opération ne va pas retourner la chaîne de caractère elle-même mais une copie de cette chaine de caractere; on appelle cette operation une operation de **shallow copy**

```python
>>> s[:] # cette operation retourne toute la chaine, c'est une operation de shallow copy
```

On peut rajouter une notion de pas 

```python
>>> s[0:10:2] # on parcourt les elements de 0 jusqu'a 10 exclus par pas de 2
'eg ao'
>>> s[ : : 2] # on parcourt tous les elements par pas de 2
'eg ao' 
>>> s[100] # va retourner une exception IndexError
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
```

Car le slice est un objet, Python va prendre tous les elements couvert par le slice et il va chercher l'intersection avec les objets disponibles dans notre chaine de caractère

```python
>>> s[5:100] # on remarque qu'on a pas d'erreur
'bacon'
```

Les indices representés par le slice et la chaîne de caractère n'ont pas d'intersection il renvoie un objet chaîne de caractère mais qui est vide

```python
>>> s[100:200] # on remarque qu'on a pas d'erreur
''
```

**<u>Operations de slicing dans le sens négatif :</u>**

On parcourt toujours les indices de gauche a droite, même avec des indices négatifs

<img src="media\cours_2.4.png" height="100px" />

```python
>>> s[:- 3] 
'egg, ba'
>>> s[::- 1]  # le pas est negatif on va donc parcourir dans l'autre sens
'nocab, gge'
>>> s[2:0:- 1]  
'gg'
>>> s[2::-1]  
'gge'
```



### 4. Les Listes

Une liste est une séquence d'objet hétérogène, elle peut donc stocker n'importe quel type d'objet, il faut noter que la liste ne stocke pas les objets mais des références vers les objets. 

La liste est un objet mutable, i.e. on peut le modifier en place, la ou il est stocké, on a donc pas besoin de faire une copie de l'objet pour le modifier.

On peut faire des 

**<u>Opérations d'affectation sur des slices :</u>**

```python
>>> a = [16, 'spam', 3.2, True]
```

L'operation sur un slice va effectuer deux operations indépendantes:

* a[1: 3]  va enlever tous les éléments de 1 inclus et de 3 exclus
* on inserer les elements [1, 2, 3] a la place des elements qui ont ete efface

```python
>>> a[ 1: 3 ] = [1, 2, 3] 
[16, 1, 2, 3, True]
```

**<u>Enlever des éléments :</u>**

```python
>>> a[ 1: 3 ] = [] 
[16, 3, True]
>>> del a[ 1: 2 ] 
[16, True]
```

**<u>Ajouter des éléments unique ou une liste:</u>**

```python
>>> a.append('18')
[16, True, '18']
```

Pour ajouter une liste

```python
>>> a.extend([1, 2, 3])
[16, True, '18', 1, 2, 3]
```

**<u>Opération de Tri:</u>**

```python
>>> a.sort() # sort fonction en place sans faire de copy temporaire, l'objet ne retourne rien
[1, 1, 2, 3, 5, 7, 9]
>>> a = a.sort() # ne va plus retourner l'objet trie mais la valeur de sort() qui est l'objet vide
>>> a 
```



### 5. Introduction aux tests if et à la syntaxe

Python est concu autour de la notion de bloc d'instruction, dans beaucoup de langage ils sont délimités par des accolades.



### 6.Introduction aux boucles for et aux fonctions

Un des points forts de la programmation et ce qui s'appelle la **Factorisation du code**; c'est ce qui permet de ne pas re ecrire plusieurs fois un code qui fait la meme chose.

Ex: fonction et boucle for



### 8. Introduction aux modules

module est un fichier python en .py, lorsqu'on l'importe avec import on va créer un objet module, qui contient plusieurs operations.

Un module est une boîte à outils 

```python
# random est le module
In [1]: import random
# On voit bien le type module
In [2]: print(random)
<module 'random' from 'C:\\Users\\SESA476345\\AppData\\Local\\Continuum\\anaconda3\\lib\\random.py'>
In [3]: dir(random) # avec la fonction built-in dir on accede a tous les attributs du modules
In [4]: help(random.randint) # pour avoir de la documentation sur une fonction
Help on method randint in module random:
randint(a, b) method of random.Random instance
    Return random integer in range [a, b], including both end points.
```

Python est un livre avec un grand nombre de module, c'est ce qu'on appelle la librairie standard



