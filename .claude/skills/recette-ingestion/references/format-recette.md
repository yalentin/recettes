# Format d'un enregistrement de recette

Chaque recette est un objet JavaScript ajouté au tableau `window.RECETTES` de
`recettes-data.js`. Voici le schéma complet, champ par champ.

## Schéma

```js
{
  id:      "identifiant-unique",   // sans espace ni accent ; sert aussi de nom d'image
  title:   "Titre lisible de la recette",
  image:   "images/identifiant-unique.jpg",  // ou null si pas de photo
  tags:    ["Plat", "Riz", "Chaud", "Japonais"],  // uniquement des tags de FACETTES
  serves:  4,        // nombre de personnes de la recette de BASE (entier > 0)
  prep:    15,       // minutes de préparation (nombre) ou null
  cook:    35,       // minutes de cuisson (nombre) ou null
  source:  "Capture d'écran — blog X",  // provenance, pour la traçabilité
  ingredients: [
    { item: "Riz basmati",  qty: 200,  unit: "g" },
    { item: "Ail", prep: "haché", qty: 2, unit: "gousses" },  // item = nom canonique ; prep = préparation
    { item: "Sel",          qty: 1,    unit: "c. à c.", warn: true },
    { item: "Poivre noir",  qty: null, unit: "au goût", warn: true },
  ],
  steps: [
    "Première étape, une phrase claire à l'impératif.",
    "Deuxième étape…",
  ],
  notes: "Astuce ou variante, ou null.",
}
```

## Règles par champ

- **id** : minuscules, accents retirés, espaces remplacés par `-`, caractères
  non alphanumériques retirés. Unique dans tout le fichier (suffixe `-2`, `-3`…
  en cas de collision). Exemple : « Riz aux lentilles & oignons » →
  `riz-aux-lentilles-oignons`.
- **title** : tel qu'on veut le lire. Garde les accents et la ponctuation.
- **image** : deux formats valides — chemin local `"images/<id>.jpg"` si une photo a été enregistrée, ou URL absolue distante `"https://..."` si l'image provient du web (repli web). Sinon `null` (valeur par défaut).
- **imageWeb** (optionnel) : `true` uniquement si l'image vient d'internet
  (illustration). Déclenche un badge « illustration web » dans la visionneuse.
  À omettre si l'image vient de la source ou s'il n'y a pas d'image.
- **tags** : liste de chaînes, toutes présentes dans `window.FACETTES` (ou
  proposées au registre et posées en connaissance de cause — voir SKILL §6).
  Plusieurs tags d'une même facette sont permis.
- **serves** : entier strictement positif. Si la source ne le précise pas,
  choisir une valeur plausible (2 ou 4) **et** le signaler en ALERTE.
- **prep / cook** : durées en minutes, en nombres (pas « 15 min »). Mettre `null`
  si l'information est absente (ne pas inventer). La visionneuse calcule le
  total et le format d'affichage.
- **source** : courte mention d'origine. Sert au journal et à une éventuelle
  revérification ultérieure.
- **ingredients** : tableau d'objets.
  - `item` : **nom canonique** de l'ingrédient (celui du registre
    `recettes-ingredients.js` / `window.INGREDIENTS`). Désigne le produit
    **acheté**, pas son état préparé. Voir SKILL §6 bis.
  - `prep` (optionnel) : préparation — « haché », « émincé », « râpé »,
    « grillé », « en dés »… Omettre s'il n'y en a pas. C'est ce champ, et non
    `item`, qui porte la préparation, pour que la liste de courses regroupe
    « Ail » et « Ail haché ». Attention : « séché / en boîte / surgelé / en
    poudre » font partie du **produit** (donc du `item`), pas de `prep`.
  - `qty` : nombre, ou `null` si pas de quantité chiffrée.
  - `unit` : unité (« g », « ml », « c. à s. », « c. à c. », « gousses »,
    « morceau »…) ou chaîne vide `""` pour les ingrédients qui se comptent à
    l'unité (ex. « Oignon », qty 2, unit ""). Si `qty` est `null`, mettre la
    mention dans `unit` (« au goût »).
  - `warn` : `true` pour les assaisonnements à dosage non linéaire (voir SKILL
    §5). Omettre (ou `false`) sinon.
- **steps** : tableau de phrases. Une étape = une action. À l'impératif, concis.
  Ne pas numéroter à la main : la visionneuse numérote. Si la source ne donne
  aucun mode opératoire, les étapes peuvent être **reconstruites** dans le cadre
  strict du SKILL §7 bis — et alors seulement, avec le double marquage ci-dessous.
- **notes** : texte libre court, ou `null`. Cas particulier : si les étapes ont
  été reconstruites (SKILL §7 bis), `notes` **commence obligatoirement** par
  `⚠️ Étapes reconstruites par l'agent (absentes de la source).`, et `source`
  reçoit le suffixe ` — étapes reconstruites`. C'est la seule trace qui survit à
  l'archivage du journal : `notes` est affiché par la visionneuse, `source` ne
  l'est pas mais reste lisible dans les données.

## Exemple complet

```js
{
  id:"riz-lentilles-oignons",
  title:"Riz aux lentilles et oignons frits",
  image:"images/riz-lentilles-oignons.jpg",
  tags:["Plat","Riz","Légumineuses","Chaud","Vegan"],
  serves:4, prep:15, cook:35,
  source:"Recette personnelle",
  ingredients:[
    {item:"Riz basmati", qty:200, unit:"g"},
    {item:"Lentilles vertes", qty:150, unit:"g"},
    {item:"Oignon jaune", prep:"émincé", qty:3, unit:""},
    {item:"Huile d'olive", qty:3, unit:"c. à s."},
    {item:"Cumin moulu", qty:1, unit:"c. à c.", warn:true},
    {item:"Sel", qty:1, unit:"c. à c.", warn:true},
  ],
  steps:[
    "Cuire les lentilles 20 minutes à l'eau bouillante, puis égoutter.",
    "Faire frire les oignons émincés dans l'huile jusqu'à coloration ; en réserver la moitié.",
    "Ajouter le riz, le cumin et le sel aux oignons restants, couvrir d'eau, cuire 12 minutes à couvert.",
    "Incorporer les lentilles, reposer 5 minutes, garnir des oignons réservés.",
  ],
  notes:"Plat à deux féculents : porte les tags Riz et Légumineuses.",
}
```

## Pièges à éviter

- Ne pas écrire les durées sous forme de texte (« 1 h ») : utiliser des minutes
  (`60`).
- Ne pas inventer une quantité « au pif » pour combler un trou : `qty: null` +
  ALERTE si l'information manque mais que l'ingrédient est nécessaire, ou
  `_à-revoir/` si la recette entière est trop lacunaire. La tolérance du §7 bis
  porte **uniquement** sur `steps` : elle ne s'étend ni à `qty`, ni à
  `ingredients`, ni à `title`.
- Ne pas reconstruire des étapes en y glissant des durées ou températures
  précises non déductibles : une durée incertaine s'omet (`cook: null`) plutôt
  que de se deviner.
- Ne pas créer un tag à la volée dans `tags` sans passer par le registre.
- Ne pas oublier la virgule entre deux objets du tableau, ni le `};` mal placé :
  une erreur de syntaxe casse tout le fichier (d'où la validation obligatoire).
