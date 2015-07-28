---
layout: post
title:  "Configurer NodeJS derrière un proxy"
date:   2015-07-27
categories: "nodejs"
tags: nodejs configuration proxy npm
---

Si vous êtes derrière un **proxy**, vous devrez probablement le configurer pour lui indiquer l'adresse du proxy. La commande **npm** permet de configurer le comportement de *NodeJS*.

```bash
npm config set proxy http://votre-proxy.tdl:3238
npm config set https-proxy https://votre-proxy.tdl:3238
```
