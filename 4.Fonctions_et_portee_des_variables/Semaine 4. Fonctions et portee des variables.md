# Semaine 4. Fonctions et portee des variables

**Ce qu'il faut connaitre:**

- Définition du Polymorphisme, typage dynamique

- Portée lexicale et règle LEGB

- Différence entre paramètre et arguments
  - Les arguments ordonnés, nommés, forme étoile, forme double étoile
  - paramètres ordonnés, optionnels, forme étoile, forme double étoile
  - Le Wrapper est utilisé avec quelle forme ?

[TOC]


### 1. Fonctions

Lorsqu'on créer une fonction, on va avoir un objet fonction et son nom sera la variable

```python
# la variable f reference un objet fonction
def f(a, b, c):
    print(a, b, c)
# on fait une reference partages    
>>> g = f
# lorsqu'on passe des arguments a une fonction (qui sont des objets) ces objets ne sont jamais copier ils sont toujours passe par references
>>> L = []
def add_1(a):
    a.append(1)
>>> add_1(L)
# on voit que la liste L a ete modifie alors qu'on a fait aucun retour; elle a en fait ete modifie par effet de bord car on a une refrence partages vers un objet mutable.
# Le probleme de cet effet de bord est qu'il est fait de maniere implicite or en Python on veut tout de facon explicite il faut donc bien documenter son code
>>> L
[1]
>>> help(list.sort) #cette methode est a effet de bord on voit qu'il a ete ecrit en majuscule dans la documentation pour bien le mettre en avant
Help on method_descriptor:
sort(...)
    L.sort(key=None, reverse=False) -> None -- stable sort *IN PLACE*
# si on ne veut pas modifier la liste on va passer une shallow copy
>>> def add_1(a):
    	a = a[:]
        a.append(1)
        return a
>>> add_1(L)
[1, 1]
>>> L # la liste originel n'a pas ete modifie
[1]
```

**Le polymorphisme**, est le concept consistant à fournir une interface unique à des entités pouvant avoir différents types. Dans l'exemple ci-dessous, la fonction fonctionne aussi bien pour des Strings que pour des Floats.

```Python
def my_add(a, b):
	print(f"{a} et {b}")
    return a + b
>>> my_add('spam', 'good')
'spamgood'
>>> my_add(4.3, 2.3)
6.6
```

Note: Python utilise le typage dynamique on a donc jamais a definir le type des fonctions; mais Python implemente les ```type hints``` ce sont des indications pour ameliorer la documentation du code ou alors pour faire une validation static du code. Cependant, les types hints resteront toujours optionnel, le createur de Python a bien specifie qu'on n'aura jamais besoin de typage statique dans le code

### 2. Tests if/elif/else et opérateurs booléens

Exemple d'expressions

1. Un type built in:

* Faux: False 0 None [] {} () ''
* Vrai: le reste

```python
d = {'marc': 10}
# Si le dictionnaire est vide on ne rentre pas dedans
if d:
    print(d)
```

2. Une comparaison

   les symboles superieur etc

3. Un test d'appartenance in

   avec l'operateur in

4. Un retour de fonction

5. Operateur de test booleen and or not

   Les operateurs sont dit ```short-circuit``` i.e. que avec

   * A and B est True si A et B sont True

     On n'evaluera pas B si on sait que A est faux

   * A or B est True si A ou B est True

   * not A est True si A est False

### 3. Boucles while

```python
a = list(range(1, 10))

while a:
    a.pop()
    # lorsque la liste aura cinq elements on va remonter au debut du while
    # on va sauter le print
    if len(a) == 5:
        continue
    print(a)
[1, 2, 3, 4, 5, 6, 7, 8]
[1, 2, 3, 4, 5, 6, 7]
[1, 2, 3, 4, 5, 6]
[1, 2, 3, 4] # on a saute le numero 5
etc
```

