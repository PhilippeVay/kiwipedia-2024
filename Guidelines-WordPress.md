# Guidelines : WordPress

_Statut : Recommendation (REC)_

Cette présente convention rassemble les bonnes pratiques WordPress en production appliquées par l'agence web [Alsacreations.fr](https://www.alsacreations.fr/). Elle a pour but d'évoluer dans le temps et de s'adapter à chaque nouveau projet.

## Structure de projet

On utilise

- [Composer](https://getcomposer.org/) pour installer WordPress et ses extensions.
- [WordPlate](https://github.com/wordplate/wordplate) qui fonctionne avec [webpackmix](https://github.com/devanandb/webpack-mix/tree/master/docs).
- [Tailwind](https://github.com/timber/timber) en tant que framework CSS.
- [Timber](https://github.com/timber/timber) pour utiliser Twig dans les templates (facultatif).

## Environnement de développement

👉 On utilise [Docker](https://www.docker.com/)

- Par défaut, on part d’une base MySQL locale. Il est possible d’utiliser une base MySQL “partagée” accessible à distance, attention cependant à la synchronisation d’informations et de fichiers (options, réglages de plugins activés…).
- Utiliser `define('WP_ENVIRONMENT_TYPE','staging');` puis [wp_get_environment_type()](https://make.wordpress.org/core/2020/07/24/new-wp_get_environment_type-function-in-wordpress-5-5/)
- Utiliser `define('WP_DEBUG',true);` pour activer le mode debug

## Git

On versionne les fichiers :

- .env.example
- composer.lock
- package.json
- le thème développé pour le projet
- les extensions développées pour le projet
- les fichiers de configuration
- les fichiers de traduction du thème (dossier /languages) ou de l’extension (dossier de l’extension)

On ne versionne **pas** :

- .env (sauf exception)
- WordPress lui-même (car installé/mis à jour par composer)
- les extensions tierces (car installé/mis à jour par composer)
- les uploads

👉 Le fichier README.md à la racine du projet doit contenir toutes les informations pour ré-installer le site rapidement en production.

## Configuration de base

### Sécurité, utilisateurs

- 👉 Supprimer l’utilisateur admin et l’utilisateur avec l’ID 1. Créer un utilisateur de niveau administrateur avec identifiant spécifique différent de “admin”.
- Créer un ou plusieurs utilisateurs de niveau éditeur pour les intervenants (doit être différent du nom de domaine pour des raisons de sécurité), ayant accès juste aux fonctionnalités utiles.
- Ajouter le script pour enlever le warning à la connexion qui permet d’indiquer que l’identifiant est le bon mais pas le mot de passe.

## Thème

- 👉 On privilégie de démarrer avec un starter thème épuré <https://underscores.me/> ou <https://github.com/timber/starter-theme> lorsque l’on utilise Timber.
- 👉 Supprimer les autres thèmes livrés par défaut.
- Il est plus rapide de développer le thème dans WordPress plutôt que de passer par une phase d’intégration statique.
- On évite d’utiliser un thème acheté car cela sous-entend qu’on ne pourra pas tout mettre en place dans ces guidelines et qu’on ne maîtrise pas son contenu (code, extensions, évolutions). Si toutefois cela arrive, utiliser le principe de thème enfant pour ne pas modifier le thème parent, qui pourrait être mis à jour par la suite.
- Modifier le logo sur le formulaire de connexion admin (voir snippets).

### Intégration du thème

#### Outils de vérification (linters)

La liste des linters recommandés est décrite par les [guidelines Visual Studio Code](https://github.com/alsacreations/guidelines/blob/master/Guidelines-VScode.md).
Les extensions spécifiques WordPress / PHP recommandées sont :  

- [PHP Intelephense](https://marketplace.visualstudio.com/items?itemName=bmewburn.vscode-intelephense-client)
- [Visual Studio Code supporte PHP](https://code.visualstudio.com/docs/languages/php) (Linting, Debug…) : le configurer en indiquant le chemin.

#### Automatisation

Avec [webpackmix](https://github.com/devanandb/webpack-mix/tree/master/docs) (présent dans WordPlate)

#### Moteur de template

[Timber](https://www.alsacreations.com/tuto/lire/1813-Timber-pourquoi-ecrire-du-Twig-dans-WordPress-.html) (présent dans notre structure-type avec Docker)

#### Framework CSS

On privilégie, dans cet ordre, les frameworks CSS suivants :

- [TailwindCSS](https://tailwindcss.com/) (couplé à un mini-fichier reset personnel “alsa-TW-Reset”) (pour la configuration voir [Guidelines Tailwind](https://github.com/alsacreations/guidelines/blob/master/Guidelines-Tailwind.md))
  - <https://www.paulund.co.uk/using-tailwind-css-in-your-wordpress-theme>
  - <https://github.com/cjkoepke/wp-tailwind>
- [Bootstrap](https://getbootstrap.com/) (si besoin spécifique ou projet le nécessitant)
- [KNACSS](https://www.knacss.com/) (si besoin spécifique) (voir [Guidelines CSS](Guidelines-CSS.md))

#### Nommage HTML, CSS et PHP

Voir [Guidelines HTML et CSS](https://github.com/alsacreations/guidelines)

- Ne pas utiliser les classes CSS générées par WordPress qui sont spécifiques à une installation précise et ne sont pas réutilisables.
- Les classes spécifiques des wrapper des menus du type `.menu-nom-de-mon-menu`
- La majorité des classse générées par `body_class()` ou `post_class()`
- Pour le chargement de fichiers CSS et JavaScript on utilise les fonctions [wp_enqueue_script](https://developer.wordpress.org/reference/functions/wp_enqueue_script/) et [wp_enqueue_style](https://developer.wordpress.org/reference/functions/wp_enqueue_style/).
- Placer `add_action()` et `add_filter()` après la fonction liée.
- Utiliser la dernière notation pour les tableaux PHP `$array = []; et non $array = array();`.
- Toutes les chaînes de caractères d’un thème doivent pouvoir être traduites. Il faut donc les entourer dans la bonne fonction gettext ( `__()`, `_n()`, `_x()` ), couplées à un text-domain cohérent en fonction du contexte (thème, thème enfant, extension, ...)
- Découper le thème de manière cohérente (boucles à part, etc.) pour pouvoir utiliser `get_template_part()` correctement

### Hiérarchie de fichiers et documentation

👉 Utiliser l’auto-chargement des fichiers PHP du thème par WordPress (selon slug de la catégorie, du Custom Post Type, etc).

- Connaître / visualiser la hiérarchie de templates : <https://wphierarchy.com/> <https://developer.wordpress.org/themes/basics/template-hierarchy/>
- Documentation officielle <https://developer.wordpress.org/themes/>
- Fonctions <https://codex.wordpress.org/Function_Reference>
- Hooks <https://adambrown.info/p/wp_hooks>
- WP_Query <https://www.smashingmagazine.com/2013/01/using-wp_query-wordpress/>
- WP_Query, query_posts, get_posts, etc <https://www.rarst.net/wordpress/wordpress-query-functions/>
- Custom Post Type <https://developer.wordpress.org/reference/functions/register_post_type/>
- Ou générateur de Custom Post Type <https://generatewp.com/post-type/> (à noter avec Gutenberg: il faut obligatoirement renseigner le champ "parent_item_colon" pour voir apparaître le sélecteur de pages parentes pour un CPT hiérarchique).
- Taxonomies <https://developer.wordpress.org/reference/functions/register_taxonomy/>

Voir aussi

- [Vie d’une requête](https://roots.io/routing-wp-requests/)
- [Cheatsheet template map](https://cdn.tutsplus.com/wp/uploads/legacy/090_WPCheatSheets/WP_CheatSheet_TemplateMap.pdf)
- [Cheatsheet loop visual model](https://cdn.tutsplus.com/wp/uploads/legacy/090_WPCheatSheets/WP_CheatSheet_LoopVisualModel.pdf)
- [A Detailed Guide To A Custom WordPress Page Templates](https://www.smashingmagazine.com/2015/06/wordpress-custom-page-templates/)

### À prévoir dans le thème

👉On ne nomme/préfixe pas le thème ou ses classes/fonctions par alsa_ mais plutôt par le nom du projet.

La [structure standard](https://developer.wordpress.org/themes/basics/organizing-theme-files/) est :

```text
assets (dir)
      - css (dir)
      - images (dir)
      - js (dir)
inc (dir)
template-parts (dir)
      - footer (dir)
      - header (dir)
      - navigation (dir)
      - page (dir)
      - post (dir)
404.php
archive.php
comments.php
footer.php
front-page.php
functions.php
header.php
index.php
page.php
README.txt
rtl.css
screenshot.png
search.php
searchform.php
sidebar.php
single.php
style.css
```

[Theme Check](https://wordpress.org/plugins/theme-check/) permet de vérifier si le thème correspond aux standards (ne fonctionne pas avec Timber).

### Traductions

Voir <https://www.alsacreations.com/article/lire/1837-wordpress-theme-internationalisation.html>

### functions.php

⚠️ Le fichier functions.php fonctionne différemment des autres fichiers “template”, lors de la création d’un thème enfant par exemple, il n’est pas simplement écrasé, mais chargé avant le thème parent. Les deux fichiers déclarant des fonctions cohabitent, et il serait dommage de ne pas pouvoir écraser une fonctionnalité, ou de tomber sur une erreur PHP car une fonction est déclarée deux fois.
Il faut donc prendre l’habitude de déclarer TOUTES les fonctions ainsi :

```php
if ( ! function_exists( 'nomdutheme_nom_de_la_fonction' )  {
    function nomdutheme_nom_de_la_fonction() {
        // do something
    }
}
add_filter('filter_name', 'nomdutheme_nom_de_la_fonction');
```

👉 Idéalement le fichier functions.php du thème inclut d’autres scripts PHP dédiés pour organiser le code :

- actions.php
- filters.php
- menu.php
- theme-setup.php
- etc.

Exemple de fichier functions.php

```php
/**
 * Menus/Sidebar/Theme options definitions
 */
require_once 'includes/theme-setup.php';
 
/**
 * Filters used to alter front-end rendering
 */
require_once 'includes/menu-filters.php';
 
/**
 * Actions & filters
 */
require_once 'includes/actions.php';
require_once 'includes/filters.php';
 
/**
 * Parent theme overload
 */
require_once 'includes/inc-pages-functions-updated.php';
require_once 'includes/cnrs-functions.php';
```

### Shortcodes

Lors de la création d’un [shortcode](https://codex.wordpress.org/fr:Shortcode) avec paramètres, il est conseillé de ne plus utiliser la fonction extract (voir <https://core.trac.wordpress.org/ticket/22400>). Tout shortcode ajouté doit faire l’objet d’un guide écrit pour l’utilisateur final.

Voir <https://capitainewp.io/formations/developper-theme-wordpress/shortcode/> et <https://kinsta.com/fr/blog/shortcodes-wordpress/>

### Gutenberg / éditeur wysiwyg

- Palette de couleurs <https://speckyboy.com/custom-color-palette-wordpress-gutenberg-editor/>

### Formulaires

- Valider les données avec les méthodes natives <https://codex.wordpress.org/Data_Validation>
- Un formulaire = un nonce <https://codex.wordpress.org/WordPress_Nonces>

## Extensions

👉 Installation : utiliser composer avec le nom du plugin, préfixé par “wpackagist-plugin”, par exemple `composer require wpackagist-plugin/wp-migrate-db`

👉 Toute fonctionnalité développée sur-mesure pour le projet se fait dans le cadre d’une extension propre à activer/désactiver.

- Documentation officielle <https://developer.wordpress.org/plugins/>

Modèles d’extension à utiliser

- [WordPress Plugin Template](https://github.com/hlashbrooke/WordPress-Plugin-Template)
- [WordPress Plugin Boilerplate Generator](https://wppb.me/)
- [WordPress Plugin Boilerplate](https://wppb.io/)

### Obligatoires

- [wp-fail2ban](https://wordpress.org/plugins/wp-fail2ban/) si hébergement interne : permet de signaler les erreurs d’identification à fail2ban+iptables pour bannir les IP tentant du bruteforce ; n’utilisez alors pas d’extension pour changer l’url de wp-admin.
- [WP Migrate DB](https://fr.wordpress.org/plugins/wp-migrate-db/) pour migrer les données de local > dev > recette > prod (et inversement), à désinstaller par sécurité après mise en production.

### Recommandées selon usage

- [Disable comments](http://wordpress.org/extend/plugins/disable-comments/) : désactiver les commentaires sur les articles/pages/médias, au choix (très propre).
- [ACF](https://www.advancedcustomfields.com/) : ajouter des champs riches aux posts / pages / Custom posts.
- [Duplicate Post](https://wordpress.org/plugins/duplicate-post/) : créer du contenu rapidement en dupliquant d'un simple clic un article, une page, ou un custom post.
- [W3-total-cache](https://wordpress.org/plugins/w3-total-cache/) : cache de contenu pour améliorer les temps de réponse.
- [Ninja Forms](https://fr.wordpress.org/plugins/ninja-forms/) : génération de formulaires. Partiellement accessible.
- [Polylang](https://fr.wordpress.org/plugins/polylang/) : traduction (remplace WPML).
- [SEOPress](https://www.seopress.org/fr/) : SEO, ou [Yoast](https://fr.wordpress.org/plugins/wordpress-seo/) (rajoute une grosse surcouche de pub très intrusive dans l'admin).
- [Filebird](https://wordpress.org/plugins/filebird/) : File Manager (s’ajoute dans la galerie de médias) : créer des dossiers. Attention, il faut prendre la version premium pour créer des dossiers illimités.
- [Photo gallery](https://fr.wordpress.org/plugins/photo-gallery/) (Galerie de médias, photos et vidéos) + riche en fonctionnalités que la galerie native (img s’ouvrent dans une popup, slider, bouton de téléchargement, création de groupes de galeries, etc…). N’est pas accessible : fenêtre modale qui ne prend pas le focus, pas d’attributs aria, bouton de fermeture non accessible.
- [Job Manager](https://fr.wordpress.org/plugins/wp-job-manager/) : Offres d’emploi.
- [Members](https://wordpress.org/plugins/members/) : Droits et utilisateurs.
- [Megamenu](https://fr.wordpress.org/plugins/wp-megamenu/) : Menu de navigation.
- [Tarteaucitron](https://fr.wordpress.org/plugins/tarteaucitronjs/) || [Cookie Notice](https://fr.wordpress.org/plugins/cookie-notice/) : bannières cookies, code non accessible (boutons qui n’en sont pas, etc.).
- [Adminimize](https://wordpress.org/plugins/adminimize/) : personnaliser l’aspect de l’admin en fonction des niveaux des utilisateurs. || [Hook natif](https://developer.wordpress.org/reference/functions/remove_menu_page/) : supprimer les items du menu (pour un rôle spécifique, vérifier le rôle avec fonction [current_user_can](https://developer.wordpress.org/reference/functions/current_user_can/)).
- [Peters-login-redirect](https://wordpress.org/plugins/peters-login-redirect/) : redirection des utilisateurs après connexion. || [Hook natif](https://developer.wordpress.org/reference/hooks/login_redirect/)
- [Relevanssi](http://wordpress.org/extend/plugins/relevanssi/) : améliore les résultats de recherche par critères de pertinence.
- [Members List](https://wordpress.org/plugins/members-list/) : listes de membres à afficher, markup entièrement personnalisable pour chaque liste et chaque champ (pouvant être custom). Possibilité de définir sur quels champs les recherches sont effectuées, et lesquels ont une fonction "sorting" activée.
- [User Switching](https://wordpress.org/plugins/user-switching/) : switcher facilement d’utilisateur.
- [Simple Page Ordering](https://wordpress.org/plugins/simple-page-ordering/) : ordonner les pages, et autres CPT ordonnés, par simple glisser/déposer, sans avoir besoin de rentrer dans chaque page.
- [Multiple Domain Mapping on Single Site](https://fr.wordpress.org/plugins/multiple-domain-mapping-on-single-site/) pour faire correspondre différentes Pages (d’accueil) à plusieurs domaines ou sous-domaines.
- [Custom Login](https://wordpress.org/plugins/custom-login/) : personnaliser la page de login. || [Tuto avec Hooks natifs](https://codex.wordpress.org/Customizing_the_Login_Form)
- [WP All Export](https://wordpress.org/plugins/wp-all-export/) : exporter les données au format CSV/XML (fonctionne avec ACF, The Events Calendar) fonctionne aussi pour l’import avec [WP All Import](https://wordpress.org/plugins/wp-all-import/)

### E-commerce

- [WooCommerce](https://woocommerce.com/) est le plugin le plus actif (communauté, support) à l’heure actuelle. Il propose des feuilles de style par défaut, un système de coupon, gestion des stocks automatisé, gestion des e-mails client avancés, plein de hooks partout.
- [WOOF](https://fr.wordpress.org/plugins/woocommerce-products-filter/) : Filtres plus riche en fonctionnalités que ceux de WooCommerce natif
- [Tickera](https://tickera.com/) Vente de billets, compatible avec WooCommerce.

## Sécurité

Masquer la version de WordPress (balise meta generator qui apparaît en front) à ajouter dans functions.php :

```php
function alsa_remove_generators() {
    add_filter( 'the_generator', '__return_false');
    // Remove WLW manifest
    remove_action( 'wp_head', 'wlwmanifest_link');
    // Display the links to the extra feeds such as category feeds
    remove_action( 'wp_head', 'feed_links_extra', 3 );
    // Display the links to the general feeds: Post and Comment Feed
    remove_action( 'wp_head', 'feed_links', 2 );
    // Display the link to the Really Simple Discovery service endpoint, EditURI link
    remove_action( 'wp_head', 'rsd_link' );
    // index link
    remove_action( 'wp_head', 'index_rel_link' );
    // prev link
    remove_action( 'wp_head', 'parent_post_rel_link', 10, 0 );
    // start link
    remove_action( 'wp_head', 'start_post_rel_link', 10, 0 );
    // Display relational links for the posts adjacent to the current post.
    remove_action( 'wp_head', 'adjacent_posts_rel_link', 10, 0 );
    // Display the XHTML generator that is generated on the wp_head hook, WP ver
    remove_action( 'wp_head', 'wp_generator' );
    remove_action( 'wp_head', 'wp_shortlink_wp_head' );
}
add_action( 'init', 'alsa_remove_generators' );
```

- Compléter le fichier wp-config.php avec les valeurs de <https://wordplate.github.io/salt/>
- Surveiller si le thème / les extensions utilisées font l’objet d’une faille sur [wpscan](https://wpscan.com/)
- Toujours utiliser [les nonces](https://css-tricks.com/wordpress-front-end-security-csrf-and-nonces/) pour éviter les CSRF, s’il faut développer des modules admin et/ou pour les utilisateurs identifiés sur le site.
- Désactiver l’édition du thème et des plugins en ligne dans wp-config.php `define('DISALLOW_FILE_EDIT', true);`
- [SF Author URL control](https://wordpress.org/plugins/sf-author-url-control/) personnalise le “author” et le slug utilisateur pour sécuriser et personnaliser les URL des pages auteur.
- [User Name Security](https://wordpress.org/plugins/user-name-security/) supprime les mentions de l’utilisateur (id et username) dans `body_class()`, entre autres choses.
- [disable-emojis](https://geek.hellyer.kiwi/plugins/disable-emojis/) pour désactiver les appels de scripts externes vers WordPress.

Bloquer xmlrpc (.htaccess)

```htaccess
<Files xmlrpc.php>
order deny,allow
deny from all
# allow from 123.123.123.123 (si IP identifiée)
</Files>
```

## Développement

- [Query Monitor](https://wordpress.org/plugins/query-monitor/) affiche les requêtes SQL exécutées et leur performance ainsi que les fichiers templates utilisés.

### Ajouter le support de Gutenberg pour les CPT

Si le projet nécessite d’utiliser Gutenberg, penser à ajouter `"show_in_rest" => true` et `"supports" => ['editor']` dans la déclaration des CPT.

### Ajout des fonctionnalités essentielles dans des mu-plugins

Toutes les fonctions de base et sur lesquelles un non-administrateur ne doit pas avoir la main doivent passer par des mu-plugins. C’est le cas notamment du renommage de fichiers dès l’upload dans la bibliothèque de médias, mais également du retrait des indices lors des erreurs de connexion au back-office (admin).

```php
function no_wordpress_errors() {
    return __( 'Something is wrong !', 'text-domain' );
}
add_filter( 'login_errors', 'no_wordpress_errors' );
```

## Performance

👉 Mettre en place un plugin de cache (voir extensions)

- Identifier les requêtes lentes <https://css-tricks.com/finding-and-fixing-slow-wordpress-database-queries/>

## Recette

👉 On utilise wp-migrate-db pour exporter les contenus en adaptant les URLs développement vers recette.

👉 Ne pas laisser indexer ce site par Google, en ajoutant une identification HTTP (par exemple avec .htaccess).

- Mise en place d’un webhook Gitlab possible pour auto-pull les derniers commits git.
- Checklist de qualité <https://wpaudit.site>

## Mise en ligne

👉 On utilise wp-migrate-db pour exporter les contenus en adaptant les URLs développement/recette vers production.

- Autoriser l’indexation par les robots à la mise en production (dans la configuration).
- Modifier l’adresse e-mail du compte administrateur.
- Activer le cache.
- Vérifier que toutes les anciennes URLs de développement ont disparu de la base.
- Modifier les constantes `WP_ENVIRONMENT_TYPE` à `production` et `WP_DEBUG` à `false`.

Si l'hébergement est mutualisé et ne permet de pointer dans le dossier /public, activer la réécriture avec un fichier .htaccess à la racine :

```htaccess
RewriteEngine on
RewriteRule ^(.*)$ /public/$1 [L]
```

## Maintenance

On peut utiliser [WP-CLI](https://www.smashingmagazine.com/2015/09/wordpress-management-with-wp-cli/) pour opérations pratiques en ligne de commande.

Forcer la mise à jour par téléchargement direct dans wp-config.php `define('FS_METHOD' 'direct');`

Désactiver le warning d'update WordPress pour les non-administrateurs

```php
if ( !current_user_can( 'edit_users' ) ) {
    add_action('admin_menu','wphidenag');
    function wphidenag() {
        remove_action( 'admin_notices', 'update_nag', 3 );
    }
}
```

Désactiver les notifications de mise à jour pour les non-admins

```php
function hide_update_notice_to_all_but_admin_users()
{
    if (!current_user_can('update_core')) {
        remove_action( 'admin_notices', 'update_nag', 3 );
        remove_action('load-update-core.php','wp_update_plugins');
        add_filter('pre_site_transient_update_plugins','__return_null');
        echo '<style>#setting-error-tgmpa>.updated settings-error notice is-dismissible, .update-nag, .updated, .core-updates { display: none; }</style>';
    }
}
add_action( 'admin_head', 'hide_update_notice_to_all_but_admin_users', 1 );
```

## Environnement sans Docker

### Installer PHP

Pour pouvoir exécuter composer en ligne de commande <https://www.php.net/downloads.php>

### Installer Composer

Suivre les instructions de <https://getcomposer.org/download/>

Sur macOS pour faire en sorte que la commande composer soit disponible partout :

```sh
mkdir -p /usr/local/bin
mv composer.phar /usr/local/bin/composer
```

### Installer WordPlate avec Composer

WordPlate <https://github.com/wordplate/wordplate>

```sh
composer create-project --prefer-dist wordplate/wordplate superprojet
```

Modifier le fichier `.env` avec les coordonnées de la base de données MySQL.

### Installer les plugins WordPress

Utiliser `composer` avec le nom du plugin, préfixé par “wpackagist-plugin”, par exemple `composer require wpackagist-plugin/wp-migrate-db`

### Développer le thème

Exploiter webpackmix intégré : `npm install`

Optionnel: `npm i concurrently -D`

```json
// Package.json
"scripts": {
    "serve": "php -S localhost:8000 -t public/",
    "build": "...",
    "dev": "...",
    "devstart": "concurrently \"npm run serve\" \"npm run dev\""
  },
```

Tâches :

- Démarrage du serveur php : `npm run serve`
- Démarrage du serveur dev (browsersync, css, js) : `npm run dev`
- Minification/compilation : `npm run build`

Optionnel : démarrage des deux en même temps: `npm run devstart`

Si cross-env n'est pas installé `npm install cross-env -g`
