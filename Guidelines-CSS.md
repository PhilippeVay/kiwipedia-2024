# Guidelines : CSS

Statut : Recommendation (REC)

Cette présente convention rassemble les bonnes pratiques CSS (et SCSS) en production appliquées par l'agence web [Alsacreations.fr](https://www.alsacreations.fr/). Elle a pour but d'évoluer dans le temps et de s'adapter à chaque nouveau projet.

## Généralités

### Éditeur, Formatage et Qualité

- L'éditeur de code recommandé pour HTML, CSS, PHP, JavaScript est [Visual Studio Code](https://code.visualstudio.com/) (voir [Guidelines VS Code](Guidelines-VScode.md)).
- [EditorConfig](http://editorconfig.org/) et [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) imposent un formatage (UTF-8, espace vs tabs, guillemets) et des règles de syntaxe directement dans l'éditeur, ainsi ce dernier s'adapte à chaque projet.
- [Stylelint](https://marketplace.visualstudio.com/items?itemName=stylelint.vscode-stylelint) et [eslint](https://eslint.org/) sont des linters apportant une configuration qui vient s'ajouter aux outils natifs de Visual Studio Code.

### Configuration des extensions et linters

Tous les détails et configuration des Extensions et Linters sont décrits au sein des Guidelines selon la typologie de projet :

- [Guidelines Vue-Nuxt-Front-End](Guidelines-Vue-Nuxt-Front-End.md)
- [Guidelines Vue-WordPress](Guidelines-Vue-WordPress.md)
- [Guidelines WordPress](Guidelines-WordPress.md)

### Langage : Scss

Le pré-processeur [Sass](https://sass-lang.com/) (syntaxe `.scss`) apporte des fonctionnalités CSS indispensables&nbsp;: variables, notation imbriquée, mixins, etc. La méthode de compilation de Sass vers CSS dépend du type de projet (voir partie ["Méthodologies et conventions de nommage"](#méthodologies-et-conventions-de-nommage-css)).

### Compatibilité navigateurs

L'ensemble des recommandations de ce document est prévu pour être compatible avec tous les navigateurs représentant plus de 1.5% de la population, ce qui représente notamment tous les navigateurs modernes supportant [CSS Grid Layout](https://caniuse.com/css-grid). **Cela ne concerne donc pas Internet Explorer.**

La liste des navigateurs supportés est définie au sein du fichier [browserslist](https://github.com/browserslist/browserslist) que l'on retrouve dans `package.json` et dont la valeur est&nbsp;:

```json
"browserslist": [
    ">1.5%",
    "not op_mini all"
  ]
```

### Fichier CSS de base ("Reset")

Un "reset" CSS permettant d'harmoniser les styles par défaut des navigateurs est systématiquement appliqué en début de projet.

Normalize, Sanitize et Reboot sont des fichiers de base courants. **Nous privilégions les fichiers de base de KNACSS** qui est un récapitulatif de ces ressources.

Documentation : [fichiers reset de KNACSS](https://github.com/alsacreations/KNACSS/tree/master/sass/base)

## Sélection des éléments

La maintenabilité des feuilles de styles est une priorité. Il est nécessaire de prioriser les sélecteurs ayant le moins de spécificité (poids) possible afin de faciliter les modifications ultérieures ou dans des contextes différents (Responsive).

Privilégier au maximum l'usage de [**classes**](http://www.drinchev.com/blog/css-with-only-class-names/) plutôt que d'écrire des sélecteurs basés sur le type des éléments ou leur `id`.

Le sélecteur CSS doit être _unique_ si cela est possible (une seule classe). Éviter les _sélecteurs composés_ tels que `.modal span` ou `.modal .date` mais plutôt `.modal-date` pour conserver un poids minimal.

Cela implique que chaque élément HTML, même anodin tels que des `<span>`, de `<p>` ou des `<a>` disposent d'attributs de classe afin de pouvoir être ciblés sans avoir à faire à leur hiérarchie.

## Notation imbriquée Scss

La [Notation imbriquée](https://sass-lang.com/guide#topic-3) (nesting) de Sass offre une vision sur la "hiérarchie" du composant et facilite la lecture du code.

Les inconvénients majeurs de cette notation sont :

- Qu'elle génère des sélecteurs CSS composés (donc avec un poids qui augmente).
- Qu'elle impose une structure au sélecteur. L'élément n'est ciblé que s'il est descendant d'un autre élément. On ne peut plus réutiliser l'élément ailleurs, au sein d'une autre structure.

**Il est conseillé d'éviter les sélecteurs imbriqués, ou au pire de limiter la syntaxe à un seul niveau d'imbrication.**

**À éviter** (car génère des sélecteurs composés de 3 niveaux `.home .home-first .home-spotlights { … }`) :

```scss
.home {
  .home-first {
    .home-spotlights {
    }
  }
}
```

**À éviter** (génère un sélecteur simple `.home-first-spotlights { … }`, mais rend difficile la lecture du code et la recherche dans les fichiers) :

```scss
.home {
  &-first {
    &-spotlights {
    }
  }
}
```

**À conseiller si vraiment nécessaire** (un seul niveau d'imbrication génère des sélecteurs composés de 2 niveaux au maximum `.home .home-first { … }`) :

```scss
.home {
  .home-first {
  }
  .home-spotlights {
  }
}
```

**Exception : le sélecteur de parent `&` est parfaitement préconisé dans le cas d'événements tels que `&:hover`, `&:focus` ou `&:active`.**

## Commentaires

Les commentaires CSS et l'usage de mots-clés informatifs au sein de commentaires importants sont appréciés, sous la forme :

- `TODO:` → point à finir / corriger avant de livrer

Cette syntaxe est préconisée car correspond au réglage par défaut de l'extension VSCode [TODO Highlight](https://marketplace.visualstudio.com/items?itemName=wayou.vscode-todo-highlight).

D'autres mots-clés peuvent être additionels selon les cas (ne pas en abuser) :

- `TODO:RG` → tâche à réaliser par Raphaël
- `TODO:URL` → URL manquante

## Ordre des déclarations

Les déclarations au sein d'une règle CSS sont ordonnées de façon à faire apparaître les propriétés importantes en tête de liste.

Voici dans quel ordre nous déclarons nos propriétés :

1. Propriété display : tout ce qui affecte le rendu par défaut de l’élément
2. Positionnement : tout ce qui détermine la position de l’élément
3. Modèle de boîte : tout ce qui influe sur les dimensions de l’élément
4. Transformations et transitions
5. Typographie : tout ce qui détermine les caractéristiques de la police de caractères
6. Décoration : les propriétés purement ornementales

Exemple :

```css
selecteur {
  display: inline-block;
  position: relative;
  top: -1em;
  z-index: 1337;
  max-width: 50%;
  margin: 1em;
  padding: 0;
  overflow: hidden;
  text-align: right;
  font: bold 1.5em/1.3 arial, verdana, sans-serif;
  background: rgba(0, 0, 0, 0.5);
}
```

**_Note : La démarche de réordonnement est généralement manuelle, en se servant de cette liste comme référence. Si un projet le nécessite, l'outil "Stylelint" intégré au Workflow sous forme de tâche Gulp permet de réordonner automatiquement les déclarations CSS grâce au plugin [stylelint-order](https://www.npmjs.com/package/stylelint-order)_**

## Méthodologies et Conventions de nommage CSS

### Workflow = "ça dépend"

Notre Workflow varie selon les types de projets, la maturité des navigateurs web et l'obsolescence des technologies.

Notre rôle est de nous adapter constamment à ces évolutions, et nos solutions techniques sont à ce jour&nbsp;: NPM, Vue, React, Webpaxmix, WordPress, Gulp, site statique ("pas de workflow").

Les [méthodologies et conventions de nommage CSS](https://speakerdeck.com/goetter/conventions-de-nommage-en-css) que nous préconisons selon les projets sont&nbsp;:

- Tailwind
- ou KNACSS
- ou Bootstrap

### Tailwind

_Source : <https://tailwindcss.com/>_

TailwindCSS correspond à une approche "atomique" de CSS, comprendre qu'à chaque classe correspond une action et une seule.

La méthodologie et les conventions de Tailwind sont très spécifiques car toutes les classes sont déjà existantes dans les fichiers CSS, il suffit donc de les appliquer au sein des fichiers HTML. Il n'est nécessaire d'écrire des styles que sporadiquement au cours du projet.

**Ce framework CSS "Utility First" est notre choix prioritaire et préconisé dans la plupart des projets (WordPress, PHP, VueJS, Webpackmix) (pour la configuration voir [_Guidelines Tailwind_](Guidelines-Tailwind.md))**

Notre workflow Tailwind comporte :

- Un mini fichier CSS "Alsa-TW-reset" dédié (voir [_Guidelines Tailwind_](Guidelines-Tailwind.md)
- Une [Convention de nommage "Utility first"](Guidelines-Tailwind.md)
- PurgeCSS (tâche permettant de supprimer les styles inutilisés)

### KNACSS

_Source : <https://www.knacss.com/doc.html>_

KNACSS est un micro-framework CSS constitué au fur et à mesure au sein d'Alsacréations pour répondre à nos besoins de conventions et de bonnes pratiques, notamment en terme d'Accessibilité.

**Ce micro-framework CSS sera choisi principalement pour des sites statiques (intégration simple), voire sans Workflow particuliers.**

Notre workflow KNACSS comporte :

- Fichiers "reset" KNACSS avec un condensé de bonnes pratiques
- Une compatibilité Sass
- Une Architecture et imports des fichiers KNACSS
- Un Mixin `respond-to`
- Une Convention de nommage : "sémantique" (se rapproche de [BEM](http://getbem.com/))

### Bootstrap

_Source : <https://getbootstrap.com/>_

**Ce framework HTML/CSS sera choisi lorsque le client est déjà familier avec ce Framework ou nous l'impose.**

Notre workflow Bootstrap comporte :

- Fichiers "reset" KNACSS avec un condensé de bonnes pratiques
- Une compatibilité Sass
- Convention de nommage : "sémantique" (se rapproche de BEM)
- Composants préfabriqués (modales, navigation, etc.)
- Grille de mise en forme --> préférer l'usage de Grid Layout plutôt que la grille de Bootstrap nécessitant des imbrications HTML

## Divers

- L'outil [Autoprefixer](https://autoprefixer.github.io/) est employé lorsque des préfixes navigateurs sont nécessaires pour un projet (dépend du Workflow / Framework).
- Les "hacks" ou détournements de propriétés ou valeurs sont vivement déconseillés. En dernier ressort, employer la ressource [Browserhacks](http://browserhacks.com/)
- La règle `!important` doit être éradiquée si possible du fait de son poids extrêmement important (certaines parties des styles peuvent toutefois exceptionnellement employer à juste titre `!important`).
- Opter pour des tailles de polices fluides (de préférence en `rem`), éviter les tailles de police de taille fixe (`px` ou `pt`) car inaccessible aux personnes nécessitant d’agrandir les contenus textuels.
- Opter pour le modèle de boîte CSS3 (`box-sizing: border-box`) en début de la feuille de style. Cela permet de connaître la taille de tous les éléments sans avoir à faire le calcul de `taille+padding+border`.
- Éviter d’écraser une règle par une autre.
- Durant la phase de développement l'intégration se fait sur plusieurs fichiers CSS (composants, layout, etc.) que l'on rassemble (`@import`) dans un fichier unique.
- Les fichiers CSS doivent être minifiés pour économiser du poids de chargement.
- Toujours préciser quelle(s) propriété(s) doit être animée dans transition ou animation
- Éviter d’animer des propriétés autres que **transform** ou **opacity** ou **filter** (ou alors ajouter la propriété `will-change` et/ou le hack de `translateZ()`.) Source : [https://tzi.github.io/presentation-CSS-perfs/](https://tzi.github.io/presentation-CSS-perfs/)
- `@font-face` : Privilégiez `.woff2`. Pour le détail, voir la partie ["medias / polices"](#polices)

## Guidelines Sass

### Variables

Utiliser systématiquement les variables Sass (`$variable`) pour éviter les répétitions de code et favoriser la maintenance du projet.

Cette consigne concerne principalement :

- les couleurs de texte
- les couleurs de fond
- les tailles de police
- les breakpoints des Media Queries en Responsive
- les margin et les padding

**Aucune de ces valeurs ne devraient apparaître dans les styles de développement sans être associées à des variables.**

### Breakpoints et Media Queries

La liste de points de rupture (breakpoints) recommandée est proposée sous forme de couple "clé:valeur" et peut bien entendu être élargie&nbsp;:

```scss
$breakpoints : (
  sm: 576px,
  md: 992px,
  lg: 1400px
);
```

Conformément à la démarche "Mobile First", ces valeurs correspondent à des jalons minimum : `sm: '576px'` correspond au Media Query `@media (min-width: 576px) {...}`.

Pour éviter les intervalles qui se chevauchent, ou des Media Queries trop variés, nous préconisons d'appliquer un [mixin Sass "respond-to"](https://github.com/alsacreations/KNACSS/blob/master/sass/abstracts/_mixins-sass.scss) pour appliquer des styles Responsive&nbsp;:

```scss
.modal {
  background: $hotpink;
  @include respond-to('medium-up') {
    background: $tomato;
  }
}
```

Les valeurs prévues dans notre mixin sont (privilégier les premières, respectueuses de la méthodologie "Mobile First") :

- `'small-up'` : correspond à `(min-width: $small)`
- `'medium-up'` : correspond à `(min-width: $medium)`
- `'large-up'` : correspond à `(min-width: $large)`
- `'extra-large-up'` : correspond à `(min-width: $extra-large)`
- `'small'` : correspond à `(max-width: $small - 1)`
- `'medium'` : correspond à `(max-width: $medium - 1)`
- `'large'` : correspond à `(max-width: $large - 1)`
- `'extra-large'` : correspond à `(max-width: $extra-large - 1)`

## Media print (impression)

La feuille de styles dédiée à l'impression aide aussi à l'export PDF dans le navigateur. La plupart du temps il s'agira en priorité de masquer les éléments inutiles dans un document statique ou papier (ex : navigation) et de retirer les décorations superflues.

- [Une feuille de styles de base pour le media print](https://www.alsacreations.com/astuce/lire/1160-Une-feuille-de-styles-de-base-pour-le-media-print.html)
- [Faire une feuille de style CSS print pour l'impression](https://www.alsacreations.com/tuto/lire/586-feuille-style-css-print-impression.html)

## Médias (images, polices)

### Images

#### Recommandations générales

Les images de contenu doivent être véhiculées via l'élément `<img>` ou `<picture>` et demeurer fluides quel que soit le périphérique où elles s'affichent.

Toujours transmettre le texte alternatif `alt` (voir [Guidelines Accessibilité](Guidelines-Accessibilite.md)).

Toujours indiquer les dimensions initiales de l'image (`width` et `height`) (voir [Guidelines Performances](Guidelines-Performances.md.md))

#### Code recommandé

```html
<!-- Dimensions initiales de l'image -->
<img src="(chemin)" alt="" width="2000" height="1000">
```

```css
img {
  max-width: 100%; /* largeur fluide */
  height: auto; /* ratio préservé */
  background: gray; /* placeholder en attendant */
}
```

### Polices

Autant que possible, privilégier le chargement de polices légères et respectueuses des performances, indiquées notamment sur [Google Web Fonts](http://www.google.com/webfonts/v2). Limiter le nombre de ces polices à 2, voire 3 grand maximum.

#### Recommandations générales

- Privilégier la police système `system-ui` pour les textes de contenus (raison : performance + UX + Layout Shifts)
- Privilégier le format `.woff2` (et `.woff` en alternative)
- Limiter à 2 ou 3 fichiers de police au maximum (regular, bold, italic), sinon préférer une [Variable Font](https://v-fonts.com/)
- Utiliser la directive `<link rel="preload">` pour charger les polices de manière asynchrone.
- Ajouter `font-display: swap;` au sein de la règle `@font-face` pour éviter les effets de FOIT. Si la police est pré-chargée, `font-display: optional;` est alors recommandé.
- Héberger la police sur son propre serveur (voir l'outil "Google Webfont Helper")

#### Code recommandé

Voici un exemple de chargement de police conseillé (cas de deux fichiers de police regular et bold)&nbsp;:

```html
<!-- Dans le <head> après
     la feuille de styles pour ne pas la bloquer -->
<link rel="preload" as="font" href="kiwi.woff2" 
      type="font/woff2" crossorigin="anonymous">
```

```css
@font-­face {
  font-­family: 'kiwi';
  src: url('kiwi.woff2') format('woff2'), 
    url('kiwi.woff') format('woff');
  font-weight: normal;
  font-style: normal;
  font-display: optional;
}
@font-­face {
  font-­family: 'kiwi';
  src: url('kiwi-bold.woff2') format('woff2'), 
    url('kiwi-bold.woff') format('woff');
  font-weight: bold;
  font-style: normal;
  font-display: optional;
}
```

#### Ressource : Google Webfont Helper

L'application web [Google Webfont Helper](https://google-webfonts-helper.herokuapp.com/) est une excellente ressource permettant d'optimiser finement les fichiers :

- choisir le bon subset (latin, latin-ext, etc.)
- choisir les styles nécessaires au projet (normal, bold, italic, etc.)
- copier le code CSS prévu pour les navigateurs modernes (woff2 et woff)
- télécharger les fontes de Google Fonts aux formats woff2 et woff

Il est conseillé de récupérer les fontes via cette ressource si cela est possible.

### Contenus de remplissage

Le remplissage par du contenu temporaire peut faire appel à _Lorem Ipsum_.

- Pour le texte :
  - [https://schnaps.it/](https://schnaps.it/)
  - [https://loripsum.net/](https://loripsum.net/)
- Pour les images :
  - Voir <https://github.com/alsacreations/guidelines/blob/master/Ressources-liens.md#placeholders-images-de-remplissage>
