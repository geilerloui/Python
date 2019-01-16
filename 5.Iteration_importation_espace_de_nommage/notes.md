# Semaine 5. Itération, importation et espace de nommage

**Ce qu'il faut connaitre:**

- Itérable, Itérateur et itération
- ToDO

[TOC]

### 1. Itérable, itérateur et itération

**<u>Itérateur</u>:**

Les itérateurs sont ce qui vous permet de parcourir les objets de manière simple et intuitive, ils définissent une interface unique que l'on appelle le **protocole d'itération**. En plus de la simplicité et de l'efficacité de ce mécanisme, la notion d'itérateur permet de découpler l'objet qui itère de l'objet qui contient les données. L'avantage est que maintenant avec un itérateur, nous avons un objet extrêmement simple et compact que l'on peut parcourir de manière intuitive

**<u>Itérable</u>:**

Un objet que l'on peut parcourir grâce à un itérateur s'appelle un objet itérable. Donc un itérable est un objet que l'on peut parcourir de multiples fois. Tous les types built-in sont itérables sauf évidement les types numériques.

**<u>Exemple: Le protocole d'itération de la boucle FOR</u>:**

Nous allons maintenant déconstruire la manière dont fonctionne une boucle for sur les itérables, via l'exemple suivant :

En fait, la boucle for va faire les opérations suivantes :

1. Elle va commencer par récupérer l'itérateur sur cet ensemble
2. La boucle for après avoir appelé cette méthode ```iter``` va appeler une méthode ```next``` utilisable autant de fois qu'il y'a d'éléments dans la Sequence
3. Une fois qu'on ne peut plus obtenir d'éléments, on obtiendra une exception ```StopIteration```

```python
>>> s = {1, 2, 3, 'a'}
>>> it = iter(s) # step 1
>>> it # On a crée un itérateur sur notre objet set
<set_iterator at0x256fddd>
>>> next(it)
1
>>> next(it)
2
```

**<u>Comparaison entre les Itérables et les Itérateurs</u>:**

Un itérable est un objet qui a une méthode ```__iter()__``` que l'on appelle une méthode spéciale, qui lorsqu'on l'appelle retourne un nouvel objet qui s'appelle un itérateur

Un itérateur est un objet qui a une méthode ```__iter()__``` qui retourne l'itérateur lui-même et une méthode ```__next()__```  qui à chaque fois qu'on l'appelle va retourner un nouvel élément jusqu'à l'appel de l'exception. Donc un **itérateur ne peut se parcourir qu'une seule fois**

<img src="media\iterable.png" height="150px">

**<u>Il y'a plusieurs questions qu'on peut se poser</u>:**

* Pourquoi est-ce qu'on a une méthode iter sur l'itérateur qui retourne l'itérateur lui-même ?

La raison est simple, c'est qu'un objet itérable est itérable parce qu'il a une méthode iter qui retourne un itérateur, et bien un itérateur est également itérable parce qu'il a une méthode iter qui retourne un itérateur. Le fait que ce soit lui-même ne change rien à l'affaire ça reste un objet itérable. Par conséquent, tous les mécanismes d'itération, boucle for, compréhension de liste, peuvent prendre soit un itérable, soit un itérateur et le parcourir

* Pourquoi est-ce qu'on a deux notions itérables et itérateur, puisque les boucles for peuvent prendre ces deux objets de manière indifférente ?

En fait, ces deux objets sont conceptuellement différents, l'itérable est l'objet qui contient les données et l'itérateur est un objet simple et compact qui parcourt les données qui sont contenues dans l'itérable. Lorsqu'on manipule des objets itérables, comme des liste, c'est le mécanisme d'itération qui va s'occuper de parcourir ces objets. Mais dans certains cas on n'aura pas d'itérable, on aura directement un itérateur. Ce qui est le cas des fichiers, ce qui est logique, si le fichier prend plusieurs megabytes il ne faudrait pas le charger entièrement en mémoire. Or, l'itérable implique qu'on a que l'objet contienne toute l'information en mémoire.

**<u>Exemple d'itération</u>:**