While True fait une boucle infini, on sortira de cet boucle avec un certains critère de test, c'est une utilisation fréquente du while

```python
while True:
    s = input('Quelle est votre question ?')
    if 'aucune' in s:
        break
```

### 4. Portée des variables - règles LEGB

Un bloc de code est un ensembles d'instructions contiguës indentées du même nombre de caractères vers la droite.

Pour dire qu'une variable référence un objet, nous avons plusieurs synonymes, ```definition, affectation, assignation, binding```

**La portée d'une variable détermine de quel endroit du code on peut accéder a cette variable**, Python utilise ce qu'on appelle une **Portée lexicale** i.e. que la portée d'une variable est déterminée en fonction de l'endroit dans le code où cette variable est défini.

**Une variable** définit dans le bloc de code d'une fonction est dite **locale**.

**Une variable globale** est définit en dehors des blocs de fonctions

**Règle LEGBD** est l'ordre dans lequel on va chercher une variable

pour savoir comment trouver les bonnes variables quand on veut les référencer, lorsqu'on référence une variable on va d'abord chercher si elle a été définit localement la ou elle a été définit

si elle n'a pas été définit dans la fonction on remonte a la fonction la plus externe, si elle n'y est pas on la cherche dans les variables globales, sinon dans le module built in

* **L**ocalement: on commence par regarder si elle est défini dans la fonction courante
* **E**nglobante: si ce n'est pas le cas on regarder la fonction externe (dite englobante)
* **G**lobalement: sinon globalement dans le code donc en dehors des fonctions
* **B**uiltins: ou dans le module builtins

```Python
a, b, c = 1, 1, 1

def g():
    b, c = 2, 4
    b = b + 10
    def h():
        c = 5
        print(a, b, c)
    h()
g()
>>> 1 12 5
```

Le résultat va au départ lire dans la fonction h() la variable c, il ne trouve pas a et b, il va remonter une case au dessus mettre b a jours, a n'est pas la il remonte au variable globale et appelé a

```python
print(a, b, c)
>>> 1 1 1
```

Parmi ce qu'on affiche il y'a en fait 4 variables, les trois lettres et une variable ```print``` qui référence un objet fonction, dans le modèle LEGB elle sera lu dans le Builtins

```python
import builtins

print = 10 # on voit qu'on peut redefinir la reference de cet variable
print(1) # on ne peut donc plus utiliser ses fonctions builtins
>>> Error
```

En Python les noms de variables défini dans le Builtins sont d'une couleur différente, ici on la voit en bleu

```python
x = 1
print = builtins.print
print(1) # remarche apres avoir redefini la fonction
```

Ccl: nous venons de voir la notion de portée de variable cpt il n'y a rien de supérieur a une variable globale, chaque module va redéfinir ses propres variables globales. Cela est possible car un mécanisme d'isolation appelé espace de nommage, chaque variable défini dans ces espaces sont appelés variables globales elles sont défini au niveau du module. Chaque module a ses propres variables globales.

Nous verrons plus tard comment faire communiquer les espaces de nommages et accéder aux attributs défini dans d'autres espaces de nommages

### 5. Modification de la portée avec global et nonlocal

```python
# modifier une variable globale depuis ma fonction
a = 'a globale'

def f():
    global a
    a = 'a dans f'
    print(a)
    
>>> print(a)
a globale
>>> f()
a dans f
>>> print(a)
a dans f
```

On voit que le ```a``` dans la fonction qui normalement est supprimé à la fin de l'exécution du code à cette fois-ci modifié la variable globale ```a```.

**Inconvénient majeur:** lorsqu'on modifie une variable globale dans une fonction on fait une *modification implicite* d'une variable globale. Or, en Python on n'aime pas ce qui est implicite

```python
note = 10

# on donne un nom explicite a la fonction
def add_10(n):
    return n + 10

>>> note = add_10(note)
20
```

