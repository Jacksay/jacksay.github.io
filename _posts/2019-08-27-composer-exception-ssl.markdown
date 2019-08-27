---
layout: post
title:  "Composer : Erreur SSL lors d'une mise à jour"
date:   2019-08-27 10:00:00
categories: "développement-web"
tags: php composer exception
---

En tentant de réaliser une mise à jour de mes paquets PHP via *composer*, j'ai obtenu ce message d'erreur :

```
[Composer\Downloader\TransportException]
  The "http://repo.packagist.org/p/composer/ca-bundle%241d01508e6da0a67953826a9d983c824ba981e5b21a75ebc7aa78a7165e627d51.json" file could not be downloaded: SSL operation failed with code 1. OpenSSL Error messages:
  error:1408F10B:SSL routines:ssl3_get_record:wrong version number
  Failed to enable crypto
  failed to open stream: operation failed
```

---

## Vérifier l'extension SSL sur PHP

Après recherche, j'ai **vérifier que SSL était bien disponible sur PHP** avec la commande :

```bash
php -i | grep -i ssl
```

Et il l'est :

```bash
Registered Stream Socket Transports => tcp, udp, unix, udg, ssl, tls, tlsv1.0, tlsv1.1, tlsv1.2
SSL => Yes
SSL Version => OpenSSL/1.1.1
openssl
OpenSSL support => enabled
OpenSSL Library Version => OpenSSL 1.1.1  11 Sep 2018
OpenSSL Header Version => OpenSSL 1.1.0g  2 Nov 2017
Openssl default config => /usr/lib/ssl/openssl.cnf
openssl.cafile => no value => no value
openssl.capath => no value => no value
SSL support => enabled
Native OpenSSL support => enabled
```

On demande un petit diagnostique à composer avec la commande :

```bash
composer diagnose
```

## Vérifier le proxy

Et là on voit que le check sur le proxy ne passe pas :

```
Checking platform settings: OK
Checking git settings: OK
Checking http connectivity to packagist: [Composer\Downloader\TransportException] The "http://repo.packagist.org/packages.json" file could not be downloaded: SSL operation failed with code 1. OpenSSL Error messages:
error:1408F10B:SSL routines:ssl3_get_record:wrong version number
Failed to enable crypto
failed to open stream: operation failed
Checking https connectivity to packagist: [Composer\Downloader\TransportException] The "https://repo.packagist.org/packages.json" file could not be downloaded: SSL operation failed with code 1. OpenSSL Error messages:
error:1408F10B:SSL routines:ssl3_get_record:wrong version number
Failed to enable crypto
failed to open stream: operation failed
Checking HTTP proxy: FAIL
```

On force le proxy :

```bash
export http_proxy=http://proxy.domain.ext:3128
export https_proxy=http://proxy.domain.ext:3128
```
