---
title: data.table mon amour
author: Elise Maigné
date: '2022-03-29'
slug: []
categories:
  - cours
tags:
  - R
  - package
  - cours
summary: 'Petite introduction au package data.table'
topics: []
---





```r
library(data.table)
```

Je présente ici mon utilisation du package `data.table` que j'utilise depuis des années sous forme d'une rapide introduction pour s'y mettre. 

Depuis que j'ai commencé à l'utilisé pour pouvoir travailler sur un énorme fichier de données, 
je ne peux plus m'en passer, il est dans tous mes projets !

**Les avantages que j'y trouve :**

 - très rapide
 - particulièrement rapide pour les `merge`
 - très rapide en lecture/écriture de fichiers
 - moins verbeux que le R base
 - pratique pour travailler en groupes de valeurs
 - moins couteux en mémoire
 - est-ce que j'ai dit rapide ?
 
**Des inconvénients :**

 - pas vraiment tidyverse friendly
 - il y a un coût d'apprentissage de la syntaxe
 - travailler avec data.table sans connaitre sa fonction `copy()` est source d'erreurs, je l'ai appris à mes dépends ! 
 
## Usage de base

### Conversion en data.table

Convertir un data.frame en data.table est fait classiquement avec la fonction `as.data.table()`. 

Mais `data.table` fournit la fonction `setDT()` moins gourmande en ressources (utile pour les gros datasets) puisqu'il n'oblige pas à copier l'objet en mémoire, contrairement au traditionnel `<-`.


```r
data(iris)
setDT(iris)
class(iris)
```

```
## [1] "data.table" "data.frame"
```
On peut voir ici qu'un objet `data.table` est aussi dans la classe `data.frame`, ce qui veut dire que le base R s'applique sans problème à un objet data.table. Pas besoin de recoder toutes ses fonctions et ses programmes, bon point !

### Fonction copy (allocation mémoire)

Puisqu'on parle de l'allocation mémoire, il faut savoir que `data.table` utilise le concept de pointeurs (comme en C). 
Le nom de l'objet utilisé dans R n'est en fait qu'un pointeur vers l'objet en mémoire. 

Concrètement ça veut dire que si on copie un objet DF, qui est un `data.table`, dans DF2 en utilisant la façon traditionnelle de R `<-`, DF et DF2 pointeront tous les deux vers le même objet. Si on modifie DF2 (ou DF, ça marche dans les deux sens), ça modifiera aussi DF :


```r
DF <- as.data.table(iris)
DF2 <- DF
DF2[, Sepal.Length := 0]
head(DF, 3)
```

```
##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species
## 1:            0         3.5          1.4         0.2  setosa
## 2:            0         3.0          1.4         0.2  setosa
## 3:            0         3.2          1.3         0.2  setosa
```

Si on ne sait pas ça ça peut vraiment créer des problèmes sans qu'on s'en rende compte. 

Pour créer un "vrai"  nouvel objet en mémoire, on peut utiliser la fonction `copy()` :


```r
DF <- as.data.table(iris)
DF2 <- copy(DF)
DF2[, Sepal.Length := 0]
head(DF2, 3)
```

```
##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species
## 1:            0         3.5          1.4         0.2  setosa
## 2:            0         3.0          1.4         0.2  setosa
## 3:            0         3.2          1.3         0.2  setosa
```

```r
head(DF, 3)
```

```
##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species
## 1:          5.1         3.5          1.4         0.2  setosa
## 2:          4.9         3.0          1.4         0.2  setosa
## 3:          4.7         3.2          1.3         0.2  setosa
```

DF n'a pas été modifié. 

C'est simple mais il faut le savoir.

### Lecture/écriture de fichiers

`data.table` fournit les fonctions **`fread`** et **`fwrite`** :


```r
fwrite(iris, file="irisblank.csv")
datatest <- fread("irisblank.csv")
```

La fonction *`fread`* vient avec les options habituelles (`sep`, `nrows`, ...) mais très souvent trouve toute seule la bonne combinaison. C'est rare que j'ai besoin de les spécifier... Très pratique !

La magie en action :


```r
data(iris)
fwrite(iris, file="irisblank.csv", sep=" ")
fwrite(iris, file="iriscomma.csv", sep=",")
irisblank <- fread("irisblank.csv")
iriscomma <- fread("iriscomma.csv")
identical(irisblank, iriscomma)
```

