# Carnet de recettes de Val

Carnet de recettes personnel, publié sur GitHub Pages depuis la branche `main`
de github.com/yalentin/recettes (dépôt **public**). Tout est en français :
code commenté, messages de commit, journaux, réponses à Val.

## Les fichiers et qui les écrit

| Fichier | Rôle | Qui l'écrit |
|---|---|---|
| `index.html` | Le moteur et le style (visionneuse, planificateur de courses) | Claude, **seulement** sur demande explicite de modifier le site |
| `recettes-data.js` | `window.FACETTES`, `FACETTES_MODE`, `RECETTES` | Le skill d'ingestion (ajout en fin de `RECETTES`) |
| `recettes-ingredients.js` | Registre des ingrédients (nom canonique, alias, rayon) | Le skill d'ingestion |
| `recettes-tags.md` | Tags approuvés / proposés | Le skill ; Val seul approuve |
| `recettes-notes.json` | Étoiles, favoris, remplacements d'image (une recette par ligne) | **La page**, via l'API GitHub |
| `images/` | Photos locales | Le skill (photo du plat fini) et la page |
| `journal/` | Un compte rendu par ingestion, ALERTES en tête | Le skill |
| `.claude/skills/recette-ingestion/` | Le skill d'ingestion | Claude, sur demande |

Deux propriétaires qui ne se recoupent pas : le skill écrit les recettes, la page
écrit les notes et les images. Ne pas mélanger ces rôles.

## Règles absolues

- **Ne jamais envoyer `dépôt/` ni `.sauvegardes/` sur GitHub.** Ces dossiers
  n'existent que sur l'ordinateur de Val (sources brutes : captures de
  messageries, photos de recettes manuscrites familiales, audio). Ils sont dans
  `.gitignore`. Ne jamais forcer leur ajout, ne jamais copier une source ailleurs
  dans le dépôt. Un fichier envoyé une fois reste lisible dans l'historique.
- Toujours ajouter les fichiers **nommément** (`git add fichier1 fichier2`),
  jamais `git add -A` ou `git add .`.
- Identité Git : `yalentin <329976600+yalentin@users.noreply.github.com>`.
  Ne jamais faire apparaître l'adresse mail réelle de Val.
- Pour l'ingestion de recettes, suivre le skill `recette-ingestion` à la lettre.
  Elle se fait **sur l'ordinateur de Val**, à la demande (pas de tâche planifiée).
  Les recettes ingérées partent directement en ligne sur `main` : c'est le choix
  de Val, l'historique Git sert de filet.

## Où se fait quoi

- **Ingestion** : Claude Code sur l'ordinateur de Val, dans sa copie du dépôt,
  parce que les sources y sont. `git pull --rebase` avant, commit + `git push` après.
- **Modifications du site** (design, fonctions) : depuis n'importe quelle session,
  y compris dans le cloud. Avant d'envoyer une modification de `index.html`,
  vérifier dans Chromium (Playwright) : aucune erreur JavaScript, rendu à 390 px
  (iPhone) et 1440 px, en clair et en sombre.

## Décisions déjà prises (ne pas les défaire sans que Val le demande)

Historique détaillé : anciens docs du projet claude.ai « Recettes »
(critique du design du 20 septembre 2026, audit mobile du 3 octobre 2026).

- **Apparence** : trois styles (Liquid Glass par défaut, Vintage, Éditorial ;
  `STYLE_DEFAUT` en tête de `index.html`). Aqua, Suisse, Brutal et Mono ont été
  supprimés le 3 octobre 2026. Mode Auto / Clair / Sombre, Auto par défaut.
  Accent rouge cerise `#ae292d` en clair ; en sombre, remplissage `#d1343b` et
  texte d'accent `#ff6b70` (valeurs choisies à la main dans la palette, appliquées
  à la fin de `toDark`).
- **Grille** : toutes les cartes ont la même anatomie. Une carte sans photo garde
  sa vignette 4/3 avec le titre en Zilla Slab italique (taille en `cqi`) ; ne
  jamais la réduire à un bandeau dans la liste (régression signalée par Val :
  les titres ne s'alignaient plus). Sur la fiche, pas de bandeau.
- **Bruit** : badge « illustration web » seulement sur la fiche, pas sur les
  cartes ; l'accent est réservé à l'action principale, aux filtres actifs, aux
  numéros d'étape et aux étoiles. Les étoiles d'une carte non notée
  n'apparaissent qu'au survol ou au focus.
- **Étoiles et favoris** : deux jugements indépendants (5 étoiles = « très bon »,
  cœur = « je la refais sans hésiter »). L'écouteur délégué des étoiles est posé
  **en phase de capture**, sinon cliquer une étoile ouvre la recette.
- **Images depuis la page** : réduction à 1400 px, JPEG ~0,82, avant tout
  enregistrement ; nommage `images/<id>-<code base36>.jpg`. `recettes-data.js`
  n'est jamais modifié par la page : un changement d'image est un remplacement
  dans le fichier de notes (`img` absent = pas de remplacement, `img:""` = image
  retirée volontairement), ce qui permet toujours de rétablir l'originale.
- **iPhone** : sous 780 px, colonne latérale masquée en vue fiche et filtres
  repliés par défaut ; fiche organisée pour cuisiner (titre, photo, temps,
  ingrédients, étapes ; note et image après).

## Enregistrement depuis la page (depuis octobre 2026)

La page écrit elle-même `recettes-notes.json` et `images/` par l'API GitHub
(Contents API), avec une clé « fine-grained » limitée à ce dépôt, collée par Val
dans « Mes notes » et gardée dans le `localStorage` de son navigateur
(`carnet_gh_cle`). Sans clé : lecture seule (`html:not([data-proprio])`).

- Notes : lues par l'API pour Val (sans attendre Pages), par `fetch` de la copie
  publiée pour les visiteurs. Fusion par horodatage `t`, entrée par entrée ; une
  entrée remise à zéro est gardée (« pierre tombale ») pour ne pas ressusciter.
- Envoi regroupé 15 s après le dernier clic, ou au passage de l'onglet en
  arrière-plan ; tampon `carnet_notes` + drapeau `carnet_notes_attente` dans
  `localStorage` pour ne rien perdre ; conflit 409/422 → relire, refusionner,
  réessayer.
- Commits faits avec l'auteur `GH_AUTEUR` (adresse noreply). Ne jamais envoyer la
  clé ailleurs qu'à `api.github.com`, ne jamais l'écrire dans le dépôt.
- Les valeurs `img` admises : `""`, `images/<nom>` ou une adresse http(s) sans
  guillemets ni chevrons (`normaliserNote`). Les champs de `recettes-data.js` sont
  du texte simple : ne pas y introduire de HTML, la page les insère tels quels et
  détient désormais une clé.

## Idées non tranchées

Écran qui reste allumé en cuisine, cocher ingrédients et étapes, minuteurs tirés
des étapes.
