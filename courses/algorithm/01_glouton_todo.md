Les algorithmes gloutons
---

[[toc]]

# Introduction

Il existe plusieurs types d'algorithmes qui permettent de résoudre des
problèmes. Ces algorithmes suivent alors une complexité et une **optimisation**
différente.

L'**optimisation** cherche à modéliser, à analyser et à résoudre les problèmes
qui consistent à **minimiser ou maximiser** une fonction sur un ensemble. Un
problème d'*optimisation* est un problème pour lequel on cherche la meilleure
solution (selon un critère défini) dans un ensemble de solutions possibles,
qu'on appelle la **solution optimale**.

# Les algorithmes gloutons

Les algorithmes gloutons sont souvent utilisés pour résoudre ces problèmes
d'optimisation. On cherche une solution optimale en effectuant **le meilleur
choix possible à chaque étape** de l'algorithme.

Dans ce type de résolution, il n'y a pas de retour en arrière. Lorsqu'un choix
est fait, il n'est pas modifié par la suite. On se retrouve donc à chaque
étape, avec un problème de plus en plus petit à résoudre.

Un algorithme glouton suit les principes suivants :

- À chaque étape, il choisit la **meilleure solution immédiate** ;
- Il ne revient **jamais** en arrière pour modifier un choix précédent.

On découpe le problème par étapes, en faisant, à chaque étape, le meilleur
choix possible. De plus, on ne remet jamais en cause les choix faits aux étapes
précédentes.

:warning: Attention, cette méthode ne fournit pas systématiquement la solution
optimale au problème proposé.

# Cas d'usage : rendu de monnaie

## Présentation du cas d'usage

Dans une boulangerie, la personne à la caisse souhaite rendre une somme avec
le **moins de pièces** (ou de billets) possible. On a alors un problème avec
une **optimisation** à effectuer.

Prenons alors le système de monnaie européen sans compter les pièces des
centimes (exprimé en euros €) : 1, 2, 5, 10, 20, 50, 100, 200, 500.

Supposons qu'on souhaite rendre à un client 49 €. On pourra alors lui rendre
les combinaisons suivantes :

- 49 € = 4 * 10 € + 1 * 5 € + 2 * 2 €, soit 7 pièces/billets au total ;
- 49 € = 9 * 5 € + 2 * 2 $, soit 11 pièces/billets au total ;
- 49 € = 9 * 5 € + 4 * 1 €, soit 13 pièces/billets au total ;
- 49 € = 49 * 1 €, soit 49 pièces/billets au total.

## Algorithme en pseudo-code

Dans ce problème, on va pouvoir construire l'algorithme comme suit :

- Trouver le billet ou la pièce qui a la valeur la plus proche à la somme qu'on
  souhaite rendre, qui est **inférieure** à la somme à rendre ;
- Soustraire la valeur du billet/pièce à la somme à rendre ;
- On recommence le processus jusqu'à ce que la monnaie à rendre est égale à 0.

Ces différentes étapes, nous pouvons les traduire avec le pseudo-code suivant :

```txt
Paramètres :
    reste : entier - la somme à rendre
    systeme : liste d'entiers - le système de monnaies par ordre décroissant

Code :
    i = 0

    tant que reste > 0:
        # Trouver le billet le proche (inférieur) du reste à rendre (reste)
        # Récupérer la première valeur la plus petite au reste dans systeme
        tant que systeme[i] > reste:
            i = i + 1

        reste = reste - systeme[i]
        afficher systeme[i]
```

Dans notre exemple où l'on doit rendre 49 €, on a les étapes suivantes :

| Étapes  | Choix de la monnaie | Reste à rendre |
| :------ | :-----------------: | -------------: |
| Étape 1 | 20                  | 49 - 20 = 29   |
| Étape 2 | 20                  | 29 - 20 = 9    |
| Étape 3 | 5                   | 9 - 5 = 4      |
| Étape 4 | 2                   | 4 - 2 = 2      |
| Étape 5 | 2                   | 2 - 2 = 2      |

On a ainsi
49 € = 20 € + 20 € + 5 € + 2 € + 2 € = 2 * 20 € + 1 * 5 € + 2 * 2 €.

## Implémentation en *Python*

Dans ce cas, nous allons utiliser des notions de **listes** et de **parcours de
listes**.

Prenons le système de monnaies dans la liste suivante :

```py
systeme = [500, 200, 100, 50, 20, 10, 5, 2, 1]
```

Construisons une fonction
`rendu_monnaie(somme: entier, systeme: liste d'entiers)` qui prend en paramètre
la somme à rendre `somme` et la liste d'entier correspondant au système de
monnaies triée par ordre décroissant. Cette dernière affichera la monnaie à
rendre.

```py
def rendu_monnaie(somme, systeme):
    i = 0

    while somme > 0:
        while systeme[i] > somme:
            i = i + 1

        somme = somme - systeme[i]
        print(systeme[i])
```

En exécutant notre fonction avec les paramètres suivants, on retombe sur notre
résultat précédent.

```py
>>> systeme = [500, 200, 100, 50, 20, 10, 5, 2, 1, 0.5, 0.2, 0.1, 0.05, 0.02, 0.01]
>>> rendu_monnaie(49, systeme)
20
20
5
2
2
```

N'hésite pas à tester la fonction avec des paramètres différents pour voir
d'autres exemples !

# Cas d'usage : problème du sac à dos

Pour la prochaine séance !

:calendar: Mardi 14 avril, 20h
