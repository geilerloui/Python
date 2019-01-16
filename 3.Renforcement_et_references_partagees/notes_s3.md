## Semaine 3. Renforcement des notions de base, références partagées

**Ce qu'il faut connaitre:**

- Les trois pré-requis des fichiers - Encodage, Itération et le Context Manager
- Les Raw String
- Définition du ```tuple unpacking``` et de ```tuple extended tuple unpacking```
- Les types Sequences ont été optimisé pour quoi ? et quels sont leur(s) désavantage(s)
- Quel est le rôle d'une table de Hash et comment les implémenter en Python ?
- Dans le dictionnaire, qu'est ce que les méthodes keys, values et items retournent ?
- Que se passe t-il quand on convertit une liste en set ?
- Le set a été optimisé pour deux applications
- Le mécanisme de Bubbling avec les Exceptions 

* Exemple de référence partagés, dans quel cas peut-on avoir un effet de bord ?
* Quel est la solution ? (shallow copy) Dans quel cas elle ne marche pas - si une liste référence une liste on peut faire une deep copy, ce qui va recopier de manière récursive tout les objets

[TOC]



### 1. Les fichiers

L'objet fichier est décomposé en trois parties :

- l'encodage
- l'iteration
- le context manager

**<u>L'encodage et le décodage</u>:**

L'objet va se charger d'encoder et de décoder pour nous, dans l'exemple des Raw String, on peut voir qu'on a mis par défaut UTF-8 qui est celui qui est recommandé. De plus, il est important de bien fermer le fichier à la fin de son utilisation.

**<u>Les raw string</u>:**

Il y'a un point très important a respecter, les chaines de caractères avec des backslash vont interpréter les backslash, notamment \t est un caractère de tabulation, on doit donc doubler tout les backslash ou alors ce qui est recommandé c'est de transformer notre chaine en Raw string avec le r

```python
In [1]: f = open('c:\temp\spam.txt ')
# rajout du raw string
In [2]: f = open(r' c:\temp\spam.txt ', 'w', encoding='utf-8')
In [3]: for i in range(100):
			f.write(f "ligne {i+1} \n")
In [4]: f.close()
```

**<u>L'itération</u>:**

Les fichiers sont des iterateurs i.e. qu'on peut les mettre directement dans une boucle for

**<u>Le context manager</u>:**

Le système des fichiers : le programme Python va discuter avec l'OS et c'est l'OS qui va ouvrir et fermer les fichiers, il y'a bien sur un nombre limité de fichier sur lequel on peut interagir. Donc, sans rien fermer on risque de se retrouver avec des blocages. 

En Python vu que tout est objet, les fichiers etc. on pourrait se demander si il n'existe pas un mecanisme qui detecte par lui-meme qu'on à plus besoin d'un objet et le ferme automatiquement.

C'est via un Protocole, le **context manager** qu'on va trouver cette fonctionalité.  Un objet fichier implémente ce protocole

```python
with open(r'c:\temp\spam.txt', 'r', encoding='utf-8') as f:

# ce bloc d'instruction va etre execute des qu'on en sort on va appeler une methode qui s'appelle exit sur le fichier qui va le fermer; si il y'a aussi un probleme d'exception il fermera le fichier aussi
	for line in f:
		print(line) # on execute le context manager
```

**<u>Le context manager sur fichier binaire</u>:**

```python
# on rajoute un 'b' pour binaire
with open(r'c:\temp\spam.bin', 'bw') as f:
	for i in range(100):
		f.write(b'\x3d')
```

### 2. Les tuples

Le tuple est un objet de type sequence et immuable contrairement au list qui sont mutable; on garde par contre toutes les autres proprietes des liste

```python
In [1]: t = (4, ) 
In [2]: type(t)
Out [2]: tuple
```

Si on ne donne pas deux elements Python va considerer qu'on cherche juste a grouper des operations et lui donnera ici un type int

```python
In [3]: t = (4)
In [4]: type(t)
Out [4]: tuple
```

Remarque: les parenthèses sont facultatives

```python
In [5]: t = True, 3.4, 18
In [6]: t = True, 3.4, 18
Out [6]: (True, 3.4, 18)
```

Le tuple est très utilise pour une opération qui s'appelle le ```tuple unpacking``` 

```python
In [7]: (a, b) = [3, 4]; print(a); print(b)
Out [7]: 3, 4 # la variable a reference l'entier 3 etc.
```

