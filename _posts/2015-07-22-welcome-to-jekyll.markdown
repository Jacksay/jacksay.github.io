---
layout: post
title:  "Utiliser les boucles et les tableaux dans SASS"
date:   2015-07-22 12:55:45
categories: "integration-web"
tags: sass
---

L'utilisation du pré-processeur CSS [SASS][sass] permet de coordonner l'utilisation des boucles et de tableaux de données pour automatiser la production de règles CSS.

<!-- more -->

# Les tableaux dans SASS

Pour créer des *listes de données dans SASS*, il suffit d'affecter les valeurs à une variables en les séparant par des espaces :

```sass
// liste des noms de couleurs
$colorsName: rouge vert bleu;

// Liste des valeurs
$colorsValue: #ff0000 #00ff00 #0000ff;
```

Nous venons de créer 2 listes contenant chaqu'une 3 valeurs.

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
