# Quiz Départements Français — Design

## Objectif

Jeu de quiz pour deviner les 101 départements français (métropole + DOM) à partir de leur numéro.

## Tech

Un seul fichier `index.html`, zéro dépendance, HTML/CSS/JS vanilla.

## Écran d'accueil

Deux modes de jeu :

- **Partie complète** : les 101 départements, mélangés, un par un. 3 tentatives par département. Score final + récapitulatif des ratés.
- **Un département** : un numéro aléatoire, une seule tentative, résultat immédiat. Bouton "Encore un" + "Retour".

## Layout (mode partie complète)

Page centrée, minimaliste (fond blanc, typo sans-serif).

- Numéro du département affiché en grand (élément dominant)
- Input autofocus en dessous, validation par Entrée
- Compteur de tentatives (1/3, 2/3, 3/3)
- Barre de progression fine en bas + score (ex: 12/101)

## Logique de jeu

### Partie complète

1. Les 101 départements sont mélangés aléatoirement au lancement
2. Un numéro s'affiche, le joueur tape le nom et valide avec Entrée
3. Bonne réponse → vert + confettis, passage au suivant (~1.5s)
4. Mauvaise réponse → shake rouge, input vidé, tentative suivante
5. 3ème échec → bonne réponse affichée en rouge (~2s), puis suivant
6. Fin → score final (ex: 78/101) + liste des départements ratés + bouton Recommencer

### Un département

1. Un numéro aléatoire s'affiche
2. Le joueur tape le nom, valide avec Entrée
3. Bonne réponse → vert + confettis
4. Mauvaise réponse → bonne réponse affichée en rouge
5. Boutons "Encore un" et "Retour"

## Comparaison tolérante

Deux étapes :

1. **Normalisation** : minuscules, suppression accents (NFD + remove diacritics), suppression tirets/apostrophes, trim espaces multiples
2. **Levenshtein** : distance ≤ 2 entre saisie normalisée et réponse normalisée

Exemples :
- `cote d armor` → `cotedarmor` vs `cotesdarmor` → distance 1 → ✅
- `cottes d'armor` → `cottesdarmor` vs `cotesdarmor` → distance 1 → ✅
- `lot et garone` → `lotetgarone` vs `lotetgaronne` → distance 1 → ✅

## Animations (CSS pur)

- **Bonne réponse** : input/numéro en vert, confettis (~20 divs avec keyframes randomisés : position, couleur, rotation, durée), disparaissent après ~1.5s
- **Mauvaise réponse** : shake horizontal de l'input (~0.4s), bordure rouge ~1s
- **Réponse révélée** : texte en rouge sous le numéro, affiché ~2s
- **Transitions** : fade doux entre départements

## Données

Objet JS avec 101 entrées :

- 96 départements métropole (01 à 95, avec 2A et 2B pour la Corse, sans 20)
- 5 DOM : 971 (Guadeloupe), 972 (Martinique), 973 (Guyane), 974 (La Réunion), 976 (Mayotte)

## Style

- Fond blanc, typo système sans-serif
- Minimaliste et épuré
- Éléments centrés verticalement et horizontalement
- Barre de progression discrète
