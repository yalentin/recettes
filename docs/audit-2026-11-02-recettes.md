# Audit des recettes — 2 novembre 2026

État audité : 120 recettes (`recettes-data.js` au commit `4adefba`).
Rien n'a encore été modifié dans les recettes : ce document liste les
propositions, Val tranche, puis on applique.

## Constat général

**Il n'y a pas de vrai doublon dans le carnet.** J'ai comparé les ingrédients de
toutes les recettes deux à deux (hors sel, poivre, eau, huile, ail, oignon,
sucre) : la paire la plus proche partage 39 % de ses ingrédients (nouilles
froides chinoises / nouilles udon), et seules huit paires dépassent 30 %.

L'impression de « recettes très similaires » vient surtout des **titres** : des
recettes différentes portent les mêmes mots (« za'atar », « citron noir »,
« tatin », « taboulé », « lentilles corail ») sans que le titre dise ce qui les
distingue. Le vrai problème de rangement est ailleurs : quelques **tags faux ou
incohérents**, qui faussent les filtres.

D'où la priorité proposée : corriger les tags, renommer pour distinguer,
fusionner très peu (voire pas du tout).

---

## A. Erreurs de tags (corrections factuelles)

| Recette | Problème | Correction |
|---|---|---|
| Légumes à la noix de coco et au maïs | Taguée « Viande / poisson » alors qu'elle n'en contient pas (la sauce d'huître a été remplacée par la version aux champignons) | Retirer « Viande / poisson », ajouter « Vegan » |
| Liseron d'eau sauté à l'ail | Entièrement végétale, pas taguée | Ajouter « Vegan » |
| Mezze d'aubergines et de tomates | Végétale à l'huile d'olive (le ghee est une option) | Ajouter « Vegan » |
| Rouleaux de printemps vegan | Ce sont des rouleaux, absents du filtre | Ajouter « Dumplings / rolls » |
| Nouilles udon épicées, sauce tofu et soja texturé | Contient des protéines de soja texturées : le tag « Soja » existe pour ça | Ajouter « Soja » |
| Riz aux champignons et kombu | Champignons au cœur du plat | Ajouter « Champignons » |
| Tian de légumes au chèvre | Les autres plats au chèvre portent « Fromage » | Ajouter « Fromage » |
| Pâte à galettes de blé noir | Aucune température | Ajouter « Chaud » |
| Sauce arachide (mafé) aux légumes | Le laurier est dans les étapes mais pas dans la liste (donc absent des courses) | Ajouter 1 feuille de laurier |

## B. Ingrédient principal : une règle à fixer

Le tag « Riz » est posé sur certaines recettes simplement servies avec du riz
blanc (Œufs à la tomate, Nasu dengaku, Mapo tofu) et pas sur d'autres dans le
même cas (Curry vert thaï, Mafé de légumes). Idem pour « Œufs » quand l'œuf ne
sert que de liant. Résultat : filtrer « Riz » ou « Œufs » remonte des plats où
ce n'est pas l'ingrédient qu'on cherche.

Règle proposée : **l'ingrédient principal, c'est ce qui fait le plat** — pas
l'accompagnement servi à côté, pas un liant.

| Recette | Correction |
|---|---|
| Œufs à la tomate (tomato egg) | Retirer « Riz » |
| Nasu dengaku | Retirer « Riz » |
| Mapo tofu végétarien | Retirer « Riz » |
| Poivrons à la sicilienne | Retirer « Œufs » (liant de la farce) |
| Tarte épicée au bœuf | Retirer « Œufs » (liant de l'appareil) |
| Gratin de chou-fleur | Retirer « Œufs » (liant de la béchamel) |

## C. Titres : distinguer les recettes qui se ressemblent

Les identifiants ne changent pas : notes, étoiles et photos suivent.

| Titre actuel | Titre proposé | Pourquoi |
|---|---|---|
| Roulés au fromage et au za'atar | Roulés feuilletés au fromage et au za'atar | Presque le même titre que la suivante |
| Rouleaux au halloumi et au za'atar | Rouleaux frits au halloumi et au za'atar | Idem (pâte à wonton, friture) |
| Gondi de tofu et sauce tomate au citron noir séché | Gondi (boulettes de tofu), sauce tomate au citron noir | Se confond avec « Tofu au citron noir séché et aux herbes » |
| Taboulé (salade de blé concassé et persil) | Taboulé libanais au boulgour et au persil | Deux taboulés très différents |
| Taboulé maison à la semoule, grenade et baies de goji | Taboulé à la semoule, grenade et goji | Idem |
| Sauce arachide (mafé) aux légumes | Mafé de légumes (sauce arachide) | Rangé à côté de l'autre mafé |
| La tatin du soleil | Tatin de tomates et burrata (tatin du soleil) | Famille des tatins : le titre doit dire laquelle |
| Tarte tatin d'échalote et burrata | Tatin d'échalotes et burrata | Idem |
| Tarte tatin au potimarron, oignons confits et chèvre | Tatin de potimarron, oignons confits et chèvre | Idem |
| La tarte à l'oignon doux et au thym d'Anouk | Tarte à l'oignon doux, au chèvre et au thym | Article et nom propre en tête, chèvre absent du titre |
| Soupe aux lentilles et carottes | Soupe de lentilles vertes aux carottes | Quatre soupes de lentilles : nommer la lentille |
| Soupe de corail aux oignons frits | Soupe de lentilles corail aux oignons frits | « corail » seul est obscur |
| Lentilles corail à la marocaine (feta light ou œuf) | Lentilles corail à la marocaine | L'option n'a pas sa place dans le titre |
| Soupe tomates (à la viande) | Soupe de tomates à la viande | Grammaire |
| Mes baklavas préférés | Baklavas | « Mes » est celui de l'auteur du livre, pas le tien |
| Nouilles froides chinoises | Nouilles froides à la chinoise, sauce sésame pimentée | Trois plats de nouilles au sésame : dire lequel |
| Œufs brouillés pimentés | Œufs brouillés pimentés à la tomate | Range avec les autres œufs à la tomate |
| Pâte à galettes de blé noir traditionnelle | Galettes de blé noir | Plus simple à chercher |

## D. Les recettes avec viande ou poisson (29 sur 120)

Le carnet est végétarien par défaut, mais près d'un quart des recettes portent
« Viande / poisson ». Elles ne sont pas toutes du même ordre.

**1. Viande ou poisson seulement dans un bouillon ou un condiment** (substitution
simple, déjà annoncée dans la note pour certaines) :
- Soupe aux lentilles et carottes — bouillon de volaille
- Soupe de corail aux oignons frits — cube de volaille
- Salade de nouilles de riz fraîches à la vietnamienne — nuoc-mâm
- Œufs pochés à la sauce tomate, anchois et ricotta — anchois (la note dit déjà de les omettre)
- Mapo tofu végétarien — **incohérent** : propose un cube de bouillon de poule en premier choix sans porter le tag

Proposition : version végétarienne par défaut (bouillon de légumes, sauce soja
ou nuoc-mâm végétal, anchois en option dans la note), tag retiré — comme le
carnet le fait déjà pour la sauce d'huître.

**2. Viande ou poisson accessoire mais qui structure la recette** : Quiche au
cottage-cheese (lardons), Pommes de terre farcies au chèvre et aux lardons,
Spaghetti all'acciugata, Spaghetti alla carbonara, Bibimbap au thon, Lentilles
aux poireaux et saucisse fumée, Pois chiches à la Casas, Croquettes de pois
chiches, Couscous, Chili con carne.
Les végétaliser changerait la recette : à garder telles quelles, sauf avis
contraire.

**3. La viande est le plat** : Vol-au-vent, Chou chinois au porc, Carbonnades,
Curry de chou au poulet, Tarte épicée au bœuf, Côtelettes d'agneau, Tajine de
pruneaux, Poulet à la crème d'estragon, Dinde à l'ananas, Tajine de boulettes
de bœuf, Filets mignons à l'orange, Tajine d'agneau aux abricots, Soupe de
tomates à la viande, Curry vert thaï au poulet.
Beaucoup viennent du carnet familial : c'est à toi de dire si elles restent
comme archive, ou si elles sortent (elles resteraient lisibles dans
l'historique Git).

## E. Fusions envisagées

Le format du carnet n'a qu'une liste d'ingrédients par recette, et le
planificateur de courses s'en sert. Fusionner deux recettes distinctes revient
à en faire une « variante » rangée dans les notes, donc absente des courses.
C'est pourquoi je ne recommande **aucune fusion** : aucune paire n'est un vrai
doublon.

Les deux seules paires discutables :
- **Les deux mafés** (protéines texturées au lait de coco / légumes à la sauce
  arachide) : 4 ingrédients communs, bases différentes (farine de cacahuète et
  coco contre beurre de cacahuète). Le renommage (C) suffit à les ranger ensemble.
- **Nouilles soba sésame-miso / Nouilles froides chinoises** : même principe
  (nouilles froides, sauce sésame), mais l'une est japonaise au miso, l'autre
  chinoise à l'huile pimentée. La seconde est mal renseignée (type et quantité
  de nouilles absents) : la compléter vaut mieux que l'absorber.

## F. Cuisines

À compléter (actuellement sans cuisine) :
- Riz sauté aux légumes → Chinois
- Poivrons marinés → Italien
- Caviar d'aubergines → Français
- Lentilles aux poireaux et saucisse fumée → Français

À reclasser :
- Taboulé à la semoule → Français au lieu de Moyen-oriental (l'autrice dit
  elle-même que c'est un taboulé « pas libanais » : c'est le taboulé à la française)
- Chili con carne → Nord-américain au lieu de Mexicain (c'est un plat tex-mex)

Les 12 autres recettes sans cuisine (Potée de lentilles, Butternut hasselback,
Chou rôti au tahin…) n'ont pas d'origine marquée : les laisser sans tag.

## G. Recettes à compléter (pas à supprimer)

- Nouilles froides chinoises : type et quantité de nouilles absents
- Pain fourré aux pommes de terre et pesto rouge : aucune quantité dans la source
- Tatin au potimarron : aucune quantité sauf le potimarron et la pâte
- Tatin du soleil : presque tout « au goût »
- Dinde à l'ananas : 100 g de dinde pour 4, quantité suspecte
- Étapes reconstruites, à relire avant de cuisiner : Riz sauté aux légumes,
  Filet O' Tofu, Okonomiyaki, Tatin du soleil

## H. Pour que ça ne se reproduise pas

Si les règles ci-dessus sont validées, les inscrire dans le skill d'ingestion
(§ 6 et § 9) :
- ingrédient principal = ce qui fait le plat, ni accompagnement ni liant ;
- titre = le plat puis ce qui le distingue ; pas d'article ni de possessif en
  tête, pas de nom d'auteur ; quand une recette de la même famille existe déjà,
  le titre doit permettre de les distinguer d'un coup d'œil ;
- selon l'arbitrage D : bouillon de volaille, nuoc-mâm et anchois d'assaisonnement
  remplacés par défaut, comme la sauce d'huître.