On peut aussi la réécrire pour alléger la notation en enlevant les parenthèses

```python
In [8]: a, b = 3, 4
```

On a aussi la notion de ```tuple extended tuple unpacking``` permet d'isoler des éléments 

```python
In [9]: a = list(range(5))
In [10]: x, *y = a
In [11]: x
Out [11]: 0
In [12]: *y # y va reference un tuple qui va reference tous les elements restants
Out [12]: [1, 2, 3, 4]
```

On peut aussi le faire dans l'autre ordre ou cette fois-ci y n'aura que un seul élément

```python
In [13]: *x, y = a
```

Le tuple est utilise principalement pour du tuple unpacking et aussi surtout comme clef de dictionnaire

### 3. Les tables de Hash

C'est une structure de donnée qui permet de répondre a certains limitations des types sequences. Les **sequences** ont été **optimisé pour l'accès la modification et l'effacement** en fonction d'un numéro de séquence, cependant ils n'ont pas été optimisé pour le test d'appartenance

```python
#Contrainte 1:
In [1]: %timeit 'x' in range(1_000_000)
28.3 ms

# on voit que le complexite est lineaire avec le nombre d'elements elle augmente de plus en plus
# Contrainte 2: on veut changer l'indice numerique en chaine de caractere
In [2]: t = [18, 35]
In [3]: t ['alice'] = 35
TypeError: list indices must be integers or slices, not str
```

La table de hash permet de répondre a ces deux limitations:

Dans l'exemple, lorsqu'on passe à notre fonction f(x) (fonction de hash) un objet elle va nous renvoyer une valeur entre 1 et 6. il va créer une correspondance entre un objet et une case dans le tableau; On créé une table de hash

<img src="media\table_hash.png" height="200px" />

eve: clef

34: la valeur

on passe eve a la fonction de hash, il la met dans la case 2 etc

lors de la lecture on passe eve a la fonction de hash elle va lire dans le tableau et renvoyer la valeur

On peut noter que la table de hash est limite, on met jo dans la table 

si le tableau est trop petit on va avoir beaucoup de collision i.e. que deux clefs vont correspondre a la même entre dans le tableau. Dépend aussi de la fonction de hash qui doit bien repartir les clefs.

la complexite est en o(1) avec cette table i.e. qu'elle est independante du nombre d'elements a l'interieur. En Python on a deux type les set et les dictionnaires pour creer des tables de hash



### 4. Les dictionnaires

Définition: **Ce sont des tables de hash; on a donc un temps d'accès, d'insertion, d'effacement et un test d'appartenance qui sont indépendant du nombre d'élément**. De plus, les dictionnaires sont des objets mutables on peut donc les modifier en place.

Dans un dictionnaire on peut avoir comme clef un objet hashables; en Python tout les objets immuables sont hashables et tout les objets mutables ne sont pas hashables. Pourquoi ? car la fonction de hash doit faire un calcul sur la clef or si la clef change au fil du temps ca ne marchera plus.

Le dictionnaire est une collection de couple clef valeur (appelé ```items``` en Python), **il n'y a aucune garantie d'ordre** dictionnaire est sans ordre

```python
In [1]:  age = {'ana': 35, 'eve': 30, 'bob': 38}
# 2nd possibilite
In [2]: a = [('ana', 35), ('eve', 30), ('bob', 38)]
In [3]: age = dict(a)
# le dictionnaire etant mutable on peut le modifier
In [4]: age['ana'] = 1458
# on peut supprimer un couple clef valeur
In [5]: del age['bob']
```

On a aussi les opérations suivantes: (équivalente aux opérations des listes)

```python
In [6]: 'bob' in age
Out [6]: False
In [7]: age.keys()
Out [7]: dict_keys(['ana', 'eve'])
In [6]: age.values()
Out [6]: dict_values([35, 30])
In [6]: age.items()
Out [6]: dict_values([('ana', 35), ('eve', 30)])
```

Les méthodes keys, values et items retournent **un objet qu'on appelle une vue**, c'est un objet sur lequel on peut itérer et on peut aussi faire un test d'appartenance. La vue est mise à jours en même temps que le dictionnaire. [Objet Proxy](http://sametmax.com/les-vues-sur-des-collections-en-python/) 

```python
# k est la vue sur les clefs
In [7]: k = age.keys()
Out [7]: dict_keys(['ana', 'eve'])
In [8]: age['bob'] = 25
In [7]: k # on peut voir que la vue a ete automatiquement mise a jours
Out [7]: dict_keys(['ana', 'eve', 'bob'])
```