```python
>>> a = [1, 2]
>>> b = [3, 4]
# elle prend le premier element de chaque liste les met dnas le tuple etc.
>>> z = zip(a, b)
zip at 0x151
>>> z is iter(z) # est-ce que z est son propre iterateur (on peut ne le parcourir que une seul fois)
True
>>> [i for i in z]
[(1, 3), (2, 4)]
>>> [i for i in z] # on le rappel mais la liste a ete consommee elle est vide
[]
```

La création d'un itérateur est très peu coûteux, vu qu'on ne le parcourt qu'une seule fois, contrairement à une liste qui restera en mémoire.



### 2. Objet fonction, fonction Lambda, Map et Filter

Python est un langage **multi-paradigme** qui supporte la programmation objet mais aussi certains concept de programmation fonctionnel. 

Nous parlerons **des fonctions lambda**. nous allons expliquer que les fonctions étant des objets, on peut les passer comme argument à d'autres fonctions et nous allons parler des **fonctions built-in map et filter.**

**<u>Les fonctions Lambdas</u>:**

Les fonctions Lambda, sont des fonctions anonymes, c'est une facilité d'utilisation pour écrire du code plus simple même si elle a les même fonctionalités qu'une fonction classique. Sa principale différence est que c'est une expression. On peut définir une fonction Lambda partout où on peut avoir une expression, lorsqu'on écrit une liste, un dictionnaire ou lors du passage d'argument à une fonction.

```python
>>> lambda x: x**2 - 1
<function <lambda> at 0x000055AWE>
>>> carre = lambda x: x**2 - 1 # maintenant qu'on lui a donne un nom ça perd de son interet
>>> carre(1)
0
```

Cas plus réel :

```python
# on attend une fonction en entrée
def image(f):
    for x in range(10):
        print(f"{f(x)}: {x}")
        
>>> image(lambda x: x**2 - 1)
-1: 0
 0: 1
etc.
```

**<u>Les fonctions Map et Filter</u>:**

Fonctions map et filter: ce sont deux primitives de programmation fonctionnelles, map applique une fonction à chaque élément d'un itérable.

```python
def carre(x):
    return x**2 - 1
>>> m = map(carre, range(10))
>>> list(m)
[-1, 0, 3 ...]
>>> filter(lambda x: x%2 == 0, range(10))
<filter object at 0x00002E>
>>> list(f)
[0, 2, 4, 6, 8]
```

En Python moderne, on préfère utiliser les compréhensions de liste plutôt que Map et Filter. Même si, ces deux fonctions ont un avantage majeur, lorsqu'on fait une compréhension, une liste temporaire est crée qu'on a pas forcément besoin si par exemple on fait la somme des elements de la liste on n'en a pas besoin.

Map et Filter crée un objet extrêmement compact, on verra plus tard les expréssions génératrices qui utilisent la fonctionalite des deux.



### 3. Compréhension de listes, sets et dictionnaires

Nous allons voir quelques exemples :

* Compréhensions de listes :

Nada

* Compréhensions de set :

Nada

* Compréhensions de dictionnaires :

```python
>>> prenoms = ['ana', 'eve', 'ALICE', 'Anne', 'bob']
>>> prenoms.extend(prenoms)
# on veut la liste des prenoms uniques a partir d'une comprehension
# le set est cree a la vole en parcourant la liste de prenoms, il n'y aucune creation de liste entre temps
>>> a_prenoms = {p.lower() for p in prenoms if p.lower().startswith('a')}
{'alice', 'ana', 'anne'}

```

* Compréhensions de dictionnaire :

```python
>>> ages = [('ana', 20), ('EVE', 30), ('bob', 40)]
>>> ages = dict(ages)
>>> ages
{'EVE': 30, 'ana': 20, 'bob': 40}
>>> ages_fix = {p.lower():a for p, a in ages.items()}
>>> ages_fix # tout les prenoms sont en minuscules
{'eve': 30, 'ana': 20, 'bob': 40}
>>> ages_fix = {p.lower():a for p, a in ages.items() if a < 40}
{'eve': 30, 'ana': 20}
```



### 4. Expressions et fonctions génératrices