Meilleur solution, solution explicite avec retour de fonction. Autrement, la maintenance du code va être très difficile

```python
a = 'a globale'

def f():
    a = 'a de f'
    def g():
        a = 'a de g'
        print(a)
    g()
    print(a)
f()
print(a)
>>> # execute code
a de g
a de f
a globale
```

Comment modifier la variable locale a f depuis g. on a l'instruction ```nonlocal```

```python
a = 'a globale'

def f():
    a = 'a de f'
    def g():
        nonlocal a
        a = 'a de g'
        print(a)
    g()
    print(a)
f()
print(a)
>>> # execute code
a de g
a de g # il a ete modifie depuis la fonction g()
a globale
```



### 6. Passage d'arguments et appel de fonctions

On illustre ci-dessous, la différence entre, **Paramètres** vs **Arguments**

```python
# on dit que a et b sont des parametres de la fonction
def sum(a, b):
    return a + b

x, y = 1, 2
# on dit que x et y sont des arguments (passage d'argument) de la fonction qu'on appelle
sum(x, y)
```

**Les arguments nommés**

Il est souvent difficile de se rappeler du noms des différents paramètres de notre fonction, et donc dans l'ordre dans lequel ils ont été écris. C'est pour ça que Python implémente les arguments nommés qui permettent lors la création de la fonction de mettre tel=XXX, nom=XXX, on nomme les arguments qui nous intéresse. 

```python
def agenda(nom, prenom, tel):
    return {'nom': nom, 'prenom': prenom, 'tel': tel}

>>> agenda(tel='0707070', nom='idle', prenom='eric')
```

**Les arguments ordonnés**

On va respecter l'ordre dans lequel les paramètres ont été défini dans la fonction

```python
>>> agenda('idle', 'eric', '0124558')
```

**Arguments : La forme étoile**

```python
def f(a, b):
    print(a, b)
    
# solution 1:
>>> L = [1, 2]
>>> f(L[0], L[1])
1 2
# cette solution n'est pas tres Pythonique il faut eviter d'utiliser des indices
# solution 2: (recommande)
>>> f(*L) # va faire du tuple unpacking
1 2
```

**Arguments : La forme double étoile**

```python
>>> d = {'a': 1, 'b': 2}
>>> f(**d)
1 2
# on peut vouloir afficher des separateurs
>>> print(1, 2, sep=";", end='FIN')
1;2FIN
# il est recommande d'utiliser le double star pour ca
>>> pp = {'sep':';', 'end':'FIN'}
>>> print(1, 2, **pp)
1;2FIN
```

**Paramètres ordonnées et optionnels** 

On peut rajouter un paramètre optionnel qui permettra d'avoir une valeur par défaut si on le spécifie pas lors de l'appel de la fonction. Les paramètres ordonnées sont les premiers paramètres de la fonction

```Python
def agenda(nom, prenom, tel="?"):
    return {'nom': nom, 'prenom': prenom, 'tel': tel}

>>> agenda(nom='idle', prenom='eric')
```

**Paramètres d'une fonction : La forme étoile**

```python
# t va referencer un tuple d'argument qu'on passe a notre fct
def f(*t):
    print(t)
    
>>> f(1)
(1,)
>>> f(1, 2, 3, 4)
(1, 2, 3, 4)
```

**Paramètres d'une fonction : la forme double étoile**

 elle permet de passer n'importe quel argument nommé à la fonction

```python
def f(**d):
    print(d)
    
>>> f()
{}
>>> f(nom='idle', prenom='eric')
{'nom': 'idle', 'prenom':'eric'}
```

Quel est l'intérêt de ces deux formes ?

```python
# en fait dans l'en-tete de la fonction print il y'a une forme etoile pour pouvoir tous les afficher
>>> print(1, 2, 3, 4)
1 2 3 4
```

La **forme double-étoiles** est principalement utilisé lorsqu'on construit des **Wrappers**
