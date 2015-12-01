---
layout: post
title:  "Utiliser Require JS pour optimiser le chargement de ces fichiers Javascript"
date:   2015-09-15
categories: "ui"
author: Stéphane Bouvry
tags: requirejs javascript webapp amd
---

Cette article s'appuit sur l'hypothèse que vous avez des connaissances minimales en Javascript et que vous avez déjà développé des scripts Javascript complexe ; une logique forte que vous avez décomposé dans plusieurs fichiers.

<!-- more -->

# Débuter avec RequireJS ?

**RequireJS** est une librairie javascript qui va vous permettre d'optimiser le chargement des fichiers javascript de votre site ou de votre application web. Son principal interêt est de favoriser la décomposition de votre code en petits modules indépendants en ne chargeant que les fichiers requis.

## Débuter avec RequireJS

Commençons par une exemple simple, pour cela, créez cette arborescence, vous pouvez télécharger les libairies javascript de l'exemple :

 - Jquery : [Jquery]
 - RequireJS : [requirejs]

<pre>
.
├── index.html
└── js
    ├── libs
    │   ├── jquery.min.js
    │   └── require.js
    └── script1.js
</pre>

Nous allons écrire un peu de code pour utiliser **RequireJS** afin de charger le contenu du fichier `script1.js`.

Voici le contenu du fichier HTML :

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Exemple RequireJS</title>
</head>
<body>
  <script src="js/libs/require.js"></script>
  <script>
  // Chargement du fichier js/script1.js
  require(['js/script1'], function(){
    console.log('Chargement terminé !');
  });
  </script>
</body>
</html>
```

Dans cet exemple, nous avons commencé par charger **RequireJS** avec une balise `script`, puis nous avons utilisé la fonction **require()** pour demander à *RequireJS* de charger le fichier `js/script1.js`. Vous noterez qu'il n'est pas necessaire de préciser l'extention `.js` et que le premier paramètre est toujours un tableau même si ce dernier ne contient qu'un seul élément.

Vous pouvez vérifier le bon fonctionnement de votre script en ajoutant ce code qu fichier `script1.js` :

```js
// js/script1.js
window.Namespace = window.Namespace || {};
window.Namespace.version = "1.0";
window.Namespace.appName = "SuperApp";
```

Puis dans le script situé dans le fichier `index.html` :

```js
require(['js/script1'], function(){
  console.log('Chargement terminé !');
  console.log(Namespace.appName, Namespace.version);
});
```

Vous verrez s'afficher dans la console le résultat stupéfiant ci-dessous :


![Le résultat dans la console](/images/require-js-01.jpg)



Bon pour le moment, rien de bien révolutionnaire. Une balise `<script>` aurait permis d'obtenir le même résultat. Mais comme expliqué plus haut, l'usage de **RequireJS** permet de charger les fichiers Javascript à la demande.

## Charger des dépendances avec la fonction *require([dep], callback)*


Supposons maintenant que notre script dans le fichier `script1.js` ai besoin de *JQuery* à un moment précis. Par exemple quand un clique se produit dans la page. Nous voudrions charger la librairie *JQuery* pour pouvoir modifier le contenu de la page. (L'exemple est stupide en soi mais permet de comprendre le principe de chargement *différé*). On va donc éditer le contenu de `script1.js` de cette manière :

```js
// js/script1.js
window.Namespace = window.Namespace || {};
window.Namespace.version = "1.0";
window.Namespace.appName = "SuperApp";

// Lorsque l'on clique sur le document...
// NB, selon la navigateur, la fonction écouteur est :
// - document.addEventListener(...) avec Firefox
// - document.addEvent(...) avec chrome
// - IE... serieux ???
document.addEventListener('click', function(){
   // ... On demande à RequireJS de charger jQuery
  require(['js/libs/jquery.min'], function(){
     // ... Une fois jQuery chargé, on change le DOM dans la page
    $('body').append('<h1>' +Namespace.appName+ " " +Namespace.version +"</h1>");
  });
});
```

Comme on peut le voir dans cet exemple, la fonction `require` placée dans `index.html` déclenche le chargement du fichier `script1.js`.

Dès que l'on clique dans le document, **RequireJS** charge JQuery puis exécute le code placé dans la *callback*.


# Configuration de RequireJS

Il est possible de configurer **RequireJS** pour simplifier la gestion des fichiers ; principalement le chargement des librairies et des modules. Cette configuration vous permettra de paramétrer pas mal de chose, et notamment le dossier de base (pour nous le dossier **js/**), les emplacements spécifiques et des alias pour les librairies.

Voici un exemple de configuration, modifier le fichier *index.html* :

```html
<body>
  <script src="js/libs/require.js"></script>
  <script>
  // Configuration de RequireJS
  requirejs.config({
      // Configuration ici
  });

  // Chargement du fichier js/script1.js
  require(['js/script1'], function(){
    console.log('Chargement terminé !');
  });
  </script>
</body>
```

## Dossier racine

La propriété `baseUrl` permet de fixer un préfixe d'URL pour éviter d'avoir à le répéter, dans notre cas, tous les fichiers sont dans le dossier *js/*, nous allons donc l'indiquer dans la configuration :


```js
requirejs.config({
  // URL de base
  baseUrl: 'js/'
});

// Chargement du fichier js/script1.js
require(['script1'], function(){
  console.log('Chargement terminé !');
});
```

Pensez à mettre à jour les chemins utilisé dans les différents scripts.

## Chemins prédéfinis

La propriété `paths` permet de définir des alias, il est généralement utilisé pour les librairies ou pour gérer plus facilement des structures complexes :

```js
requirejs.config({
  // URL de base
  baseUrl: 'js/',
  paths: {
    jquery: 'libs/jquery.min'
  }
});
```
Et dans le fichier *script1.js* :

```js
// js/script1.js
window.Namespace = window.Namespace || {};
window.Namespace.version = "1.0";
window.Namespace.appName = "SuperApp";

document.addEventListener('click', function(){
  // On peut maintenant utiliser l'alias plutôt que le chemin
  // complet
  require(['jquery'], function(){
    $('body').append('<h1>' +Namespace.appName+ " " +Namespace.version +"</h1>");
  });
});
```

## Utiliser un fichier de Configuration

Pour cela nous allons créer un fichier de configuration **config.js** que nous placerons dans le dossier **js/** (au même niveau que le fichier **script1.js**).

# Créer des modules

L'intêret de tous ça, c'est de commencer à décomposer son application en 

## Shim, gérer ces dépendances

Bien entendu, cette ligne doit intervenir *après la chargement de RequireJS*.




[requirejs]:      http://www.requirejs.org
[jquery]:      https://jquery.com
