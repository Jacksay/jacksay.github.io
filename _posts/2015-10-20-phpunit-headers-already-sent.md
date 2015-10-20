---
layout: post
title:  "PHPUnit, l'erreur header already sent"
date:   2015-10-19
categories: "php"
tags: phpunit bug
---

Si vous utilisez **PHPUnit** pour tester votre code, vous avez peu-être déjà
rencontrée l'erreur **... - headers already sent** sous la forme **Cannot modify header information - headers already sent by ( output started at /un/fichier/dans/la/matrice.php:666)**

<!-- more -->

# L'origine du mal

Généralement, cette erreur viendra de vous. Un echo maladroit ou un var_dump égaré sera à l'origine du problème.

Mais il se peut que l'origine du problème soit perdue au fin fond de votre framework préféré, un endroit où vous n'avez jamais posé les pieds et dont les arcanes vous sont inconnues.

Cette erreur survient, semble t'il, lors que du code commence à bricoler dans les
session/header. Dans mon cas, le problème prenez racine dans les classes de gestion
des sessions dans **Zend framework**.

Plusieurs issues sont visibles sur github, le problème a apparement
été réglé dans des versions précédente pour ensuite réapparaître dans la dernière
version de PHPUnit (PHPUnit 4.8.*).

# La solution de merde

Pour le moment, j'ai contourné le problème en ajoutant l'annotation **@runInSeparateProcess**.

Lors de mes périgrinations, je suis également tombé sur une variante de cette solution impliquant l'ajout
au début des méthodes de test d'un fonction header bidon **header('Location: http://wtf');**, j'ai retiré
la ligne en question car mes tests unitaires fonctionnent sans.

```php
<?php
namespace Wtf\Test;

class MyTest extends PHPUnit_Framework_TestCase
{
    /**
     * @runInSeparateProcess
     */
    public function testUnTruc()
    {
        // Le test ici
    }
}
```

Oui, c'est de la grosse **patafix** dégueux. Mais c'est ça ou pas de test (celui qui me répond pas de test dégage immédiatement).
