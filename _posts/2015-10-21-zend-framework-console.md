---
layout: post
title: "Créer ces propres utilitaires en ligne de commande dans Zend Framework 2"
date: 2015-10-21
tags: [Zend Framework, Console]
category: PHP
author: Stéphane Bouvry
---

Zend Framework 2 permet développer des utilitaires php en ligne de commande. Ce genre
d'outil permet d'isoler des procédures de maintenance ou des tâches spécifiques que vous
pourrez exécuter via la console ou une tâche CRON.

# Déclarer le controller

Première étape (si ça n'est pas déjà fait), déclarer le controller dans le
configuration. Dans mon exemple je vais créer un nouveau controlleur qui ne fait pas
grand chose :

```php
<?php
// module/Application/src/Controller/ConsoleController

namespace Application\Controller;

use Zend\Mvc\Controller\AbstractActionController;

class ConsoleController extends AbstractActionController
{
    public function syncAction()
    {
        die('SYNC');
    }
}
```

Ce controlleur sera déclaré **invokable** dans la configuration de Zend, comme pour vos autres controlleurs :

```php
<?php
// module/Application/config/module.config.php
return [
    // ...
    'controllers' => [
            // Vos autres controlleurs ...
            'invokables' => [
                'ConsoleController' => \Application\Controller\ConsoleController::class,
            ]
        ]
    ]
];
```


Ensuite on va dans la configuration du module pour y ajouter l'index 'console' :


```php
<?php
return [
  // Votre conf est ici ...
  'console' => [
         'router' => [
             'routes' => [
                 'macommande' => [
                     'options' => [
                         'route' => 'macommande',
                         'defaults' => [
                             'controller' => 'ConsoleController',
                             'action' => 'sync',
                         ],
                     ],
                 ],
             ],
         ],
     ],
];
```

Voilà, il vous suffit maintenant d'ouvrire un terminal, de vous rendre à la racine de votre application et de tapper :

```bash
php public/index.php macommande
```

# Utiliser les flags

Comme d'autres utilitaires en ligne de commande
