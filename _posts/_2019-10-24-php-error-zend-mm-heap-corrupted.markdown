---
layout: post
title:  "Installation de PHP 7.3 sous Debian/Ubuntu"
date:   2019-10-28 10:00:00
categories: "développement-web"
tags: php debian error
---

## Problème

Lenteur de l'appilication, voir erreur d'execution.

Après inspection des logs, l'erreur **zen_mm_heap_corrupted** est présente dans les logs (`/var/log/apache2/error.log`).

L'erreur semble être liée au cache PHP (APC)
