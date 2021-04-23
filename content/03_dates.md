---
jupytext:
  cell_metadata_filter: -all
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.12
    jupytext_version: 1.9.1
kernelspec:
  display_name: IMongo
  language: ''
  name: imongo
---

# Attributs de type date

* Auteurs/trices : HAMELIN Marine, ABOULKACEM Zakaria, RALIN Kévin, GARY Gaston

Ce chapitre traite des attributs de type dates (et sous-cas des listes de dates) et des différents types de requêtes que l'on peut vouloir faire sur de tels attributs
Le fichier que vous devez modifier pour ce chapitre est `mongo_book/content/03_dates.md`.*

Les dates sur MongoDB sont définies a la milliseconde près, nous allons nous intéresser aux requêtes utilisant des dates, car les attributs de type date doivent être considérés de façon particulière.
les dates sont enregistrées sous la forme ISO date, nous retrouvons la date de la forme suivante : 
("<YYYY-mm-ddTHH:MM:ssZ>").

Ce format nous permet de d’avoir une version de référence (UTC) de la date spécifiée. 
Nous pouvons mettre uniquement l’année ou l’année et le mois sans devoir spécifier tout le langage s’occupe de remplir les autres arguments pour retrouver la forme :
("<YYYY-mm-ddTHH:MM:ssZ>").

 Exemple :
new Date ('2021'), ce qui donne un objet de type date de la forme suivante 
ISODate("2021-01-01T00:00:00Z")

Ainsi il faut toujours utiliser une comparaison sous forme d’intervalle pour ces attributs.
lorsque l’on souhaite effectuer un test sur une date, on utilisera des opérateurs de comparaison, qu’on a pu voir plus haut dans le cours.  
Afin de vérifier les deux conditions de borne dans un intervalle de comparaison nous avons besoin de l’opérateur ’"elemmatch", ce qui va nous permettre de comparer pour chaque élément de type date s’il existe bien dans l’intervalle qui nous intéresse.
Ainsi, si nous souhaitions regrouper des documents par date, il faudrait faire recours à une syntaxe particulière pour préciser à quel degré de précision sur la date l’agrégation doit se faire.
Exemple :
Ici nous souhaitions regrouper ensemble tous les documents concernant un mois donné, puis en faire une somme sur le prix par mois.
db.coll.aggregate( [
   {
     $group: {
        _id: {
               month: { $month: "$att_date" },
               day: { $dayOfMonth: "$att_date" },
               year: { $year: "$att_date"}
           },
        total: { $sum: "$price" }
     }
   }
] )




## Exemples d'applications
Utilisation d'un objet date au format string.

Exemple d'une requête simple. On veut récupérer la liste des individus dont l'attribut date est supérieur à une date créée.
```javascript
madate = "<YYYY-mm-dd>"
db.coll.find({"varDateString" : {$gt : madate}})
```
Utilisation d'un objet date au format Date.

Exemple d'une requête simple dans la db `food`. On veut récupérer la liste des restaurants dont la date de la note est supérieure à une date créée.
```javascript
madate = new Date("<YYYY-mm-dd>")
db.NYfood.find({"grades.date": {$gt : madate}})
```
