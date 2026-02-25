# Quiz Départements

Jeu de quiz pour deviner les 101 départements français à partir de leur numéro.

## Jouer

Ouvrir `index.html` dans un navigateur. Aucune installation requise.

## Modes de jeu

**Partie complète** — Les 101 départements (métropole + DOM) dans un ordre aléatoire. 3 tentatives par département. Score final avec la liste des départements ratés.

**Un département** — Un numéro aléatoire, une seule question. Idéal pour s'entraîner rapidement.

## Fonctionnalités

- **Comparaison tolérante** : accents, tirets, apostrophes et majuscules sont ignorés. Les petites fautes de frappe sont acceptées grâce à la distance de Levenshtein (seuil de 2 caractères).
- **Animations** : confettis sur bonne réponse, shake rouge sur erreur, fade entre les questions.
- **Barre de progression** : barre fixée en bas de l'écran, pleine largeur, visible pendant toute la partie complète.
- **Écran de résultats** : score final et liste des départements manqués pour réviser.
- **Carte de France** : carte SVG interactive qui colore les départements au fur et à mesure des bonnes réponses. Les DOM sont représentés par des badges sous la carte. Activable dans les réglages.
- **Liste des départements trouvés** : liste triée alphabétiquement des départements devinés avec compteur de progression. Activable dans les réglages.
- **Réglages** : accessible via l'icône engrenage en haut à droite. Permet d'activer/désactiver la carte et la liste. Les préférences sont sauvegardées dans le navigateur (localStorage).
- **Abandonner** : possibilité de quitter une partie complète en cours pour revenir à l'accueil.

## Données

101 départements : 96 métropole (dont 2A Corse-du-Sud et 2B Haute-Corse) + 5 DOM (971 Guadeloupe, 972 Martinique, 973 Guyane, 974 La Réunion, 976 Mayotte).

## Tech

Un seul fichier `index.html`, zéro dépendance. HTML + CSS + JS vanilla. Polices Google Fonts (Cormorant Garamond + Outfit). Carte SVG issue de `@svg-maps/france.departments` (CC-BY 4.0).
