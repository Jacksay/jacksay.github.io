---
layout: post
title:  "Installation de PHP 7.3 sous Debian/Ubuntu"
date:   2019-09-17 10:00:00
categories: "développement-web"
tags: php debian
---

## Maj du système

On commence par mettre à jour le système :

```bash
apt update
apt upgrade
```

## Installation des paquets PHP 7.3

```bash
apt install php7.3 php7.3-bz2 php7.3-cli php7.3-curl php7.3-dom php7.3-gd php7.3-intl php7.3-ldap php7.3-mbstring php7.3-pgsql php7.3-xml php7.3-zip
```



## Switcher sur PHP 7.3 (si besoin)

```bash
update-alternatives --set php /usr/bin/php7.3
update-alternatives --set phar /usr/bin/phar7.3
update-alternatives --set phar.phar /usr/bin/phar.phar7.3
```