```
## [1] TRUE
```

Deux options utiles dont je me sert pour des très gros jeux de données sont les options `select` et `drop` 
pour présélectionner des colonnes au moment de la lecture du fichier, ce qui rend le fichier plus léger dès la lecture. 

Les fonctions `fread()` et `fwrite()` sont très rapides, notamment avec de gros jeux de données là où R sera lent, voire ne marchera pas du tout (out of memory), data.table est la (ma) solution !

### Sytaxe (sélection de lignes et colonnes)

Le classique `object[rows, columns]` s'applique toujours, mais en moins verbeux, en évitant le `object$` dans les sélections, ainsi que les guillemets :


```r
nrow(irisblank[Sepal.Length>6, ]) # Virgule optionnelle ici
```

```
## [1] 61
```

```r
head(irisblank[, .(Sepal.Length, Sepal.Width)], 3) # Mais pas ici !
```

```
##    Sepal.Length Sepal.Width
## 1:          5.1         3.5
## 2:          4.9         3.0
## 3:          4.7         3.2
```

J'ai trouvé la syntaxe `.()` de la sélection de variables un peu dure à intégrer dans mes réflexes au départ mais on s'y fait vite.
Mais c'est largement compensé par la suppression des `object$` ou des guillemets. 

La suppression des guillemets permet notamment l'auto complétion, encore un bon point !

### Syntaxe : création de variables

Pour créer une nouvelle variable dans un objet `data.table`, on utilise les symboles `:=` avec la syntaxe suivante :


```r
irisblank[, newvar := 2*Sepal.Length]
```

Et voilà ! La syntaxe est aussi moins verbeuse que du R base ...

A noter que pour des syntaxes très complexes dans des créations de variables, au début j'ai parfois du mal à écrire en data.table ce que je voulais... Heureusement la syntaxe R basepour les data.frame s'applique sans problème, ça a un côté bien rassurant ! 

Et voilà, déjà avec ces quelques éléments on peut démarrer avec `data.table` tranquillement. 

## Utilisation spécifique à data.table

en plus du gros avantage de la rapidité, `data.table` vient avec quelques nouveautés par rapport à data.frame qui permettent une manipulation de données très aisée :

### Syntaxe : by

En plus de la syntaxe `object[rows, columns]`, data.table a une troisième dimension : `object[rows, columns, by]` pour faire des calculs groupés :


```r
irisblank[, MeanSepLength_species := mean(Sepal.Length), by= .(Species)]
head(irisblank,3)
```

```
##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species newvar
## 1:          5.1         3.5          1.4         0.2  setosa   10.2
## 2:          4.9         3.0          1.4         0.2  setosa    9.8
## 3:          4.7         3.2          1.3         0.2  setosa    9.4
##    MeanSepLength_species
## 1:                 5.006
## 2:                 5.006
## 3:                 5.006
```

Avec la syntaxe `:=` on a rajouté la variable `MeanSepLength_species`, moyenne de `Sepal.Length` par valeur de `Species`.

Le `.()` dans le by est optionnel si on n'a qu'une variable mais j'ai préféré prendre l'habitude de l'écrire pour garder une constance.

On peut noter aussi que la syntaxe suivante marche :

```r
bycol <- "Species"
irisblank[, MeanSepLength_species := mean(Sepal.Length), by=bycol]
```

Ce qui permet d'envoyer une variable (character vector) dans le by. 

On aurait pu aussi ne pas rajouter la variable dans l'objet lui même mais créer un vecteur sans redondance avec la fonction de sélection `.()` directement au moment du calcul :


```r
irisblank[, .(MeanSepLength_species = mean(Sepal.Length)), by = .(Species)]
```

```
##       Species MeanSepLength_species
## 1:     setosa                 5.006
## 2: versicolor                 5.936
## 3:  virginica                 6.588
```

La subtilité ici est sur le `=`, on n'utilise le `:=` que pour créer une (ou plusieurs) nouvelle variable dans l'objet. 

### Création de plusieurs variables à la fois

Pour créer par exemple un jeu de données des moyennes des variable par `Species`, on peut procéder de la façon suivante :


