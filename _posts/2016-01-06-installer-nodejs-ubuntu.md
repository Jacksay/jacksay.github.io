---
layout: post
title:  "Installer NodeJS sur Debian/Ubuntu"
date:   2016-01-06
categories: "utils"
tags: linux nodejs proxy
---

Récapitulatif des différentes étapes pour installer **NodeJS** sur un système *Linux* (Basé sur Debian) ainsi que la configuration de **NPM** lorsque l'on est derrière un proxy.

## NodeJS

Via **APT** :

```bash
sudo apt-get install nodejs-legacy
```

Pour tester l'installation, la commande `node -v` vous affichera la version installée.


## NPM

**NPM** est le gestionnaire de paquet de nodejs, il n'est pas installé par défaut lors de l'installation de **NodeJS**, pour l'installer on utilise également **APT** :

```bash
sudo apt-get install npm
```

Si tout c'est bien passer, la commande `npm -v` devrait vous afficher la version.

### Proxy

Quand on est derrière une proxy, **NPM** risque de ne pas fonctionner, et ce même si vous avez correctement déclaré les variables d'environnement `HTTP_PROXY`. L'URL du proxy doit être renseignée à **NPM** avec la commande `npm config CONF`, pensez à suspendre le mode strict SSL et à modifier l'URL de registry en fesant sauter le S de HTTPS :

```bash
npm config set strict-ssl false
npm config set registry http://registry.npmjs.org
npm config set proxy http://proxy.domain:3128
npm config set https-proxy http://proxy.domain:3128
```

Vous pouvez tester si tout a bien fonctionné avec la commande `npm view gulp`.
