---
layout: post
title:  "Se connecter à MySQL en PHP avec PDO"
date:   2015-08-05 12:55:45
categories: "développement-web"
tags: php pdo mysql connection sgbd
---

L'utilisation du pré-processeur CSS [SASS][sass] permet de coordonner l'utilisation des boucles et de tableaux de données pour automatiser la production de règles CSS.

<!-- more -->

# Titre 1

## Titre 1.1

### principe

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

### Exemple

```sass
// liste des noms de couleurs
$colorsName: rouge vert bleu;

// Liste des valeurs
$colorsValue: #ff0000 #00ff00 #0000ff;
```

## Titre 1.2

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

# Parcourir une liste avec une boucle dans SASS

La boucle `@each` permet de parcourir une (ou plusieurs) liste :

```sass
// liste des noms de couleurs
$colorsName: rouge vert bleu;

// Liste des valeurs
$colorsValue: #ff0000 #00ff00 #0000ff;

@for $i from 1 through length($colorsName){
  .color-#{nth($colorsName,$i)} {
    background-color: nth($colorsValue, $i);
  }
}
```

Nous obtenons :

```css
.color-rouge {
  background-color: #ff0000;
}
.color-vert {
  background-color: #00ff00;
}
.color-bleu {
  background-color: #0000ff;
}

```

Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll’s dedicated Help repository][jekyll-help].

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