```r
stat_seplength <- irisblank[, .(MeanSepLength_species = mean(Sepal.Length),
                                SdSepLength_species = sd(Sepal.Length)), by=.(Espece = Species)]
```

A noter le renommage de la variable `Species` au même moment, très pratique.

On aurait pu créer ces variables directement dans le jeu de données. Pour ça on remplace la fonction ` .() `  par la fonction `` `:=`()` `:


```r
irisblank[, `:=`(MeanSepLength_species = mean(Sepal.Length),
                 SdSepLength_species = sd(Sepal.Length)), by=.(Species)]
head(irisblank,3)
```

```
##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species newvar
## 1:          5.1         3.5          1.4         0.2  setosa   10.2
## 2:          4.9         3.0          1.4         0.2  setosa    9.8
## 3:          4.7         3.2          1.3         0.2  setosa    9.4
##    MeanSepLength_species SdSepLength_species
## 1:                 5.006           0.3524897
## 2:                 5.006           0.3524897
## 3:                 5.006           0.3524897
```

### Autre "tips"

#### Enchainement d'opérations :

Il est possible d'enchainer les opérations avec une succession de crochets : 


```r
irisblank[, MeanSepLength_species := mean(Sepal.Length), by = .(Species)][, .(MyDoubleMean = 2*MeanSepLength_species)]
```

```
##      MyDoubleMean
##   1:       10.012
##   2:       10.012
##   3:       10.012
##   4:       10.012
##   5:       10.012
##  ---             
## 146:       13.176
## 147:       13.176
## 148:       13.176
## 149:       13.176
## 150:       13.176
```

#### Variable .N

`data.table` vient avec la variable `.N` qui contient le nombre d'observations. Voyons la en action :


```r
irisblank[, .(nbobs_species = .N), by = .(Species)]
```

```
##       Species nbobs_species
## 1:     setosa            50
## 2: versicolor            50
## 3:  virginica            50
```

Pratique pour l'intégrer dans des calculs :


```r
irisblank[, numobs_species := 1:.N, by=.(Species)]
head(irisblank[,.(Sepal.Length, Sepal.Width, Petal.Length, Petal.Width, Species, numobs_species)])
```

```
##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species numobs_species
## 1:          5.1         3.5          1.4         0.2  setosa              1
## 2:          4.9         3.0          1.4         0.2  setosa              2
## 3:          4.7         3.2          1.3         0.2  setosa              3
## 4:          4.6         3.1          1.5         0.2  setosa              4
## 5:          5.0         3.6          1.4         0.2  setosa              5
## 6:          5.4         3.9          1.7         0.4  setosa              6
```

#### Et bien d'autres !

Les fonctions bien nommées `setnames()`, `setorder()`, `setcolorder()`...

La suppression de variable :

```r
irisblank[, numobs_species := NULL]
```


`.SD` est une variable qui retourne le nom des colonnes.

```r
irisblank[, lapply(.SD, sum), by = "Species"]
```

```
##       Species Sepal.Length Sepal.Width Petal.Length Petal.Width newvar
## 1:     setosa        250.3       171.4         73.1        12.3  500.6
## 2: versicolor        296.8       138.5        213.0        66.3  593.6
## 3:  virginica        329.4       148.7        277.6       101.3  658.8
##    MeanSepLength_species SdSepLength_species
## 1:                 250.3            17.62448
## 2:                 296.8            25.80856
## 3:                 329.4            31.79398
```

```r
irisblank[, lapply(.SD, sum), by = "Species", .SDcols = -"newvar"]
```

```
##       Species Sepal.Length Sepal.Width Petal.Length Petal.Width
## 1:     setosa        250.3       171.4         73.1        12.3
## 2: versicolor        296.8       138.5        213.0        66.3
## 3:  virginica        329.4       148.7        277.6       101.3
##    MeanSepLength_species SdSepLength_species
## 1:                 250.3            17.62448
## 2:                 296.8            25.80856
## 3:                 329.4            31.79398
```


## Pour aller plus loin

https://github.com/Rdatatable/data.table

The cheat sheet : https://raw.githubusercontent.com/rstudio/cheatsheets/master/datatable.pdf
  

```r
file.remove("irisblank.csv")
```

```
## [1] TRUE
```

```r
file.remove("iriscomma.csv")
```

```
## [1] TRUE
```