Précédemment, nous avons vu les notions de compréhensions, compréhensions de liste, de set et de dictionnaire. Et nous savons que les compréhensions constituent un moyen extrêmement simple de parcourir les itérables et d'appliquer des traitements sur ces itérables. Ces compréhensions cependant, ont un inconvénient majuer, c'est qu'elles créen des structures de données temporaires. Or, si on veut juste calculer la sommes d'une liste d'éléments on va créer une liste pour rien.

La solution est la notion d'expression génératrice, ce qui sera retourné est un itérateur et non pas une liste temporaire :

* Compréhension de liste

```python
>>> carre = [x**2 for x in range(1000)]
>>> sum(carre)
332833500
```

* Expression generatrice

```python
>>> carre = (x**2 for x in range(1000))
>>> carre 
<generator object <genexpr> at 0x0000EA481>
>>> sum(carre)
332833500
>>> sum(carre) # l'itérateur a été consommé 
0
```

On peut **chaîner les expressions génératrices** ce qui permet d'avoir une succession de traitements qui peut s'exécuter sans jamais avoir besoin de créer des objets temporaires. 

On va créer une premier générateur qui génère des carrés, puis un second, qui va détecter les palindromes, comme 121.

```python
>>> gen_carre = (x**2 for x in range(1_000))
# on veut creer un generateur qui est capable de detecter les palindromes dans les carres
>>> palin = (x for x in gen_carre if str(x) == str(x)[::-1])
>>> palin
<generator object <genexpr> at 0x0000EA151>
>>> list(palin)
[0,
 1,
 4,
 9,
 121,
 484,
 676,
 ...    
]
```

**Les Fonctions génératrices - yield :**

On voit que la notion d'expression génératrice est très puissante et extrêmement souple. Il faut donc éviter de créer des objets lorsqu'on n'en a pas besoin. Cependant, l'expression génératrice a une limitation, c'est que dans une expression génératrice exactement comme dans une compréhension de liste, on ne peut mettre qu'une expression.

En fait les expressions génératrices on été généralisées par ce qu'on appelle une Fonction génératrice, on pourra créer des fonctions qui se comportement comme des expressions génératrices.

On peut comparer les deux modèles :

* **Les fonctions :** Lorsqu'elle est définie, elle retourne forcément une valeur, une fois qu'elle a été retourné, la fonction détruit toutes ses variables locales, et ne garde aucun état entre deux appels
* **Les fonctions génératrices :** Elle va retourner un objet itérateur

**<u>Exemple-1</u> :**

```python
def gen():
    yield 10

# on a pas de valeurs de retours, cet appel cree un objet 
>>> gen()
<generator object gen at 0x000021515>
>>> g = gen() # on reference ce generateur
>>> next(g)
10
>>> next(g)
Exception StopIteration
```

**<u>Exemple-2</u> :**

On va faire un générateur un tout petit peu plus sophistiqué. Le générateur lorsqu'il a été crée attend. On appelle next, ça déclenche son exécution, on va jusqu'au premier yield, le premier yield retourne x. Ensuite, le générateur se stoppe, il attend avec tout son état qui est préservé. On rappelle next, ce qui fait qu'on va reprendre l'exécution juste là ou on s'est arrêté

```python
def gen(x):
    yield x
    x = x + 1 
    yield x
    
>>> g = gen(10)
>>> next(g)
10
>>> next(g) # on yield le second x
11
```

**<u>Exemple-3</u> :**

Un exemple plus realiste, mais qui aurait pu être mis directement dans une expression génératrice

```python
def carre(a, b):
    for i in range(1, b):
        yield i **2
        
>>> c = carre(1, 10)
>>> list(c)
[1, 4, 9, ... ]
```

**<u>Exemple-4</u> :**

Cet exemple est plus adapté aux fonctions génératrices. On va créer un générateur qu'on va appeler Palindrome et on va lui passer directement un objet itérable à l'intérieur de la boucle for on va regarder si i est une instance soit d'un string soit d'un int (avec isinstance).

Dans l'exemple, même si on utilise une liste temporaire, on devrait plutôt travailler directement avec un itérateur. Par exemple, en lisant, en ouvrant un fichier qui contient sur chaque ligne, un nombre ou une chaine de caractères et passer directement le fichier au générateur de Palindrome.

