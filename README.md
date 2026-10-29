# Carnet de recettes

Le carnet est en ligne : **https://yalentin.github.io/recettes/**
Il se met à jour tout seul à chaque modification du dépôt, en une à deux minutes.

## Consulter

Ouvrir le lien, sur n'importe quel appareil. Pour partager le carnet, il suffit
d'envoyer ce lien : les visiteurs voient tout, en lecture seule, y compris tes
étoiles.

- **Planifier mes courses** : choisis des envies et un nombre de recettes et de
  personnes. Le carnet tire des recettes au hasard et dresse la liste de courses,
  regroupée par rayon.
- **Apparence** : Liquid Glass (par défaut), Vintage ou Éditorial ; mode Auto,
  Clair ou Sombre ; images en vignettes ou en mode compact. Ces réglages sont
  mémorisés par navigateur.

## Noter et ajouter des photos

Une recette peut recevoir de 1 à 5 étoiles et un cœur. Les deux ne disent pas la
même chose : cinq étoiles veulent dire « c'était très bon », le cœur veut dire
« celle-là, je la refais sans hésiter ». La facette **Mes notes** (en bas des
filtres) et le tri **Mieux notées** permettent de les retrouver.

**Pour pouvoir noter, il faut connecter le navigateur une fois** : bouton
**Mes notes**, puis coller la clé GitHub (`github_pat_…`) et **Connecter**. À faire
une fois par navigateur (ordinateur, iPhone…).

Ensuite, plus rien à faire. Les étoiles, les cœurs et les photos s'enregistrent
sur GitHub une quinzaine de secondes après le dernier clic, ou dès que tu quittes
l'onglet. Le bouton affiche « Mes notes (enregistrement…) » pendant ce temps.
S'il affiche « ⚠ non enregistré », ouvre-le : il dit pourquoi. Rien n'est perdu,
la modification attend dans le navigateur et repart à la visite suivante.

**Photos** : sur la fiche d'une recette, en bas, **Image…** permet de choisir une
photo (ordinateur ou iPhone), de coller une adresse web, de retirer l'image ou de
rétablir celle d'origine. La photo est réduite à 1400 px avant l'envoi. Sur
l'ordinateur, une photo d'iPhone au format HEIC ne passe pas : l'iPhone doit être
réglé sur « Le plus compatible », ou la photo convertie en JPEG.

### La clé GitHub

- C'est une clé « fine-grained » limitée au seul dépôt `yalentin/recettes`, avec
  le seul droit **Contents : Read and write**. Elle reste dans le navigateur et
  n'est envoyée qu'à GitHub.
- **Elle expire** à la date choisie à sa création. Le carnet le signale alors
  (« GitHub refuse la clé ») : il faut en créer une nouvelle sur
  github.com/settings/personal-access-tokens, puis la coller dans **Mes notes**.
- Si un appareil est perdu : supprimer la clé sur la même page GitHub. Elle ne
  donne accès qu'à ce dépôt, et l'historique permet de tout restaurer.
- **Mes notes › Déconnecter ce navigateur** retire la clé de ce navigateur.

## Ajouter des recettes (ingestion)

1. Dépose tes sources en vrac dans le dossier `dépôt/` du carnet, sur
   l'ordinateur : captures, photos, textes, liens. Ce dossier ne part jamais sur
   GitHub.
2. Dans ce dossier, lance Claude Code et dis « traite le dépôt ».
3. Claude formalise les recettes, pose les tags, cherche une image, écrit le
   journal et te montre les **ALERTES**. Les recettes partent ensuite directement
   en ligne.
4. Les sources traitées sont rangées dans `dépôt/_traité/`, les douteuses dans
   `dépôt/_à-revoir/`, toujours sur l'ordinateur.

Le détail est dans le skill : `.claude/skills/recette-ingestion/SKILL.md`.

**Étapes reconstruites.** Quand une source donne les ingrédients mais pas la
marche à suivre, et que le procédé est standard (soupe, vinaigrette, gratin…),
le skill écrit lui-même les étapes. Jamais les quantités, les ingrédients ni le
titre. La recette porte alors « ⚠️ Étapes reconstruites par l'agent » en tête de
ses notes, et une ALERTE au journal : relis-les.

## Modifier le site

Depuis une session Claude Code, sur l'ordinateur ou dans le cloud (y compris
depuis l'onglet Code de l'app iPhone), sur le dépôt `yalentin/recettes`. Les
règles du projet sont dans `CLAUDE.md`. Pour revenir en arrière, tout est dans
l'historique Git.

## Les fichiers

| Fichier | Contenu | Écrit par |
|---|---|---|
| `index.html` | Le moteur et le style | Claude, sur demande |
| `recettes-data.js` | Les recettes et le vocabulaire de tags | L'ingestion |
| `recettes-ingredients.js` | Les ingrédients : orthographe, alias, rayon | L'ingestion |
| `recettes-tags.md` | Tags approuvés et proposés (toi seul approuves) | L'ingestion |
| `recettes-notes.json` | Étoiles, cœurs, remplacements d'image | La page |
| `images/` | Photos locales | L'ingestion et la page |
| `journal/` | Un compte rendu par ingestion, ALERTES en tête | L'ingestion |

Sur l'ordinateur seulement, ignorés par Git : `dépôt/` (sources et archives) et
`.sauvegardes/` (anciennes sauvegardes d'avant Git, figées).

## Hors ligne

Deux dépendances à internet, aucune bloquante : les polices viennent de Google
Fonts (une police système prend le relais), et la plupart des photos sont des
illustrations hébergées sur d'autres sites. Si l'un d'eux disparaît, la vignette
affiche le titre de la recette à la place. Le texte des recettes ne dépend de
rien.
