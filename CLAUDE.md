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
- **Commentaires et ajustements depuis un chat** : Val discute du carnet dans le
  projet claude.ai « Recettes ». Un chat de ce projet qui doit toucher au carnet
  attache le dépôt `yalentin/recettes` (accès en écriture), le clone, lit ce
  fichier, puis travaille comme n'importe quelle session : modification, tests,
  commit, `git push` sur `main`. Avant de modifier : `git pull --rebase`, parce
  que la page et l'ingestion écrivent aussi dans le dépôt.
  - Un retour de Val sur le **fonctionnement** (règle d'ingestion, format,
    tags…) se traduit par une modification du skill ou de ce fichier, pas
    seulement par une réponse dans le chat : sinon la prochaine session l'ignore.
  - L'**ingestion** reste sur l'ordinateur (les sources y sont). Depuis un chat,
    seulement si Val le demande et si le chat est relié à son ordinateur ; les
    sources ne transitent alors que pour être lues, jamais vers le dépôt.
  - Ne jamais committer `recettes-notes.json` depuis une copie de travail : il
    appartient à la page. Si un test l'a modifié, `git checkout -- recettes-notes.json`.

## Décisions déjà prises (ne pas les défaire sans que Val le demande)

Historique détaillé et raisons de ces choix : `docs/historique/` (critique du
design du 20 septembre 2026, audit mobile du 3 octobre 2026). Ces textes décrivent
encore l'ancien fonctionnement local pour les notes et les images ; seules les
décisions de design résumées ci-dessous font foi.

- **Apparence** : quatre styles (Liquid Glass par défaut, Vintage, Éditorial,
  Marché ; `STYLE_DEFAUT` en tête de `index.html`). Aqua, Suisse, Brutal et Mono
  ont été supprimés le 3 octobre 2026. Marché (29 octobre 2026) est le style
  dessiné par Claude sans consigne de style : Fraunces + Schibsted Grotesk,
  cartes blanches à filets fins, photo sertie, étapes en frise ; quatre
  palettes (Tomate par défaut, Cerise, Basilic, Prune). Les polices du dernier
  style vu sont demandées dès le script de tête (`carnet_fonts_eff`). Mode Auto / Clair / Sombre, Auto par défaut.
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
  Un assaisonnement (`warn`) est signalé par la mention « au goût » en texte
  secondaire après la quantité, cliquable pour l'explication ; plus de « ! »
  cerclé de rouge, qui se lisait comme une erreur.
- **Étoiles et favoris** : deux jugements indépendants (5 étoiles = « très bon »,
  cœur = « je la refais sans hésiter »). L'écouteur délégué des étoiles est posé
  **en phase de capture**, sinon cliquer une étoile ouvre la recette.
- **Images depuis la page** : réduction à 1400 px, JPEG ~0,82, avant tout
  enregistrement ; nommage `images/<id>-<code base36>.jpg`. `recettes-data.js`
  n'est jamais modifié par la page : un changement d'image est un remplacement
  dans le fichier de notes (`img` absent = pas de remplacement, `img:""` = image
  retirée volontairement), ce qui permet toujours de rétablir l'originale.
- **Colonne latérale** (critique du 29 octobre 2026) : « Planifier mes courses »
  est l'action principale, en accent plein. « Mes notes » et « Apparence » sont
  des réglages discrets avec chevron, jamais en accent, même ouverts. Le bouton
  ET/OU d'une facette est neutre (c'est un mode, pas un filtre actif). La
  facette des étoiles s'affiche « Favoris et étoiles » (clé interne « Mes
  notes ») pour ne pas se confondre avec le bouton. Cuisine, Régime et Favoris
  et étoiles sont repliés au premier chargement. Largeur minimale 240 px ;
  dans le panneau Apparence, les boutons passent à la ligne, jamais tronqués.
  Repliable sur écran large (29 octobre 2026) : bouton neutre en haut à droite
  de la colonne, bouton de réouverture fixé en haut à gauche, état gardé dans
  `carnet_side_ferme` et posé dès le script de tête ; glisser la poignée sous
  140 px replie aussi. Fiche et planificateur sont centrés dans la zone
  principale (`margin-inline:auto`), pas calés à gauche.
- **Unités** : écrites au singulier dans les données, accordées par la page
  (`accorderUnite`, liste fermée de noms comptables).
- **iPhone** : sous 780 px, colonne latérale masquée en vue fiche et filtres
  repliés par défaut ; fiche organisée pour cuisiner (titre, photo, temps,
  ingrédients, étapes ; note et image après).
  Les −/+ des personnes font 44 px sur écran tactile ou étroit (règle en fin
  de feuille). En Glass, le « Retour » de la fiche vit dans une barre collante
  qui prend un fond de verre dès que la page défile (`data-defile`), pour ne
  plus flotter par-dessus les tags et le titre.

## Enregistrement depuis la page (depuis octobre 2026)

La page écrit elle-même `recettes-notes.json` et `images/` par l'API GitHub
(Contents API), avec une clé « fine-grained » limitée à ce dépôt, collée par Val
dans « Mes notes » et gardée dans le `localStorage` de son navigateur
(`carnet_gh_cle`). Sans clé : lecture seule (`html:not([data-proprio])`).

- Notes : lues par l'API pour Val (sans attendre Pages), par `fetch` de la copie
  publiée pour les visiteurs. Fusion par horodatage `t`, entrée par entrée ; une
  entrée remise à zéro est gardée (« pierre tombale ») pour ne pas ressusciter.
- Envoi regroupé 10 s après le dernier clic, ou au passage de l'onglet en
  arrière-plan ; tampon `carnet_notes` + drapeau `carnet_notes_attente` dans
  `localStorage` pour ne rien perdre ; conflit 409/422 → relire, refusionner,
  réessayer.
- Retour visuel : pastille d'état fixe en bas (`afficherEtat`), aperçu voilé de
  la photo à sa place pendant l'envoi (`PHOTO_EN_COURS`). Toute nouvelle écriture
  vers GitHub doit passer par ces deux mécanismes : Val a signalé qu'une
  opération sans retour visible donne l'impression que rien ne se passe.
- Commits faits avec l'auteur `GH_AUTEUR` (adresse noreply). Ne jamais envoyer la
  clé ailleurs qu'à `api.github.com`, ne jamais l'écrire dans le dépôt.
- Les valeurs `img` admises : `""`, `images/<nom>` ou une adresse http(s) sans
  guillemets ni chevrons (`normaliserNote`). Les champs de `recettes-data.js` sont
  du texte simple : ne pas y introduire de HTML, la page les insère tels quels et
  détient désormais une clé.

## Idées non tranchées

Écran qui reste allumé en cuisine, cocher ingrédients et étapes, minuteurs tirés
des étapes.
