---
layout: post
title:  "Gulp"
date:   2016-01-06
categories: "utils"
tags: gulp task-runner
---

L'utilitaire [Gulp](http://gulpjs.com/) est un automatisateur de tâche (un *task runner*) concurent de **Grunt** basé sur **NodeJS**. Comme son homologue, il va vous permettre d'automatiser différentes tâches répétitives lors des étapes du développement.

## Installation

Étant basé sur **NodeJS**, **Gulp** s'installe via `npm` :

```bash
npm install -g gulp
```

Pour tester l'installation de **Gulp** :

```bash
gulp -v
```

## Sans être root

Placez vous dans votre répertoire personnel, puis lancez l'installation de gulp en local avec la commande `npm install gulp`. **NPM** aura créé un dossier **node_modules** avec un dossier **gulp** dedans.

Puis dans votre *.bashrc*, ajouter un alias :

```bash
alias gulp="~/node_modules/gulp/bin/gulp.js"
```

Puis testez avec la commande `gulp -v`

## Gulpfile : Bonjour Gulp

Pour configurer les différentes tâches, on utilise un fichier `gulpfile.js` que l'on place à la racine de son projet.

Commençons par créer une tâche **hello**.

```javascript
// gulpfile.js

var gulp = require('gulp');

// Création d'un tache 'hello'
gulp.task('hello', function(){
  console.log("Bonjour Gulp !");
});
```

Pour exécuter cette tache :

```bash
gulp hello
```
On obtient :

>[16:10:45] Using gulpfile ~/Test/Gulp/gulpfile.js
>[16:10:45] Starting 'hello'...
>Bonjour Gulp!
>[16:10:45] Finished 'hello' after 83 μs

## Principes

De base, Gulp est fournit *brut* avec un nombre très limité de
fonctionnalités. La commande `npm` vous permettra d'en ajouter pour
répondre à vous besoins.

Parmis les fonctionnalités de base on trouve :

 - L'ouverture d'un flux de fichier (`gulp.src(GLOB)`)
 - L'envoi de ce flux dans un plugin de traitement (`pipe()`)
 - L'envoi d'un flux dans une destination (`gulp.dest()`)


### Source : Glob

Le **Glob** est un ensemble (qui peut être vide) de fichier exprimé sour la forme d'un tableau d'emplacements. Ces emplacements peuvent utiliser des *wildcard* au format [Node Globs](https://github.com/isaacs/node-glob#glob-primer) (comparable à celle que l'on trouve dans les fichiers `.gitignore`):

```text
fichier.js    // un fichier précis
*.js          // Tous les fichiers JS directement dans l'emplacement
**/*.js       // Tous les fichiers JS récursivement
!**/*.min.js  // SAUF les fichiers .min.js
```

## Pipe

La méthode `pipe(FOO)` permet d'envoyer le flux de données dans un traitement, les traitements (*plugins*) étant chaînés, on peut faire intervenir plusieurs `pipe()` à la suite :

```javascript
// Gulpfile.js
var gulp = require('gulp');

// Principe du pipe
gulp.task('simple-demo', function(){  
  gulp.src('style/sass/*.scss')
    .pipe(plugin1())
    .pipe(plugin2())
    .pipe(pluginX())
    // ... etc
    .pipe(gulp.dest('style/css/'));
});
```

Plus compliqué (factorisation des traitements) : [Making reusable pipelines in a Gulp build](http://paulsalaets.com/posts/reusable-pipelines-in-gulp-build/).


### Exemple : Compilation SASS

Petit exemple avec le plugins **Gulp SASS** qui permet de compiler le contenu des fichiers SASS
en CSS.

```bash
npm install gulp-sass
```

Puis dans le `Gulpfile.js` :

```javascript
// Gulpfile.js
var gulp = require('gulp'),
    sass = require('gulp-sass');

// ...

gulp.task('sass', function(){
  console.log("Compilation SASS");

  // On lit les fichiers .scss du dossier style/sass
  gulp.src('style/sass/*.scss')

    // On les envois dans le plugin SASS
    .pipe(sass())

    // Puis le résultat est envoyé dans le dossier style/css
    .pipe(gulp.dest('style/css/'));
});
```

Pour tester créez un fichier `toto.scss` dans le dossier `style/sass` avec ce contenu :

```css
$color: pink;

body {
    background-color: $color;
}
```

Enfin, lancer la tache en tapant :

```bash
gulp sass
```

## Dépendances

Une tâche peut avoir des dépendances, dans notre exemple, on peut dire que la compilation ne peut être faite qu'après avoir éxécuté la tache **hello**, cela je définit dans la déclaration de la tache :

```javascript
// Gulpfile.js

// ...
gulp.task('sass', ['hello'], function(){
   // etc...
});
```

## Watch

Gulp sait surveiller les modifications sur les fichiers et lancer des tâches suite à ces changements avec la méthode `gulp.watch(GLOB, TASKS])` :

```javascript
// Gulpfile.js

// ...
gulp.task('sass:watch', function(){
  gulp.watch('style/sass/*.scss', ['sass']);
});
```

On lance ensuite la surveillance avec la commande `gulp sass:watch`. Dès qu'une modification interviens sur un fichier surveiller, Gulp lance la (ou les) tâches couplées.

## Appel système

```javascript
// gulfile.js
var gulp = require('gulp'),
    exec = require('child_process').exec
    ;

gulp.task('ls', function(){
  console.log("Liste des fichiers");
  exec('ls -la', function(err, stdout, stderr){
    if(err){
      console.log('Erreur', stderr);
    }
    console.log(stdout);
  });
});
```

Attention, cette éxécution est asynchrone :

```javascript
// gulfile.js
var gulp = require('gulp'),
    exec = require('child_process').exec
    ;

gulp.task('ls', function(){
  console.log("Liste des fichiers");
  exec('sleep 2; ls -la;', function(err, stdout, stderr){
    if(err){
      console.log('Erreur', stderr);
    }
    console.log(stdout);
  });
});

//
gulp.task('demo', ['ls'], function(){
  console.log("Je fais mon boulot");
});
```

Si l'on veut forcer la *synchronisation* :

```javascript
// gulfile.js
var gulp = require('gulp'),
    exec = require('child_process').exec
    ;

gulp.task('ls', function(cb){
  console.log("Liste des fichiers");
  exec('sleep 2; ls -la', function(err, stdout, stderr){
    if(err){
      console.log('Erreur', stderr);
      return cb(err);
    }
    console.log(stdout);
    cb();
  });
});

gulp.task('demo', ['ls'], function(){
  console.log("Je fais mon boulot");
});
```
