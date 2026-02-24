# Quiz Departements

Jeu de quiz pour deviner les 101 departements francais a partir de leur numero.

## Jouer

Ouvrir `index.html` dans un navigateur. Aucune installation requise.

## Modes de jeu

**Partie complete** — Les 101 departements (metropole + DOM) dans un ordre aleatoire. 3 tentatives par departement. Score final avec la liste des departements rates.

**Un departement** — Un numero aleatoire, une seule tentative. Ideal pour s'entrainer rapidement.

## Fonctionnalites

- **Comparaison tolerante** : accents, tirets, apostrophes et majuscules sont ignores. Les petites fautes de frappe sont acceptees grace a la distance de Levenshtein (seuil de 2 caracteres).
- **Animations** : confettis sur bonne reponse, shake rouge sur erreur, fade entre les questions.
- **Barre de progression** : suivi discret de l'avancement en mode partie complete.
- **Ecran de resultats** : score final et liste des departements manques pour reviser.

## Donnees

101 departements : 96 metropole (dont 2A Corse-du-Sud et 2B Haute-Corse) + 5 DOM (971 Guadeloupe, 972 Martinique, 973 Guyane, 974 La Reunion, 976 Mayotte).

## Tech

Un seul fichier `index.html`, zero dependance. HTML + CSS + JS vanilla.
