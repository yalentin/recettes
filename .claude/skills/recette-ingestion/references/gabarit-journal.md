# Gabarit du journal d'exécution

Écrire un fichier par exécution — `journal/AAAA-MM-JJ.md`, ou
`journal/AAAA-MM-JJ-HHMM.md` si plusieurs exécutions le même jour (ne jamais
écraser un journal existant) — sur ce modèle. La **section ALERTES vient en
premier**.

```markdown
# Journal d'ingestion — {date}

## ⚠️ ALERTES

> À lire en priorité. Tout ce qui demande ton attention.

- {Source} : {nature du problème}. {Action prise : ignoré / mis en _à-revoir / ajouté malgré tout}.
- {Recette} : valeur suspecte — {détail, ex. « 150 g de sel pour 2 personnes »}.
- {Recette} : **étapes reconstruites** — la source ne donnait {aucun mode opératoire / que les ingrédients}. Reconstruction appuyée sur {technique standard}. À relire.
- {Recette} : sans image (`image: null`) — recherche web menée, aucune correspondance forte.
- Tag proposé : « {tag} » (facette {Facette}) — déclenché par {recette}. À approuver dans recettes-tags.md puis FACETTES.
- …

(S'il n'y a rien : écrire « Aucune alerte. »)

## Résumé

- Sources trouvées dans le dépôt : {n}
- Recettes ajoutées : {n}
- Dont recettes à étapes reconstruites : {n}
- Sources mises en _à-revoir : {n}
- Tags proposés : {n}
- Validation syntaxique de recettes-data.js : {OK / ÉCHEC → restauré avec git checkout}
- Mise en ligne : {commit xxxxxxx envoyé / non envoyé — raison}

## Recettes ajoutées

| Titre | Tags | Ingrédients | Source |
|---|---|---|---|
| {titre} | {tags} | {n} | {source} |
| … | | | |

## Étapes reconstruites (détail)

(Omettre la section s'il n'y en a aucune.)

- {titre} : la source ne fournissait {aucune étape / un mode opératoire lacunaire}.
  Étapes écrites d'après {technique standard invoquée}. Marquage posé : `notes`
  préfixé + `source` suffixée. **À relire avant de cuisiner.**

## Tags proposés (détail)

- « {tag} » → facette {Facette}. Justification : {pourquoi}. Recette : {titre}.

## Sources mises en _à-revoir

- {fichier} : {raison précise — illisible, manuscrit incertain, vidéo sans texte, doublon…}

## Sources traitées et archivées

- {fichier} → _traité/ (recette : {titre})
```

## Conseils de rédaction

- Être **factuel et précis** dans les alertes : nommer la source, la recette, la
  valeur en cause. « Quantité douteuse » sans détail est inutile.
- Ne pas noyer les alertes réelles sous des remarques cosmétiques. Si tout va
  bien, le dire en une ligne.
- La table « Recettes ajoutées » est le filet de sécurité principal de
  l'utilisateur : la garder complète et lisible, une ligne par recette.
- Une recette à étapes reconstruites apparaît **trois fois** : dans les ALERTES,
  dans la table des recettes ajoutées, et dans la section dédiée. Ce n'est pas
  une redondance inutile : c'est le seul cas où l'agent a écrit du contenu que la
  source ne contenait pas.
