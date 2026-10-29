CARNET DE RECETTES — structure du projet
=========================================

CONSULTATION (ce dont tu te sers au quotidien)
----------------------------------------------
recettes.html        Le moteur + le style. Tu l'ouvres pour consulter. Contient
                     aussi le "Planificateur de courses" (bouton dans la barre
                     latérale) : choisis des envies + un nombre de recettes/
                     personnes, il tire des recettes au hasard et dresse la liste
                     de courses agrégée par rayon.
recettes-data.js     TES RECETTES + le vocabulaire de tags (FACETTES, FACETTES_MODE).
                     Seul window.RECETTES est destiné à grandir (ajout en fin).
recettes-ingredients.js  Registre des noms d'ingrédients (window.INGREDIENTS) :
                     orthographe canonique + alias + rayon. Sert à la liste de
                     courses du planificateur. Lu par recettes.html AVANT les données.
recettes-notes.js    TES ÉTOILES, TES FAVORIS et tes images (window.NOTES_DISQUE).
                     Écrit par le bouton « Mes notes › Enregistrer sur le disque ».
                     Facultatif : s'il manque, le carnet fonctionne sur les
                     seules notes mémorisées par le navigateur.
images/              Photos enregistrées localement : images/<id>.jpg. En pratique
                     ce dossier est presque vide : la quasi-totalité des recettes
                     pointent vers une URL externe (champ image + imageWeb:true),
                     affichée avec un badge « illustration web ».

APPARENCE (bouton dans la barre latérale)
------------------------------------
Trois styles, au choix dans le menu Apparence (mémorisé par navigateur) :
  Liquid Glass   par défaut à l'ouverture (depuis octobre 2026) : panneau de
                 verre, police système, couleurs Apple, carte qui rebondit
                 légèrement au survol avec un halo rouge.
  Vintage        fiche imprimée deux encres, papier crème, Rouge cerise.
  Éditorial      le carnet lu comme un livre.
Aqua, Suisse, Brutal et Mono ont été SUPPRIMÉS le 3 octobre 2026. La version
qui les contenait encore est dans .sauvegardes/
(recettes.html.*.avant-suppression-4-themes).

Le style par défaut se règle en tête de recettes.html : STYLE_DEFAUT.

  Mode      Auto (suit le système), Clair ou Sombre. Réglage de confort de
            lecture, pas un choix esthétique.
  Images    « Vignettes » affiche les photos quand elles existent.
            « Compact » masque toutes les images et densifie les cartes.