La lecture se fait avec ```tuple unpacking``` 

```python
In [8]: for k, v in age.items():
			print(f"{k}, {v})
```

### 5. Les ensembles

Très proche des dictionnaires, **la principale différence qu'il stocke que les clefs il n'y a pas de valeurs** correspondante. Il a été crée pour des opérations spécifiques / optimiser, (1) pour garder le nombre unique d'éléments d'une séquence, (2) pour faire aussi des tests d'appartenances sur une séquence.

```python
In [1]: s = set()
In [2]:  s = {1, 2, 3, 'a', True}
In [2]:  1 in s # le test d'appartenance est le meme avec tous les types built-in
Out [2]: True
```

On peut aussi faire les opérations classiques sur les ensembles

```python
In [3]:  s1 = {1, 2, 3}; s2 = {3, 4, 5} #
In [4]:  s1 - s2 
Out [4]: {1, 2}
In [5]:  s1 | s2
Out [5]: {1, 2, 3, 4, 5}
In [6]:  s1 & s2
Out [6]: {3}
```

Pour ce qui est du test d'appartenance, la différence entre une liste et un set est que la liste on accède a la première case mémoire on compare l'objet dans la liste avec 0 c'est le même objet donc on retourne vrai. Avec le set on fait un calcul avec la fonction de hash sur 0 on accède a une case et 

```python
In [7]: %timeit -n 50 0 in a
```

Convertir une liste en set revient a passer la fonction de hash a chacun de ces éléments

### 6. Les exceptions

Mécanisme de notification d'erreur, c'est utilise couramment dans le fonctionnement normal d'un programme

```python
In [1]: def div(a, b):
			print(a/b)

In [2]: div(1, 0)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in div
ZeroDivisionError: division by zero
```

On peut décortiquer l'erreur, la dernière ligne donne le nom de l'exception *ZeroDivisionError* , on peut capturer l'exception

```python
def div(a, b):
    try:
        print(a/b)
	except ZeroDivisionError:
        print("attention, division par zero")
    except TypeError:
        print("il faut des int")
    print("continuons")
    
>>> div(1, 2)
0.5
continuons
>>> div(1, 0)
attention, division par zero
continuons
>>> div(1, '0') # on va corriger le probleme en ajoutant l'exception TypeError
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in div
TypeError: unsupported operand type(s) for /: 'int' and 'str'
>>> div(1, '0')
il faut des int 
continuons
```

En Python on peut avoir une close except sans spécifier aucune exception, ce qui est une très mauvaise pratique, car ca cachera les exceptions qui peuvent se produire.

**Une caractéristique des exceptions et qu'elles bubble i.e. qu'elles vont remonter notre pile d'exécution jusqu'à arrêter notre programme**

Exemple de mécanisme de bubbling

```python
def div(a, b): 
    print(a/b)
    
def f(x):
    div(1, x)

# ma fonction va executer la fonction div mais tant que ma fonction div n'a pas retourner de valeur la fonction f reste en cours d'execution. On dit qu'elle reste dans la pile d'execution; La pile d'execution va contenir toutes les fonctions de notre programme qui ont ete appele mais qui n'ont pas retourne de valeurs
>>> f(1)
1.0
# la fonction a ete appele par f, l'exception va donc remontrer la pile d'execution elle va sortir dans ma fonction f elle n'a pas ete capture elle va remontrer la pile et arreter le programme
>>> f(0) # on va avoir une exception
Traceback (most recent call last):
    File "<pyshell#11>", line 1 in <module>
    f(0)
    File "C:...", line 5 in f
    div(1,x)
    File "...", line 2 in div
    print(a/b)
    ZeroDivisionError: division by zero
```

Le mécanisme de bubbling a deux avantages majeurs:

