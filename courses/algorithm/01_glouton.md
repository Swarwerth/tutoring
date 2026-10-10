Les algorithmes gloutons
---

[toc]

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

⚠️ Attention, cette méthode ne fournit pas systématiquement la solution
optimale au problème proposé.

# Cas d'usage : rendu de monnaie

## Présentation du cas d'usage

Dans une boulangerie, la personne à la caisse souhaite rendre une somme avec
le **moins de pièces** (ou de billets) possible. On a alors un problème avec
une **optimisation** à effectuer.

Prenons alors le système de monnaie européen sans compter les pièces des
centimes (exprimé en euros €) : 1, 2, 5, 10, 20, 50, 100, 200, 500.

Supposons qu'un client prend des viennoiseries pour 49 €. On pourra alors lui
rendre les combinaisons suivantes :

- 49 € = 4 * 10 € + 1 * 5 € + 2 * 2 €, soit 7 pièces au total ;
- 49 € = 9 * 5 € + 2 * 2 $, soit 11 pièces au total ;
- 49 € = 9 * 5 € + 4 * 1 €, soit 13 pièces au total ;
- 49 € = 49 * 1 €, soit 49 pièces au total.

## Algorithme en pseudo-code

Dans ce problème, on va pouvoir construire l'algorithme comme suit :

- On sélectionne le billet ou la pièce avec la plus grande valeur possible
  (qui reste inférieur à la somme à rendre) ;
- On soustrait cette valeur à la monnaie à rendre ;
- On recommence ainsi jusqu'à ce que la monnaie à rendre est nulle.

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
monnaies triée par ordre décroissant. Cette dernière renverra la liste de
monnaies à rendre.

```py
def rendu_monnaie(somme: int, systeme: list[int]) -> list[int]:
    monnaies_rendues = []
    somme_restante = somme
    i = 0

    # Tant qu'il nous reste de l'argent à rendre
    while somme_restante > 0:
        # Chercher la valeur la plus grande, qui reste inférieur
        # à la somme à rendre
        while systeme[i] > somme_restante:
            i = i + 1

        # Retirer la valeur prise et l'ajouter à la liste
        somme_restante = somme_restante - systeme[i]
        monnaies_rendues.append(systeme[i])

    return monnaies_rendues
```

En exécutant notre fonction avec les paramètres suivants, on retombe sur notre
résultat précédent.

```py
>>> rendu_monnaie(49, systeme)
[20, 20, 5, 2, 2]
```

# Cas d'usage : problème du sac à dos

## Présentation du problème

Dans ce cas, on dispose d'objets, chacun ayant une **valeur** et un **poids**.
Le sac a une **capacité maximale** et l'objectif est de choisir les objets pour
maximiser la valeur, sans dépasser le poids.

On prend alors l'exemple suivant :

| N° article | Objet    | Valeur | Poids  |
| ---------: | :------- | -----: | -----: |
| 1          | Jumelles | 5      | 0.5 kg |
| 2          | Tente    | 3      | 4 kg   |
| 3          | Gourde   | 2      | 1 kg   |
| 4          | Carte    | 1      | 0.2 kg |

Avec un sac limité à 4.7 kg, on peut alors prend les éléments suivants :

- Jumelles, Tente, Carte - 5 + 3 + 1 = 9 - 0.5 + 4 + 0.2 = 4.7 kg ;
- Jumelles, Gourde, Carte - 5 + 2 + 1 = 8 - 0.5 + 1 + 0.2 = 1.7 kg ;
- Tente, Carte - 3 + 1 = 4 - 4 + 0.2 = 4.02 kg.

## Algorithme en pseudo-code

En considérant que la liste est triée par ordre décroissant par valeur, on peut
alors suivre l'algorithme suivant :

- On sélectionne l'objet avec la plus grand valeur possible (qui reste avec un
  poids inférieur au poids maximal du sac) ;
- On soustrait le poids de l'objet au poids maximal du sac ;
- On recommence ainsi jusqu'à ce qu'aucun objet ne peut rentrer dans le sac.

## Implémentation en *Python*

Dans ce cas, nous allons utiliser des notions de **listes** et de **parcours de
listes** et de **tuples**.

Prenons les objets suivants dans la liste suivante, représentés par un tuple
`(valeur, poids)`:

```py
objets = [(5, 0.5), (3, 4), (2, 1), (1, 0.2)]
```

Construisons une fonction
`remplir_sac(poids_max: float, objets: liste d'objets)` qui prend en paramètre
le poids maximal du sac `poids_max` et la liste d'objets associés avec leur
valeur et du poids, triée par ordre décroissant de valeur. Cette dernière
renverra la liste d'objets à prendre avec le numéro des objets.

```py
def remplir_sac(poids_max: float, objets: list[tuple[int, float]]) -> list[int]:
    objets_a_prendre = []
    poids_actuel = 0
    i = 0

    for i in range(len(objets)):
        valeur, poids = objets[i]

        if poids_actuel + poids <= poids_max:
            poids_actuel = poids_actuel + poids
            objets_a_prendre.append(i)

    return objets_a_prendre
```

En exécutant notre fonction avec les paramètres suivants, on retombe sur notre
résultat précédent.

```py
>>> remplir_sac(4.7, objets)
[0, 1, 3]
```

## Améliorations possibles

Il est alors possible d'améliorer le programme en prenant en compte les points
suivants :

- Afficher les objets pris en utilisant en dictionnaire ;
- Calculer le ratio valeur / poids pour maximiser la valeur et minimiser le
  poids du sac.
