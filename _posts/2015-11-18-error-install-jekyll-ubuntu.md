---
layout: post
title:  "Erreur en installant Jekyll"
date:   2015-11-18
categories: "utils"
tags: jekyll ruby
---

Si vous rencontrez l'erreur suivante en installant **Jekyll** sous *Ubuntu* :
Building native extensions.  mkmf.rb can't find header files...

# Erreur : mkmf.rb can't find header files

Voici l'erreur complète tel que me l'a craché la console :

```
Building native extensions.  This could take a while...
ERROR:  Error installing jekyll:
	ERROR: Failed to build gem native extension.

    /usr/bin/ruby2.1 extconf.rb
mkmf.rb can't find header files for ruby at /usr/lib/ruby/include/ruby.h
```

Vérifiez que vous avez bien installé le paquet **ruby-dev**.

Si ça n'est pas le cas, un petit **apt-get install** réglera le problème :

```bash
sudo apt-get install ruby-dev
```

# Dependency Error: Yikes! It looks like you don't have pygments

Autre erreur rencontrée, l'absence de la dépendance **pygments** pour la coloration syntaxique du code.

Comme je suis un optimiste, j'ai tenté un petit `gem install pygments` de derrière les fagots, mais sans succès. Après une petite recherche, le paquet à installé s'appelle **pygments.rb** :

```bash
gem install pygments.rb
```
