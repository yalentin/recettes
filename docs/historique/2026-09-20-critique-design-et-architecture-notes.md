> **Archive.** Document rédigé dans le projet claude.ai « Recettes » le 20 septembre
> 2026, déplacé ici le 29 octobre 2026. Les décisions de design (§1 et §2) restent
> valables et sont résumées dans `CLAUDE.md`. En revanche, le stockage des notes
> (§3), la gestion des images (§4) et l'hébergement (§5) décrivent l'ancien
> fonctionnement local, remplacé en octobre 2026 par l'enregistrement direct sur
> GitHub depuis la page.

# Carnet de recettes — critique du design, notes personnelles, gestion des images

État au 20 septembre 2026. Chantier terminé : thème, réduction du bruit, notes personnelles, gestion des images depuis la page. Reste une seule vérification, qui ne peut être faite que par Val (section 6).

Sauvegardes préalables : `.sauvegardes/recettes.html.2026-09-18_134122.avant-notes-et-theme-verrouille` et `.sauvegardes/recettes.html.2026-09-20_*.avant-gestion-images`

## 1. Critique du design — diagnostic du « trop de bruit »

Mesures relevées sur le rendu réel avant correction : 119 cartes, 46 pastilles de filtre affichées en permanence, 95 badges « illustration web ».

Le défaut n'était pas la quantité d'information mais **l'égalité de poids** entre des informations de rang très différent. Cinq foyers, par ordre d'impact décroissant.

**Le badge « illustration web » sur 95 cartes sur 119.** Un marqueur présent sur 80 % des objets ne distingue plus rien : il ne coûte que de l'attention. Sa place utile est la fiche détaillée, au moment où l'on regarde vraiment la photo et où sa provenance change la lecture qu'on en fait.

**L'accent rouge dépensé partout.** Il portait simultanément le bouton principal, les bascules ET/OU, la ligne de tags de chaque carte, « tout effacer », le bouton retour, les tags de la fiche, les numéros d'étape et les boutons de portions. Une couleur qui signale huit choses ne signale plus rien.

**La ligne de tags au-dessus du titre.** En accent, graisse 700, elle se lisait avant le titre alors qu'elle ne fait que confirmer ce que dit déjà l'intertitre de section sous lequel la carte est rangée. Redondance et inversion de hiérarchie dans le même objet.

**La vignette absente.** Elle affichait le titre de la recette dans un cadre 4/3, puis le titre réapparaissait immédiatement sous elle : un titre écrit deux fois, et plus de 500 px de vide sur la fiche. Ce cas se déclenche aussi quand un blog source disparaît — donc de plus en plus souvent avec le temps.

**Quatre vocabulaires de filets simultanés :** cadre plein de carte, tirets de vignette, filet double d'intertitre, pointillés de liste. Le style Vintage en assume deux ; à quatre, l'œil ne les hiérarchise plus.

Deux points secondaires : les 46 pastilles de filtre portaient chacune un cadre et une ombre, ce qui en faisait 46 objets de même poids ; et la colonne de filtres reste entièrement déployée pendant la lecture d'une recette, où elle ne sert à rien.

Sur le mode sombre précisément, deux artifices du style Vintage ne se transposaient pas. Les deux dégradés radiaux bruns qui simulent l'inégalité du papier se lisaient comme un voile chaud posé sur l'écran — l'effet « mode nuit » dont Val se plaignait. Et le décalage rouge sous les grands titres, qui cite le défaut de repérage des presses bon marché sur papier crème, ne cite plus rien sur fond sombre : il ressemble à un défaut de rendu.

Ce qui fonctionne et qu'il ne faut pas casser : le regroupement automatique par type de plat qui s'efface dès qu'on filtre sur cette facette ; les compteurs par pastille, qui évitent les culs-de-sac ; le calcul du mode sombre par luminance plutôt que par clarté HSL ; la séparation nette entre jetons de mise en page et jetons de peau.

## 2. Ce qui a été corrigé

