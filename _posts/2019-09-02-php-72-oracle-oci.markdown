---
layout: post
title:  "Installation de OCI pour BDD Oracle pour PHP 7.2"
date:   2019-09-02 10:00:00
categories: "développement-web"
tags: php oracle oci8
---

Petite note pour installer le driver d'accès aux base de données Oracle pour PHP 7.2 (OCI8).


Télécharger l'**instant client** ici : https://www.oracle.com/database/technologies/instant-client/downloads.html


FIX :
PHP Warning:  PHP Startup: Unable to load dynamic library 'oci8' (tried: /usr/lib/php/20170718/oci8 (/usr/lib/php/20170718/oci8: cannot open shared object file: No such file or directory), /usr/lib/php/20170718/oci8.so (libnnz19.so: cannot open shared object file: No such file or directory)) in Unknown on line 0

```bash
# vérifier les dépendances de la librairie
ldd /usr/lib/php/20170718/oci8.so
	linux-vdso.so.1 (0x00007ffd92758000)
	libclntsh.so.19.1 => /opt/instantclient_19_3/libclntsh.so.19.1 (0x00007f0c6a248000)
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f0c69e57000)
	libnnz19.so => not found
	libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007f0c69c53000)
	libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x00007f0c698b5000)
	libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x00007f0c69696000)
	libnsl.so.1 => /lib/x86_64-linux-gnu/libnsl.so.1 (0x00007f0c6947c000)
	librt.so.1 => /lib/x86_64-linux-gnu/librt.so.1 (0x00007f0c69274000)
	libaio.so.1 => /lib/x86_64-linux-gnu/libaio.so.1 (0x00007f0c69072000)
	libresolv.so.2 => /lib/x86_64-linux-gnu/libresolv.so.2 (0x00007f0c68e57000)
	/lib64/ld-linux-x86-64.so.2 (0x00007f0c6e482000)
	libclntshcore.so.19.1 => not found
```

On peut voir que les SO **libnnz19.so** et **libclntshcore.so.19.1** sont manquantes / inaccessibles.

La globale **LD_LIBRARY_PATH** n'est peut-être pas correctement renseignée : 

```bash
export LD_LIBRARY_PATH=/opt/oracle/instantclient
```
