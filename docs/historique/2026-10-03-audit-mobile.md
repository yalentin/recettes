> **Archive.** Document rédigé dans le projet claude.ai « Recettes » le 3 octobre
> 2026, déplacé ici le 29 octobre 2026. Les décisions qu'il contient restent
> valables et sont résumées dans `CLAUDE.md`. Une exception : la règle « sur écran
> tactile, la gestion d'image est masquée » a été levée en octobre 2026. Avec sa
> clé GitHub, Val peut désormais ajouter une photo depuis l'iPhone.

# Carnet de recettes — audit et corrections du 3 octobre 2026

Sauvegardes : `.sauvegardes/recettes.html.2026-10-03_*` avec les suffixes `avant-audit-apple`, `avant-titre-auto-planif`, `avant-vignette-sobre` et `avant-rouge`. Le CSS ajouté est en fin de `<style>`, sous l'en-tête « AUDIT DU 3 OCTOBRE 2026 ».

## Passe 1 — audit (vérifiée : 390 px tactile et 1440 px, clair et sombre, aucune erreur JS)

- iPhone : la colonne latérale est masquée en vue fiche sous 780 px. La fiche commence à 22 px au lieu de 1 451 px.
- iPhone : filtres repliés par défaut sous 780 px. La première carte passe de 1 564 px à 568 px.
- L'animation d'entrée des cartes ne joue qu'au premier affichage. Avant, elle ajoutait environ 0,9 s à chaque filtre.
- Cartes accessibles au clavier ; cibles tactiles d'environ 44 px ; sous-titre de l'en-tête qui n'apparaît plus en double ; titres de groupe en `h2` et de carte en `h3`.
- « Base N pers. » ne s'affiche plus que si l'on modifie les portions.

## Passe 2 — demandes de Val

- **Vignette sans photo = titre de la recette.** Version définitive après le retour de Val (la première, en Alfa Slab rouge avec filet double, était trop chargée et le titre était parfois coupé à mi-ligne à cause du `line-clamp`) : Zilla Slab italique 500, encre secondaire, filet fin, aucune coupe. La taille est en `cqi`, donc proportionnelle à la vignette 4/3 : le rapport entre texte et cadre est le même sur toutes les tailles d'écran. `ajusterTitres()` sert de filet de sécurité et réduit la police si un titre déborde. Vérifié avec les vraies polices (servies en local) sur les 119 titres, de 320 à 1440 px : aucun débordement, aucune réduction nécessaire.
- **Fiche sans photo** : pas de bandeau ; le bouton s'appelle « Ajouter une image… » ; si l'image d'origine ne répond plus, la légende dit « image d'origine introuvable ».
- **Mode clair / sombre** : Auto / Clair / Sombre, Auto par défaut. Il suit `prefers-color-scheme` et bascule en direct. `PREF_V` est passé à "7", ce qui remet une fois tout navigateur en Auto.
- **« Planifier mes courses »** : rangé avec les actions secondaires (même style que « Mes notes » et « Apparence »).
- **Fiche organisée pour le moment où l'on cuisine** : titre, photo, temps, puis ingrédients et étapes. « Ma note » et la gestion d'image passent après la recette. Sur écran tactile, la gestion d'image est masquée.

## Passe 3 — accent plus rouge, moins orangé

- **Clair** : l'accent passe de `#b3372c` (teinte à 5°, rouge brique tirant sur l'orange) à `#ae292d` (358°, rouge cerise). Il devient aussi un peu plus foncé (`accentD #841f22`). Contrastes : 5,5:1 sur la page, 6,2:1 pour le texte clair posé dessus.
- **Sombre** : c'est surtout là que le rouge tournait au saumon. `toDark()` éclaircit l'accent jusqu'à 4,5:1 parce qu'il sert aussi de texte, et un rouge éclairci à ce point devient forcément saumon ou rose. Le contraste n'est pas en cause, c'est une limite de la couleur elle-même. Solution : séparer les deux rôles avec des valeurs choisies à la main (`sombre:{…}` dans la palette Rouge cerise, appliqué à la fin de `toDark`).
  - Remplissage (pastilles actives, ET, étiquettes de filtre) : vrai rouge `#d1343b`. Texte clair dessus : 4,57:1. Contre la page : 3,27:1.
  - Texte d'accent (numéros d'étape, compteur de facette, quantité qui clignote) : `#ff6b70`, 5,8:1, via des règles `html[data-dark] … {color:var(--accent-d)}`.
  - Survol d'un élément rempli en sombre : `color-mix(accent 82 %, noir)`. L'ancienne règle `--accent-d` donnait 2,57:1 une fois `accent-d` devenu clair.
  - Les valeurs codées en dur pour le premier rendu (blocs `html[data-style="vintage"]` et `[data-dark]`) ont été mises à jour en même temps : pas de flash de couleur au chargement.

## Encore ouvert

- Fonctions « en cuisine » proposées et pas encore tranchées : écran qui reste allumé, cocher ingrédients et étapes, minuteurs tirés des étapes.
- Poignée de redimensionnement de la barre latérale : pas auditée.
