# Prise de notes

## Semaine 1. Introduction au MOOC et aux outils Python

Ce qu'il faut connaitre:

- Le ```garbage collector```
- Qu'est ce que le typage dynamique ? Espace de nommage et Espace des variables
- Pourquoi dit-on que Python est un langage de Type fort

### 5. Notions de variables, d'objet et typage dynamique

**<u>Tout est objet en Python:</u>**

Un objet est constitué de :

* **Données**
* **Méthodes :** Un ensemble de mécanisme pour contrôler les données
* **Un "type"** qui est le comportement par défaut, ce qui lui octroie un certains nombres de données et méthodes par défaut

 **Exemple:**

De création d'objet

```python
>>> 'spam'
```

<img src="media\cours_5.1.png" height="175px" />



**<u>Les variables référencent (nomment) les objets:</u>**

Signifie qu'on donne un nom aux objets par l'intermédiaire des variables, on dit que les variables référencent les objets.

**Exemple:**

On veut nommer l'entier 1, la variable "note" va référencer l'objet 1

```python
>>> note = 1
```

**<u>Le Typage Dynamique:</u>**

On représente **l'espace des objets** qui contient tous les objets dans l'ordinateur, et **l'espace des variables** (espace de nommage) qui vont référencer ces objets. On dira que Python est à **Typage Dynamique**, i.e. que le type n'est pas lié à la variable qui référence l'objet mais il est lié à l'objet. Donc, si on veut changer le type d'une variable il faudra créer un nouvel objet qui sera ensuite référencé

<img src="media\cours_5.2.png" height="150px" />



```Python
# Exemple :

>>> a = 3
# Lorsqu'on fait un retour chariot Python va exécuter trois operations:
# 1. On créée l'entier 3
# 2. Il va créer une variable a
# 3. Il va créer une référencer entre la variable a et l'entier 3

# Va changer la référencer de la variable a vers le nouvel objet 'spam
>>> a = 'spam'
```

**<u>Le Typage fort:</u>**

Python est un langage à typage fort, i.e. que le typage est lié aux objets et que l'objet va garder le même type durant toute l'exécution du programme, alors que la variable elle pourra changer de type (en référençant un autre objet)

**<u>Le Garbage Collector:</u>**

Si un objet n'a plus de référence on à un mécanisme qui s'appelle ```garbage collector``` qui va libérer la mémoire de l'ordinateur une fois que l'objet n'est plus référencé. C'est entre autre ce qu'il se passe lorsqu'on veut changer le type d'une variable.



### 6. Les types numériques

Il existe quatre types numériques en Python:

* int
* float
* complex
* bool (un sous-ensemble des entiers) représentés par ```True``` ou ```False```, il est codé 0 et 1

Les entiers sont des objets de précisions illimités, i.e. que si on multiple deux entiers il n'y aura pas de nombre manquants

Les floats sont par contre limités
