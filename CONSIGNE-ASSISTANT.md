# Consigne : créer un thème communautaire pour Atlas L1

> **À qui s'adresse ce document.** À un assistant d'IA à qui l'on demande de créer un thème pour
> le logiciel de magasin Atlas L1 **pour le dépôt communautaire de thèmes**, y compris un thème
> inspiré d'une œuvre existante (jeu, film, série, personnage, univers). Donnez-lui ce document,
> le fichier `gabarit.atlas-theme.json` et le vérificateur `check-theme.mjs`, puis décrivez le
> thème voulu. L'assistant doit suivre toutes les règles ci-dessous : un fichier qui en enfreint
> une seule est refusé par l'application.

## Ta mission

Produire **un seul fichier** `<nom>.atlas-theme.json` qui habille l'application selon la demande,
puis **le faire passer au vérificateur jusqu'à obtenir « OK »** :

```
node check-theme.mjs <nom>.atlas-theme.json
```

Ne livre jamais un fichier que le vérificateur refuse. Corrige d'après le motif affiché
(`REFUSED [code] détail`) et relance. Le vérificateur applique exactement les mêmes contrôles que
l'application : « OK » chez toi veut dire « accepté » dans le magasin.

Commence par copier `gabarit.atlas-theme.json` : il est valide tel quel.

## Ce qu'un thème peut et ne peut pas faire

