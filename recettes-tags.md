# Registre des tags

Ce fichier est la mémoire du vocabulaire de tags. Il a deux sections.

- **Approuvés** : reflet de `window.FACETTES` dans `recettes-data.js`. C'est la
  source de vérité. Quand tu approuves un tag proposé, ajoute-le ici **et** dans
  `FACETTES` (les deux doivent rester cohérents).
- **Proposés** : ce que le skill suggère mais que toi seul intègres. Le skill
  ajoute ici ; il ne touche jamais à `FACETTES`.

Le skill peut poser un tag *proposé* sur une recette : il restera invisible dans
les filtres tant qu'il n'est pas dans `FACETTES`. Donc une fois approuvé, rien à
reprendre sur les recettes déjà taguées — sauf celles antérieures à sa création,
qu'il faudra éventuellement re-baliser (opération de fond, à demander au skill).

---

## Tags approuvés

### Type de plat
Entrée · Plat · Accompagnement · Soupe · Salade · Sauce / condiment · Dumplings / rolls · Conserve / ferment · Apéritif · Boisson · Petit-déjeuner · Sucré

### Ingrédient principal
Nouilles · Riz · Pâtes · Céréales · Légumineuses · Légumes · Pomme de terre · Champignons · Tofu · Seitan · Tempeh · Soja · Œufs · Fromage · Fruits · Pâte / pain

> Note : « Soja » désigne uniquement ce qui n'est ni tofu ni tempeh (edamame, lait de soja, protéines de soja texturées).

### Température
Chaud · Froid

### Cuisine
Japonais · Coréen · Chinois · Thaï · Vietnamien · Asie du Sud-Est · Indien · Italien · Français · Belge · Grec · Est-européen · Baltique · Scandinave · Moyen-oriental · Maghrébin · Ibérique · Afrique subsaharienne · Nord-américain · Mexicain · Caraïbes · Sud-américain

> Note : « Caraïbes » couvre les cuisines insulaires de la zone (Porto Rico, Cuba, Jamaïque, Haïti, Antilles…), qui ne relèvent ni de « Mexicain », ni de « Sud-américain », ni d'« Ibérique ». Approuvé le 2026-08-04.

> Note : « Asie du Sud-Est » est un tag de repli pour les cuisines de la zone qui n'ont pas de tag propre (indonésienne, malaise, singapourienne, philippine…). Quand un pays a déjà son tag — Thaï, Vietnamien — c'est celui-là qu'on pose, pas le tag régional. Approuvé le 2026-08-02, en remplacement de la proposition « Indonésien ».

### Régime
Vegan · Viande / poisson

> Note : le carnet est végétarien par défaut. « Vegan » marque le plus strict ; « Viande / poisson » signale une recette contenant viande ou poisson (exception à repérer).

---

## Tags proposés (en attente d'approbation)

Format : `- « Tag » → facette {Facette}. Justification : {…}. Recette : {titre}. (proposé le AAAA-MM-JJ)`

*(aucun tag en attente)*

Historique des arbitrages :

- « Caraïbes » (proposé le 2026-08-04) → **approuvé** le 2026-08-04, ajouté à la facette Cuisine et à FACETTES. Recette déclenchante : Arroz con salchichas (version vegan), plat portoricain.

- « Indonésien » (proposé le 2026-08-02) → **non retenu**. Approuvé à la place « Asie du Sud-Est », tag régional couvrant l'indonésien et ses voisins sans tag propre. Recette concernée : Sauce satay (ex-Saté Ayam).

---

## Comment approuver un tag

1. Décider que le tag est pertinent et non redondant avec un tag existant.
2. L'ajouter à la facette correspondante dans `recettes-data.js`
   (`window.FACETTES`) **et** dans la section « Approuvés » ci-dessus.
3. Le retirer de la section « Proposés ».
4. Si d'anciennes recettes auraient dû le porter, demander au skill une passe de
   re-balisage ciblée (il relit le corpus et propose les ajouts).