MES NOTES (étoiles et favoris)
------------------------------------
Chaque recette peut recevoir une note de 1 à 5 étoiles et un cœur « favori ».
Les deux sont indépendants et ne disent pas la même chose : cinq étoiles
veulent dire « c'était très bon », le cœur veut dire « celle-là, je la refais
sans hésiter ». Ce ne sont pas les mêmes recettes.

  Où noter     Sur la carte dans la liste (étoiles et cœur apparaissent au
               survol si la recette n'est pas encore notée) et en haut de la
               fiche.
  Retrouver    Facette « Mes notes » en bas de la colonne de gauche :
               ♥ Favoris, ★★★★★, ★★★★ et +, ★★★ et +, pas encore notée.
               Et le tri « Mieux notées » en haut à droite de la liste.

OÙ VIVENT LES NOTES — et pourquoi il faut les enregistrer
Une page ouverte en file:// ne peut pas écrire dans son propre dossier :
aucun navigateur ne l'autorise. Les notes sont donc stockées en deux couches.

  1. Dans le navigateur (localStorage), écrites immédiatement à chaque clic.
     Ça survit à la fermeture du fichier et au redémarrage de la machine.
     Ça NE survit PAS à un nettoyage des données de navigation, et ça ne
     suit pas sur un autre navigateur ni sur une autre machine.
  2. Dans recettes-notes.js, à côté de recettes.html. C'est la copie durable,
     celle qui part avec la sauvegarde du dossier et que l'agent peut lire.
     Elle est rechargée au démarrage comme socle.

Le bouton « Mes notes » de la colonne de gauche affiche le nombre de
modifications qui ne vivent encore que dans le navigateur, et propose
« Enregistrer sur le disque » : ça télécharge un recettes-notes.js qu'il faut
déposer dans le dossier du carnet, par-dessus l'ancien. À faire de temps en
temps — pas à chaque étoile.

Fusion : chaque note porte son horodatage. Au chargement, le fichier du
disque et les notes du navigateur sont fusionnés recette par recette, et la
plus récente gagne. Un export ancien ne peut donc pas écraser une note
récente, ni l'inverse. « Recharger depuis un fichier… » applique la même
règle et sert à récupérer ses notes sur une autre machine.

LES IMAGES (depuis la page, bouton « Image… » sur la fiche)
------------------------------------
Sur la fiche d'une recette, sous la photo : « Image… » ouvre un panneau qui
permet de remplacer l'image par une photo de l'ordinateur ou par une adresse
web, de la retirer, ou de rétablir celle d'origine.

« Choisir une photo sur l'ordinateur… » ouvre le sélecteur de fichiers de
GNOME. La photo est redimensionnée à 1400 px, réencodée en JPEG, et écrite
comme VRAI FICHIER dans images/<id>-<code>.jpg. Une photo d'iPhone brute pèse
3 à 5 Mo : la réduire évite d'alourdir le dossier et le dépôt pour rien.

IL N'Y A PAS DE PLAN B. Soit la photo est écrite dans images/, soit rien n'est
enregistré et le message dit pourquoi. Une version antérieure savait replier
la photo à l'intérieur du fichier de notes quand l'écriture échouait : c'était
une mauvaise idée, ça dispersait les images entre deux endroits. Supprimé.

PREMIÈRE UTILISATION — ET LE RÉGLAGE BRAVE À CONNAÎTRE
Ça repose sur l'API File System Access.

  BRAVE LA DÉSACTIVE PAR DÉFAUT. Il faut ouvrir
      brave://flags/#file-system-access-api
  passer le réglage sur « Enabled », et relancer Brave. Sans ça, le bouton
  répond que le navigateur n'expose pas l'API — ce n'est pas un bug du carnet.

  Firefox ne l'implémente pas du tout : sous Firefox, ajouter une photo depuis
  la page est impossible, point.

Ensuite, « Autoriser l'accès au dossier du carnet… » et DÉSIGNER LE DOSSIER
QUI CONTIENT recettes.html. Une seule fois : l'autorisation est mémorisée. Si
tu désignes un autre dossier, la page refuse et le dit — elle vérifie qu'il
contient bien recettes-data.js, pour ne pas semer un images/ fantôme ailleurs.

recettes-data.js N'EST JAMAIS MODIFIÉ par la page. Un changement d'image est
enregistré comme un remplacement dans recettes-notes.js. Deux fichiers, deux
propriétaires : l'agent d'ingestion écrit les recettes, la page écrit tes
notes et tes images. Aucun conflit possible, et « rétablir l'image d'origine »
fonctionne toujours puisque l'originale n'a jamais été écrasée.

Conséquence pour l'export : une image que tu as ajoutée compte comme une
modification de notes. Pense à « Mes notes › Enregistrer sur le disque » pour
que le remplacement parte avec le reste.

INGESTION AUTOMATIQUE (la partie agent, via Claude Cowork)
----------------------------------------------------------
dépôt/               Tu y déposes tes sources en vrac (captures, photos, textes,
                     liens, vidéos). Le skill les traite puis les déplace.
  _traité/           Sources intégrées avec succès (archive, ne rien supprimer).
  _à-revoir/         Sources illisibles ou douteuses, à reprendre à la main.
journal/             Compte rendu du skill, un par exécution (AAAA-MM-JJ.md),
                     avec en tête une section ALERTES. À LIRE après chaque exécution.
recettes-tags.md     Registre des tags : approuvés (= FACETTES) et proposés par le
                     skill. Toi seul approuves un tag (et l'ajoutes à FACETTES).
.sauvegardes/        Copies de recettes-data.js faites avant chaque écriture.
skills/
  recette-ingestion/ Le skill que Cowork exécute. SKILL.md + references/.

FONCTIONNEMENT DE L'AGENT (résumé)
----------------------------------
1. Tu déposes des sources dans dépôt/.
2. Le skill (à la demande ou planifié chaque semaine via /schedule dans Cowork) :
   sauvegarde recettes-data.js, lit chaque source, la formalise au format
   standard, pose les tags approuvés (propose les manquants au registre), pose
   les drapeaux d'assaisonnement, passe des contrôles de cohérence, ajoute la
   recette à recettes-data.js, valide la syntaxe, déplace la source, et écrit le
   journal d'exécution.
3. Il NE TOUCHE JAMAIS à recettes.html ni à FACETTES.
4. Tu lis le journal, surtout la section ALERTES.

ÉTAPES RECONSTRUITES (à connaître)
-----------------------------------
Quand une source donne les ingrédients mais pas le mode opératoire, le skill ne
disqualifie plus la recette : si le procédé est standard et non ambigu (soupe,
vinaigrette, riz pilaf, gratin…), il écrit lui-même les étapes. Trois garde-fous :
- ça ne vaut QUE pour les étapes — jamais les quantités, les ingrédients ou le
  titre, qui restent soumis au « ne jamais inventer » ;
- si le procédé est délicat (pâtisserie, pâte levée, fermentation…), la recette
  part quand même en _à-revoir/ ;
- la recette est marquée deux fois : une ALERTE au journal, ET une mention
  « ⚠️ Étapes reconstruites par l'agent » en tête de ses notes, visible dans la
  visionneuse. Ces étapes sont plausibles, pas vérifiées : relis-les.

Limite de planification Cowork : une tâche planifiée ne tourne que si l'ordinateur
est allumé et Claude Desktop ouvert ; sinon elle s'exécute au réveil de la machine.

ENVOYER LE CARNET À QUELQU'UN
-----------------------------
Envoie recettes.html + recettes-data.js + recettes-ingredients.js. Les trois sont
nécessaires : sans le registre d'ingrédients, la liste de courses perd ses rayons.
recettes-notes.js est facultatif — et personnel : c'est TA notation. Ne l'envoie
que si tu veux transmettre tes étoiles avec le carnet.
Le dossier images/ est facultatif — les rares photos locales deviennent alors des
cartouches « pas de photo », tout le reste fonctionne.

NOTE HORS-LIGNE
---------------
Deux dépendances à internet, aucune bloquante :
- Les polices viennent de Google Fonts : sans réseau, une police système prend le
  relais. Pour un hors-ligne strict, on embarquera les polices plus tard.
- La quasi-totalité des photos sont des URL externes (illustrations web). Sans
  réseau, ou le jour où un blog source disparaît, elles ne s'affichent plus et
  laissent la place au cartouche « pas de photo ». Le texte des recettes, lui,
  est entièrement local et ne dépend de rien.