Thème et mode sombre. `DARK.paperChroma` passé de `.20` à `0` : le papier sombre est un gris strictement neutre (chroma mesurée à 0,000 après correction, contre 0,016 avant). Fond plat en sombre pour le Vintage, décalage rouge des titres supprimé en sombre. La couleur d'alerte ne passe plus par un plancher de saturation d'accent : elle virait au vert citron en sombre, une couleur qui n'appartenait à aucune palette et criait plus fort que l'accent.

Contraste. `toDark()` lifte désormais l'accent contre le fond de page **et** contre le fond de carte pour les styles où l'accent sert de texte. Il était mesuré à 4,01:1 sur les cartes, sous le seuil AA.

Bruit. Badge de provenance retiré des cartes (conservé sur la fiche, et retiré automatiquement si l'URL ne répond plus) ; ligne de tags passée à l'encre secondaire et débarrassée du tag qui répète l'intertitre ; un seul vocabulaire de filet par objet ; pastilles de filtre sans cadre ni ombre tant qu'elles ne sont pas actives ; facettes repliables, état mémorisé, toutes ouvertes par défaut ; accent rendu à l'action principale, à l'état actif d'un filtre, aux numéros d'étape et aux étoiles.

Thème verrouillé. `THEME_VERROUILLE = true` : les rangées « Style » et « Couleurs » ne sont plus construites. Les six styles et leurs 36 palettes restent intégralement dans le fichier ; repasser le drapeau à `false` rallume tout. Le bouton Clair / Sombre reste accessible — c'est un réglage de confort de lecture, pas un choix esthétique.

### Une correction annulée, et pourquoi — à ne pas refaire

La vignette absente avait d'abord été réduite à un bandeau plat pour récupérer le vide. **Val a signalé la régression : dans une même rangée, les cartes sans photo voyaient leur titre remonter d'environ 150 px pendant que les autres restaient en bas.** La grille perdait sa ligne de base commune, ce qui coûte plus à la lecture que le vide ne coûtait.

Règle qui en sort : une grille de cartes ne se lit que si toutes les cartes ont la même anatomie. La vignette garde donc sa hauteur **dans la liste**, et seule la redondance (le titre écrit deux fois) est retirée. Sur la **fiche** il n'y a pas de voisine avec qui s'aligner : là le cartouche est bien réduit à un bandeau, et les 500 px sont récupérés pour de bon. Alignement vérifié après correction : écart de 0 px entre les titres d'une rangée mixte.

### Contrastes mesurés après correction (sur fond de carte)

| Élément | Sombre | Clair |
|---|---|---|
| Texte principal (`--ink`) | 10,48:1 | 14,6:1 |
| Texte secondaire (`--ink-soft`) | 5,05:1 | 6,81:1 |
| Accent | 4,55:1 (était 4,01) | 5,57:1 |
| Ligne de tags | 5,05:1 | 6,81:1 |

## 3. Notes personnelles — architecture

Deux jugements séparés : la note (étoiles 1 à 5, portée sur une recette déjà cuisinée) et l'envie (« à tester », portée sur une recette qu'on n'a pas encore faite). Les fondre dans un seul curseur obligerait à noter ce qu'on n'a pas goûté, ou à perdre la liste d'envies dès qu'on note.

Règle d'affichage : une carte non notée ne montre rien ; ses étoiles n'apparaissent qu'au survol ou au focus clavier. Afficher cinq étoiles sur 119 cartes ajouterait 595 glyphes à une page dont le défaut était déjà l'encombrement.

Le marque-page prend l'encre et non la couleur d'alerte : le Vintage est un thème à deux encres, un troisième ton y introduisait une couleur qui n'appartient à rien. La forme (losange plein contre losange vide) suffit à dire l'état.

Stockage en deux couches, parce qu'une page ouverte en `file://` ne peut pas écrire dans son propre dossier : localStorage en écriture immédiate, et un `recettes-notes.js` exporté à la main, rechargé au démarrage comme socle. Chaque entrée porte son horodatage ; la fusion se fait recette par recette, la plus récente gagne. Un export ancien ne peut donc pas écraser une note récente.

Un piège rencontré et corrigé, à retenir : l'écouteur délégué des étoiles doit être posé **en phase de capture**. L'ouverture de la recette est branchée sur `.card` ; en phase de remontée, elle se déclenche avant un écouteur posé sur `document`, et cliquer une étoile ouvrait la recette.

## 4. Gestion des images depuis la page

Besoin de Val : un bouton, sur son ordinateur sous Brave, qui ouvre le sélecteur de fichiers GNOME pour ajouter, remplacer ou supprimer l'image d'une recette — web ou locale.

**Mécanisme.** API File System Access. `showDirectoryPicker()` demande une fois l'accès en écriture au dossier du carnet, la poignée est conservée dans IndexedDB, et aux visites suivantes le navigateur ne redemande qu'une confirmation. Disponibilité vérifiée en `file://` sur moteur Chromium : contexte sécurisé `true`, `showOpenFilePicker` / `showSaveFilePicker` / `showDirectoryPicker` présents, IndexedDB fonctionnel.

**Repli.** Si le navigateur ou l'utilisateur refuse l'accès au dossier, la photo n'est pas perdue : elle est enregistrée redimensionnée dans le fichier de notes sous forme de donnée encapsulée. Fonctionne partout, Firefox compris, au prix d'un fichier de notes plus lourd. Un message le dit explicitement quand ce repli s'active.

**Redimensionnement systématique** à 1400 px, JPEG qualité 0,82, avant toute écriture. Une photo d'iPhone brute pèse 3 à 5 Mo. Mesure de contrôle : 3000×2000 / 35 Ko → 1400×933 / 8 Ko.

**Piège HEIC.** Chromium ne décode pas le HEIC. Une photo transférée telle quelle depuis un iPhone échouera ; le message d'erreur le dit et renvoie vers le réglage « Le plus compatible » de l'iPhone.

**Point d'architecture central : `recettes-data.js` n'est jamais modifié par la page.** Un changement d'image est enregistré comme un remplacement dans `recettes-notes.js`. Deux fichiers, deux propriétaires — l'agent d'ingestion écrit les recettes, la page écrit les notes et les images — donc aucun conflit possible entre eux, et « rétablir l'image d'origine » fonctionne toujours puisque l'originale n'a jamais été écrasée. Le champ `img` absent signifie « pas de remplacement », `img:""` signifie « image retirée volontairement ».

Nommage : `images/<id>-<code base36>.jpg`. Un nom neuf à chaque écriture évite les problèmes de cache, et l'ancien fichier que la page avait elle-même écrit est supprimé au passage.

## 5. Hébergement — tranché

Val gère GitHub lui-même : téléversement manuel des fichiers, HTML renommé `index.html`, dépôt public, **consultation seule depuis l'iPhone, aucune modification**. Aucune intégration GitHub n'est donc construite dans la page : pas de jeton, pas d'appel d'API, aucun secret dans le navigateur. C'est la configuration la plus sobre possible.

Conséquence pratique : les photos ajoutées localement étant de vrais fichiers dans `images/`, elles se téléversent telles quelles. Les chemins enregistrés sont relatifs, donc ils fonctionnent aussi bien en local que servis par Pages. Le `recettes-notes.js` exporté emporte étoiles et images vers la copie consultée sur le téléphone.

## 6. Vérifications

Passées, sous Chromium, réseau coupé : aucune erreur JS ; notation, marque-page, filtre par note, tri « mieux notées », repli de facette, persistance après rechargement ; disparition des 95 badges de la grille ; rangées Style et Couleurs non construites ; contrastes dans les deux modes ; alignement des titres en rangée mixte à 0 px d'écart ; panneau image — pose d'URL, rejet d'une adresse invalide, retrait, rétablissement, persistance et export du remplacement ; chaîne de redimensionnement.

**Non vérifiable sans Val, et c'est le seul point ouvert :** l'ouverture réelle du sélecteur GNOME et l'écriture effective dans `images/`. Ces deux opérations exigent un clic humain et une fenêtre native, qu'un navigateur piloté ne peut pas produire. La disponibilité de l'API a été vérifiée, son invocation ne peut l'être que par un vrai clic. Si Brave refuse au moment de l'appel, le repli s'active et le message d'erreur le dira.
