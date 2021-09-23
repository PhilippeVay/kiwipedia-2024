# Conventions Générales de Nommage

_Statut : Working Draft (WD)_

Ces présentes conventions ont pour objectif d'harmoniser les noms des fichiers, des fonctions ou classes utilisées au sein des projets web de l'agence [Alsacréations](https://www.alsacreations.fr/).

## Convention de Langue

La langue employée pour tout texte rédigé au cours d’un projet est le Français.

Cela concerne :

- les commentaires dans un fichier de code,
- les titres de commit (_versionning_),
- les instructions dans le fichier `readme.md`,
- toute documentation explicative ou technique.

La langue Anglaise demeure préconisée pour :

- L’architecture et les dossiers du projet (assets, layout, components, fonts)
- Le nom des fichiers (`single-something.html`)
- Les branches principales de versionning (`master`, `develop`), avec possibilités en français si besoin (`recette`)

## Convention de Formatage

La règle d’indentation appliquée par défaut est de **“2 espaces”** pour l’ensemble des langages. Les conventions spécifiques à certains langages ou technologies (PHP, WordPress) sont prioritaires sur cette règle générale au cas par cas.

Par exemple :

- PHP suit la convention de styles [PSR-12 "Extended Coding Style"](https://www.php-fig.org/psr/psr-12/) qui stipule _“Code MUST use an indent of 4 spaces for each indent level, and MUST NOT use tabs for indenting.”_
- WordPress suit la convention [PSR-5 "PHPDoc Standard"](https://www.php-fig.org/psr/)
- Les Documentations techniques se réfèrent à PHPdoc, JSdoc, etc.

On débute par `/**` dans VSCode qui auto-complète en optant pour un formatage conventionnel.

JSDoc est [supporté nativement par VSCode](https://code.visualstudio.com/docs/languages/javascript#_jsdoc-support), mais peut être facilité par une [extension JSDoc](https://marketplace.visualstudio.com/items?itemName=stevencl.addDocComments).

Exemple :

```js
/**
 * Represents a book.
 * @constructor
 * @param {string} title - The title of the book.
 * @param {string} author - The author of the book.
 */
function Book(title, author) {}
```

Toujours configurer et appliquer les Linters et Formatters **Editorconfig**, **ESlint** et **Stylelint** (voir détails dans les [Guidelines VSCode](Guidelines-VScode.md))

## Convention d'Union de mots

Les conventions d’usage pour lier les mots sont :

- under_score :
  - Fonctions PHP
- kebab-case :
  - fichiers pouvant se retrouver dans les URLs (ex : `single-something.html`)
  - classes HTML/CSS
- PascalCase :
  - noms de composants dans Vue/Nuxt (ex : `ModalAccountMixins.js`)
  - classes PHP
- camelCase :
  - variables et fonctions dans JavaScript (ex : `dateFormat`, `getResellSwitchQty()`)
  - méthodes PHP
- ALL_CAPS (SCREAMING_SNAKE_CASE) :
  - constantes
- snake_case :
  - nope

## Convention de nommage pour code en attente

En phase de développement d'un projet, les notes de modifications et améliorations restant à réaliser dans le code (CSS, HTML, JavaScript) doivent être consignées et notées `TODO:` (et non ~~@TODO~~) ou `FIXME:` selon leur fonction&nbsp;:

- `TODO:` Partie de code **non finalisée**, non mis en oeuvre (par exemple `TODO: implémenter les données`, `<a href="TODO:">`)
- `FIXME:` Partie de code **à améliorer**, à modifier pour être plus performant, plus maintenable, etc. (par exemple : `FIXME: mieux gérer le responsive`, `FIXME: refactoring`)

L'extension VSCode **[TODO Highlight](https://marketplace.visualstudio.com/items?itemName=wayou.vscode-todo-highlight)** permet de mettre en exergue les tags `TODO:` (en jaune par défaut) et `FIXME:` (en rose par défaut).

TODO Highlight propose également de lister l'ensemble des tags par fichier via `ctrl/cmd + maj + p` > `List highlighted annotations` (liste les tags dans le document en cours).

Si nécessaire, il est possible d'indiquer quels types de fichiers sont à surveiller par TODO Highlight au sein de `settings.json` (par exemple, les fichiers `.vue` et `.md` ne sont pas surveillés par défaut).

```json
"todohighlight.include": [
    "**/*.js",
    "**/*.jsx",
    "**/*.ts",
    "**/*.tsx",
    "**/*.html",
    "**/*.vue",
    "**/*.php",
    "**/*.md",
    "**/*.css",
    "**/*.scss"
  ]
```

**En fin de phase de développement, avant la livraison du projet, il est fondamental de vérifier la présence indésirable de ces tags au sein du code. L'idéal étant qu'un projet soit livré au client avec zéro tag `TODO:`.**

## Convention pour Langages spécifiques et Frameworks

Les règles de nommage particulières à chaque langage sont consignées dans leurs Guidelines respectives :

- [Guidelines HTML](Guidelines-HTML.md)
- [Guidelines CSS](Guidelines-CSS.md)
- [Guidelines Développement PHP](Guidelines-Developpement-PHP.md)
- [Guidelines JavaScript](Guidelines-JavaScript.md)
- [Guidelines WordPress](Guidelines-WordPress.md)
- [Guidelines Accessibilité](Guidelines-Accessibilite.md)