Un thème change **l'apparence** de l'application, **jamais son comportement**. Il ne peut ni lire
une donnée, ni envoyer quoi que ce soit, ni cacher, déplacer ou recouvrir un élément, ni agir sur
le serveur. Toute tentative fait refuser le fichier entier. Il n'y a donc **aucun script**, **aucune
adresse externe** (ni image, ni police, ni feuille venues d'ailleurs) et **aucun texte ajouté**.

## Thème inspiré d'une œuvre existante

Ce dépôt accepte les **thèmes de fans**. Le nom de l'œuvre peut figurer dans le champ `nom`
(par exemple « Hommage à <œuvre> »). En contrepartie, ces règles sont strictes :

- **Tout est fait main.** Aucune image tirée de l'œuvre : ni sprite extrait, ni logo, ni capture
  d'écran, ni illustration officielle, ni scan. Les images du thème sont **dessinées par toi**
  (motifs, pixels, dégradés) en t'inspirant du style, sans le décalquer.
- **Aucun logo ni marque imités.** Pas de reproduction d'un logo, d'un emblème ou d'un lettrage
  officiel, même redessiné.
- **Les couleurs et l'ambiance sont libres** : une palette, une époque, une texture rappellent une
  œuvre sans la copier.
- **Jamais officiel.** Le thème ne se présente jamais comme produit, approuvé ou soutenu par le
  titulaire des droits. Le champ `licence` porte la mention
  `Thème de fans non officiel ; images originales CC-BY-4.0` (ou une licence libre équivalente).
- **Le nom de l'auteur** figure dans `auteur` : c'est lui qui répond de sa création.
- **Pas de contenu choquant** ni de nom de personne réelle.

## Le fichier

Un objet JSON en UTF-8, **256 Ko au plus** en tout.

| Champ | Obligatoire | Règle |
|---|---|---|
| `format` | oui | toujours `1` |
| `nom` | oui | 1 à 40 caractères |
| `auteur`, `licence`, `version` | non | texte de 80 caractères au plus ; licence libre recommandée (`CC0-1.0`, `CC-BY-4.0`) |
| `theme` | oui | voir ci-dessous |
| `css` | non | feuille de style, 64 Ko au plus, voir les règles |
| `images` | non | au plus 8 images, voir les règles |

**Aucun autre champ n'est accepté**, ni en haut du fichier, ni dans `theme`.

### `theme`

| Champ | Obligatoire | Règle |
|---|---|---|
| `fontFamily` | oui | une liste de familles ; **la première** doit être l'une de : `Inter Variable`, `Manrope Variable`, `IBM Plex Sans Variable`, `Source Sans 3 Variable`, `DM Sans Variable`, `Plus Jakarta Sans Variable`, `Press Start 2P`, `system-ui`, `Tahoma`, `Segoe UI`. Seuls lettres, chiffres, espaces, tirets, guillemets et virgules. |
| `prefersDark` | oui | `true` si le thème est pensé pour la nuit d'abord |
| `accent` | oui | couleur `#rrggbb` (boutons principaux, liens, sélection) |
| `neutrals` | non | `{ "hue": 0 à 360, "chroma": 0 à 2 }` : la teinte des gris |
| `surfaces` | oui | `{ "menu", "fond", "carte" }`, chacun facultatif |
| `surfacesClair` / `surfacesSombre` | non | mêmes clés, pour remplacer une surface le jour ou la nuit seulement |
| `shape` | oui | `{ "card": longueur, "control": longueur }` ; longueur = `0`, ou un nombre suivi de `px`, `rem` ou `em` (ex. `0.75rem`) |
| `encreMinimale` | non | 0 à 100 : force minimale du texte secondaire |
| `flat` | oui | `true` pour un rendu sans relief |
| `nuitSeulement` | non | `true` pour un thème fait pour la nuit seulement : l'application l'affiche en mode nuit quel que soit le choix de l'appareil (le bouton jour/nuit disparaît), et seule la nuit est mesurée |

Une surface s'écrit `{ "mode": "exact", "hex": "#rrggbb" }` (couleur exacte), ou
`{ "mode": "libre", "teinte": 0-360, "saturation": 0-100, "clarte": 0-100 }`, ou `{ "mode": "defaut" }`.
La nuit, une couleur exacte claire est assombrie automatiquement en gardant sa teinte.

**Lisibilité obligatoire :** sur chaque surface, de jour comme de nuit, le texte doit atteindre un
contraste de **4,5:1**. Les gris moyens (vers `#777`) échouent : choisis des surfaces nettement
claires ou nettement sombres.

## La feuille de style (`css`)

Chaque règle est automatiquement limitée au thème importé : écris tes sélecteurs normalement
(`.surface-carte { … }`), l'application ajoute elle-même le préfixe.

**C'est une liste d'autorisations** : toute propriété qui n'y figure pas fait refuser le fichier.
Un thème habille ; il ne peut ni cacher, ni déplacer, ni recouvrir un élément de l'application.

### Sur les éléments de l'application

| Propriétés | Limites |
|---|---|
| `color` | lisible : ni `transparent`, ni transparence de 30 % ou moins |
| `background`, `background-color`, `background-image`, `background-size`, `background-position`, `background-repeat`, `background-attachment`, `background-origin`, `background-blend-mode` | dégradés et `var(--image-<nom>)` ; aucune adresse |
| `border` et ses côtés, `border-color`, `border-style`, `border-width` | épaisseur 8 px au plus |
| `border-radius` et ses coins | 48 px au plus |
| `outline`, `outline-color`, `outline-style`, `outline-width`, `outline-offset` | 6 px au plus |
| `box-shadow` | décalages, flou et étalement de 32 px au plus |
| `text-shadow` | 16 px au plus |
| `font-family` | polices de la liste ci-dessus |
| `font-weight`, `font-style`, `font-variant`, `text-transform` | libres |
| `font-size` | de 10 à 64 px (0,625 à 4 rem) |
| `letter-spacing`, `word-spacing` | de −2 à 8 px, de −2 à 16 px |
| `line-height` | de 0,9 à 3 |
| `text-align` | `left`, `right`, `center`, `justify`, `start`, `end` |
| `text-decoration` et ses variantes, `text-underline-offset` | 6 à 8 px au plus |
| `padding` et ses côtés, `gap`, `row-gap`, `column-gap` | de 0 à 48 px ; pas de pourcentage |
| `opacity` | 0,5 au moins |
| `position` | `relative` ou `static` seulement |
| `animation` et ses variantes, `transition` et ses variantes | voir les animations |
| `cursor` | un mot-clé (`pointer`, `default`…) |
| `accent-color`, `caret-color`, `scroll-behavior`, `overscroll-behavior` | libres |

Aucune taille (`width`, `height`), aucun placement (`top`, `left`, `inset`, `margin`), aucun
`display`, `visibility`, `transform`, `filter`, `clip-path`, `overflow`, `z-index`,
`pointer-events`, `user-select` ni `all` sur les éléments de l'application.

### Sur les décorations (`::before` et `::after`)

En plus des propriétés ci-dessus : `content`, `display`, `visibility`, `opacity`, `position`
(`absolute` ou `fixed`), `inset`, `top`, `right`, `bottom`, `left`, `width`, `height`, `min-*`,
`max-*`, `margin`, `transform`, `transform-origin`, `z-index`.

- `content` : seulement `""`, `none`, `normal` ou `var(--image-<nom>)`. **Aucun texte.**
- Une décoration **dimensionnée ou placée** doit être hors du flux (`position: absolute` ou
  `fixed`) et **derrière** le contenu (`z-index: 0` ou moins, dans la même règle). Par exemple :

```css
.surface-carte { position: relative; }
.surface-carte::before {
  content: "";
  position: absolute;
  inset: 0;
  z-index: -1;
  background: linear-gradient(135deg, #eef2ff, #ffffff);
}
```

### Les animations

`@keyframes` n'anime que `color`, `background-color`, `background-position`, `border-color`,
`box-shadow`, `text-shadow`, `outline-color` et `opacity` (0,5 au moins). Rien ne bouge ni ne
disparaît.

### Toujours refusé

- `url(` sous toute forme, `image-set(`, `@import`, `@font-face`, `@namespace`, `@charset`,
  `@layer`, `@property`, `@page` et toute autre règle `@` hors `@media`, `@supports`, `@keyframes` ;
- les fonctions `attr()`, `env()`, `image()`, `cross-fade()`, `element()`, `paint()`, `src()`,
  `expression()` ;
- **toute barre oblique inverse** (aucun échappement) et le caractère `<`, y compris dans les
  requêtes média : écris `@media (max-width: 600px)`, pas `@media (width < 600px)` ;
- les sélecteurs qui nomment `:root`, `html`, `:host` ou `:has(` ;
- **les variables définies par la feuille** (`--ma-couleur: …`) : la feuille ne lit que celles de
  l'application (tableau suivant) ;
- **le texte** : les guillemets ne servent qu'à nommer des polices ;
- `calc()`, `min()`, `max()`, `clamp()` sur les éléments de l'application : écris les valeurs ;
- **une couleur de fond opaque sur le menu** (`.surface-menu`, `[data-app-sidebar]` et leurs décorations) :
  l'application déduit l'encre des libellés de la couleur du menu, qui peut aussi venir des réglages
  du magasin. Pars de `var(--surface-menu)` (par exemple
  `linear-gradient(180deg, color-mix(in srgb, var(--surface-menu) 88%, #ffffff), var(--surface-menu))`)
  ou garde des couleurs à 25 % d'opacité au plus ; une image `var(--image-<nom>)` reste permise ;
- des accolades qui ne s'équilibrent pas.

### Le jour et la nuit

La feuille ne peut pas savoir si l'écran est en mode jour ou nuit. Pour qu'une couleur suive
toute seule, **utilise les variables de l'application** plutôt que des couleurs fixes :

| Variable | Contenu |
|---|---|
| `var(--surface-menu)`, `var(--surface-fond)`, `var(--surface-carte)` | les trois surfaces, jour ou nuit |
| `var(--color-primary-50)` à `var(--color-primary-950)` | l'échelle de l'accent |
| `var(--color-gray-50)` à `var(--color-gray-950)` | l'échelle des gris |
| `var(--radius-card)`, `var(--radius-control)` | les rayons de `shape` |
| `var(--font-app)` | la police de `fontFamily` |

Une couleur fixe dans la feuille sera la même le jour et la nuit : réserve-la aux accents.

### Où accrocher tes règles

| Sélecteur | Élément |
|---|---|
| `[data-app-menubar]` | la barre du haut (ordinateur) |
| `[data-app-sidebar]` | le menu de gauche (ordinateur) |
| `[data-app-main]` | la page |
| `.surface-menu`, `.surface-fond`, `.surface-carte` | les faces peintes avec les trois surfaces |
| `[data-nav-title]`, `[data-nav-icon]`, `[data-nav-row-face]` | titres de section, pictogrammes et lignes du menu |
| `[aria-current="page"]` | la ligne du menu de la page ouverte |
| `[role="dialog"]` | les fenêtres |
| `[data-menubar-panel]` | le menu de l'avatar |
| `[data-status-color]` | les pastilles de statut (laisse-leur leur couleur : elle porte un sens) |
| `h1`, `table`, `thead th`, `tbody td`, `button`, `input`, `select` | éléments courants |

Les règles larges (`button { … }`) touchent toute l'application : reste sobre et vérifie que les
boutons colorés gardent leur sens (rouge = danger).

## Les images (`images`)

Un objet `{ "nom": "data:image/png;base64,…" }` :
- nom en minuscules, chiffres et tirets, 32 caractères au plus ;
- **PNG, JPEG, WebP ou GIF seulement**, encodé en base64 dans le fichier même ; **pas de SVG** ;
- le contenu doit être vraiment du type annoncé (il est vérifié octet par octet) ;
- 96 Ko au plus par image une fois décodée, 8 images au plus.

Dans la feuille, une image s'emploie par sa variable : `background-image: var(--image-<nom>);`.
Un petit motif répété (`background-size: 8px 8px`) coûte peu et rend bien.

## Ce que tu livres

1. Le fichier `<nom>.atlas-theme.json`, **accepté par le vérificateur** (colle sa ligne « OK »).
2. Trois lignes : l'intention du thème, ses couleurs principales, ce que fait sa feuille.
3. Si le thème s'inspire d'une œuvre : une ligne qui confirme que **toutes les images sont
   originales** et qu'aucun élément officiel n'a été repris.

L'administrateur du magasin l'installe dans **Réglages → Apparence → Thème importé → Importer un
fichier…**, voit un aperçu, puis l'installe. Il peut le retirer à tout moment.

Un thème signalé par un titulaire de droits est retiré du dépôt sans discussion.