1. On peut capturer mon exception n'importe ou dans la pile d'exécution donc on peut tout a fait capturer l'exception au niveau du print, au niveau de ma fonction f dans l'appel de div 
2. Ma trace d'exécution va être capable de dire exactement ou est passe l'exception et va donc aider fortement aider au diagnostic (on peut le lire dans le Traceback on commence par le haut et on redescend la pile d'exécution, ici on remonte au point ou l'appel de f(0) a pose problème

Une bonne pratique en Python est de capturer l'exception au plus près de l'endroit ou elles se produisent; pour savoir les exceptions qu'on va avoir besoin il faudra lire en détails la documentation des modules ou objets qu'on utilisera

### 7. Les références partagés

Ce mécanisme permet d'accéder aux attributs de n'importe quel objet de notre programme

A gauche on a l'espace des variables (espaces de nommages) et a droite espace des objets. 

A l'objet 3 qu'on va créer va être associé: un compteur de références qui est en rouge qui représente le nombre de variable qui référence cet objet, un champ de type qui référence le type de l'objet; Python étant a typage fort le type de l'objet une fois crée ne pourra pas être changé.

#### Cas 1:

Le compteur de référence passe a 1 une fois que **a** fait référence a 3 si on crée un nouvel objet et qu'on l'assigne à la variable ```a``` ici 'spam' le compteur de référence de **a** va repasser a 0 et le ```garbage collector``` va prendre un accès sur cet objet et va libérer la mémoire lors d'un cycle de ```garbage collector``` c'est le module GC qui va gérer ça automatiquement. 

<img src="media\ref_part1.png" height="250px" />

#### Cas 2:

b = a, on va avoir deux références vers l'objet entier 3, c'est ce qu'on appelle une référence partagé, c'est une référence partagé par plusieurs variables, lorsqu'elle est faite avec un objet immuable il n'y a aucun effet de bord possible, par contre si c'est un mutable il y'a un risque d'effet de bord.

```python
>>> a = 3
>>> b = a
>>> a = 'spam'
```

![ref_image_part2.3](media\ref_image_part2.3.png)



#### Cas 3:

a = [1, 2]

L'objet liste va référencé un objet 1 et un objet 2; la référence de la liste vers l'entier 1 va incrémenter **le compteur de référence est augmente aussi bien si c'est un objet ou une valeur**

On illustre l'effet de bord avec a et b référence la même liste, mais avec **a** on va modifier le premier élément, ce qui par effet de bord va aussi modifier b, qui aura les mêmes valeurs que a.

<img src="media\ref_part3.png" height="250px" />

#### Cas 4:

Comment se prémunir de l'effet de bord

on fait b = a[:] , un slice vide fait une shallow copy de la liste, on va créer un nouvel objet liste qui va référencer les mêmes objets que a

<img src="media\ref_part4.png" height="250px" />

#### Cas 5: 

Lorsqu'on fait une shallow copy on fait juste une copie de l'objet liste par contre toutes les références sont partages si la liste référence un objet mutable la shallow copy ne sera pas suffisante.

a = [1, [2]]

on voit que l'objet spam a été modifie pour a et b **pour un objet mutable qui modifie des mutables la shallow copy n'est pas suffisante** on doit faire une deep copy, qui va recopier de manière récursive tous les objets

<img src="media\ref_part5.png" height="250px" />

#### Cas 6: deep copy

```python
a = [1, [2]]
import copy
b = copy.deepcopy(a)
a[1][0] = 'spam'
>>> a
[1, ['spam']]
>>> b
[1, [2]]
```

Ce mécanisme de référence partages est fondamentale dans le passage des arguments d'une fonction ou aussi bien pour l'ensemble des objets de Python qui cherchent a optimiser l'espace mémoire.

<img src="media\ref_part7.png" height="250px" />

### 8. Introduction aux classes

Nous avons vu les principaux types built-in, on avait remarque qu'ils avaient des comportements assez proche, len() etc. 

On peut écrire nos propres objets qui vont se comporter comme le type built-in ils vont support le in, l'affectation avec les crochets ...

```python
class phrase:
    # on va creer un constructeur
    def __init__(self, phrase):
        # self c'est mon instance courante
        self.mots = phrase.split()
    def upper(self):
        self.mots = [m.upper() for m in self.mots]
    def __str__(self):
        return "\n".join(self.mots)
        
>>> phrase
<class__main___.phrase object at 0x0000021BCE7D>
>>> p = Phrase("je fais un mooc") # on cree une instance de la classe
>>> p.mots
['je', 'fais', 'un', 'mooc']
>>> p.upper()
# on affiche le texte mais on remarque que ce n'est pas comme ca en Python on doit utiliser un print; on va donc creer une classe __str__ dans cet objectif
>>> p.mots
['JE', 'FAIS', 'UN', 'MOOC']
>>> print(p) # va automatiquement appeler __str__
je
fais
un
mooc
```

En plus de str il y'a aussi contain pour les in etc. pour chaque type built-in on peut les implémenter dans notre classe.