```python
# si i est un type entier ou chaine de caractere, il le convertit en chaine de caractere puis regarde si c'est un palindrome
def palin(it):
    for i in it:
        if (isinstance(i, (str, int)) and 
            str(i) == str(i)[::-1]):
            yield i
            
>>> p = palin([121, 10, 12321, 'abc', 'abba'])
>>> list(p)
[121, 12321, 'abba']
```

**<u>Exemple-5</u> :**

```python
# on peut ausi travailler directement sur un iterateur, comme un fichier et le passer directement dans la fonction temporaire
>>> list(palin(x**2 for x in range(1000)))
[0, 1, 4, ...]
```



### 5. Modules et espace de nommage

Lorsque nous avons parlé de la notion de portée de variable, nous avons expliqué que nous pouvions avoir une variable d'un nom donné, par exemple une variable x, qui coexiste dans le même fichier à l'intérieur d'une fonction et à l'intérieur d'un module. Nous avons également expliqué que les modules isolaient complètement leurs variables.

Ce mécanisme d'isolation des variables se nomme **Espace de nommage**, il permet de regrouper un ensemble de variables appartenant à un objet. En Python, les modules, les fonctions et les classes et les instances définissent des espaces de nommage. 

* En C : Cette notion n'existe pas il faut donc bien faire attention lors de l'écriture du code
* En Java : Il faut créer une classe à chaque fois pour définir un nouvel espace de nommage
* En Python : Les modules isolent les variables

**<u>Exemple</u> :**

On va lancer le code à partir de ```egg.py```  , on se focalisera sur les modules, on ne verra pas les espaces de nommages des fonctions qui sont normalement crée à l'appel de la fonction. L'appel de l'import va exécuter le contenu de spam.py en premier, pour ensuite revenir à egg.py. Ce qui va nous permettre d'importer le module spam et de créer l'objet module spam etc

<img src="media\esp_nommage.png" height="250px">



**Effet de bord et isolation des objets**

En résumé, la notation ```objet.attribut``` permet d'accéder à ```attribut``` dans l'espace de nommage de ```objet```. Les espaces de nommage en Python isolent les variables, mais ils n'isolent pas les objets. Par conséquent, avec des mécanismes d'espace de nommage, on peut tout de même avoir des références partagées vers des objets mutables. Donc, on a deux variables dans deux espaces de nommage qui peuvent référencer le même objet. Cet objet étant mutabe, si on le modifie, il sera modifié par effet de bord donc les deux variables dans les deux espaces de nommage différents veront cet objet modifié.

**Comment sont implémentés les espace de nommages ?**

Ils sont implémentés en général sous forme de dictionnaires. La clef du dictionnaire correspond au nom de la variable, la valeur correspondant à cette clef va être la référence vers l'objet qui est référencé par la variable. Donc lorsque on ajoute une variable dans un module, ça va créer une entrée dans l'espace de nommage de ce module, donc une clef correspondant au nom de la variable et une valeur qui est une référence vers l'objet associé à cette variable.



### 6. Processus d'importation des modules

Nous allons parler du processus d'importation des modules, i.e. les différentes étapes que va suivre l'interpréteur Python du moment où on tape l'instruction import jusqu'au moment où on à l'objet module.

Nous allons importer le ```module os```. ```Os``` a deux rôles, il va définir le nom du fichier qui va être cherché sur le disque dur, qui va s'appeler ```os.py```, et ce nom, ```os```, va également définir le nom de la variable qui va référencer l'objet module (on peut le voir dans le code ci-dessous)

```python
>>> import os
>>> print(os)
<module 'os' 'C:\\Users\\...\\os.py'

```

Regardons maintenant les différentes étapes qui vont se dérouler lorsqu'on tape import os :

* **Etape 1:** il faut trouver le fichier sur le disque dur, l'interpréteur va regarder dans un certains nombre de répertoires. Il commence par regarder si il est défini dans le répertoire courant, là où on a démarer l'interpréteur, puis dans la variable système PYTHONPATH et sinon ce sera dans le répértoires des librairies standards.

```python
>>> os.environ['PYTHONPATH']
'C:\\Users\\louis\\Desktop'
```

Lorsqu'on a un doute sur le chemin de recherche, on peut regarder dans la variable suivante, il donnera tout les chemins qui sont suivis par l'interpréteur Python dans l'ordre. 

```python
>>> import sys
>>> sys.path # renvoie tous les chemins de recherche
[ '',
  'C:\\Users\\SESA476345\\AppData\\Local\\Continuum\\anaconda3\\python36.zip',
  'C:\\Users\\SESA476345\\AppData\\Local\\Continuum\\anaconda3\\DLLs',
  'C:\\Users\\SESA476345\\AppData\\Local\\Continuum\\anaconda3\\lib',
  'C:\\Users\\SESA476345\\AppData\\Local\\Continuum\\anaconda3',
  'C:\\Users\\SESA476345\\AppData\\Local\\Continuum\\anaconda3\\lib\\site-packages',
  'C:\\Users\\SESA476345\\AppData\\Local\\Continuum\\anaconda3\\lib\\site-packages\\Babel-2.5.0-py3.6.egg',
...]
```

* **Etape 2:** Une fois qu'on a trouvé le fichier module, on va devoir le pré-compiler, ce qui consiste à générer ce qu'on appelle du bytecode, qui est en général au format ```.pyc``` . Tout ces fichiers bytecode vont être mis dans un répertoire qui s'appelle ```__pycache___```
* **Etape 3:** Une fois que l'interpréteur Python a généré le bytecode, il va l'évaluer pour générer l'objet module.

Le processus d'importation étant une opération coûteuse, l'interpréteur, lorsque vous faites de multiples import vers le même module, ne va imorter ce module qu'une seule fois, et il ensuite créer des références partagées vers cet objet module. C'est pourquoi il est très important de comprendre qu'un objet module est mutable, et que la manière d'importer un module peut avoir un impact sur l'espace de nommage de ce module.



### 7. Importation des modules et des espaces de nommage

Nous avons vu dans la section 6, que lorsqu'on importe un module, le module n'était importé qu'une seule fois, donc de multiples import vont simplment créer des références partagées vers cet objet module. Mais l'objet module n'existe qu'en un seul exemplaire. L'objet module étant mutable, il est possible de le modifier. Nous allons couvrir la relation entre les différents mécanismes d'importation avec l'espace de nommages des modules.

Regardons l'impact de l'importation sur les espaces de nommages

<img src="media\num6.1.png" height="250px">



On va faire quelques petits changements dans ```egg.py``` en rajoutant deux lignes de codes:

```python
# on regarde dans l'espace de nommage de egg.py
>>> print(x)
2
# On regarde dans l'espace de nommage de spam
>>> print(spam.x)
1
```

On va utiliser la **mutabilité des modules** toujours dans egg.py

```python
>>> x = 2
>>> spam.x = 3
```

<img src="media\num7.png" height="250px">



<u>**Une autre manière d'importer avec from**:</u>

Lorsqu'on fait le premier print dans ```egg.py``` on va retourner ```1``` qui est ce vers quoi les deux ```x``` référent, ligne 3 on le modifie et on lui donne une référence vers un nouvel objet ```3```.

<img src="media\num6.2.png" height="250px">

<u>**Résumer**</u>:

Le risque de collision de from peut se produire si on a deux variables qui ont le même noms.

<img src="media\num6.3.png" height="150px">

<u>**Il est plus recommander d'appeler import que from**:</u>

Le from est en pratique quelque chose d'utile, mais on ne l'utilise pas lorsqu'on utilise systématiquement une fonction définie dans un autre module. D'une manière générale, il est préférable d'utiliser simplement la notation import le module. Il faut noter aussi que lorsqu'on fait from module import attribut ; on n'a pas de référence vers l'objet module. Donc, on pourrait croire que cet import va coûter moins de mémoire. En fait, ça ne change absolument rien, puisque l'objet module va quand même été créé, et va quand même être maintenu en mémoire tant que votre programme est en cours d'exécution. Donc ces deux méthodes d'importation n'ont aucun impact en terme d'occupation de mémoire.





