---
layout: post
title:  "Créer des validateurs personnalisés dans Zend Framework 2"
date:   2015-10-19
categories: "middleware"
tags: zf2 validator
---

Voici la procédure pour créer des Validators personnalisés dans *Zend Framework*. Pour illustrer mon propos, nous allons créer un validateur `MotDePassePourri` qui nous permettra d'invalider la saisi un mot de passe foireux (genre AZERTY ou 1234).

<!-- more -->

# Classe de Validation

Dans Zend Framework 2, un validator doit implémenter l'interface **Zend\Validator\ValidatorInterface**. Cette interface impose la présence des méthodes :

* **isValid($value)** Qui va retourner un booléen indiquant si la donnée est valide,
* **getMessages()** Qui va retourner les messages d'erreurs.

Le moyen le plus rapide est d'étendre la classe **Zend\Validator\AbstractValidator**, cette dernière va assurer la gestion des messages (en grande partie), ce qui va vous permettre de vous concentrer sur la validation elle-même.

La classe vide se présentera comme ça :

```php
<?php
namespace Application\Validator;

class Application\Validator\MotDePassePourri extends Zend\Validator\AbstractValidator
{
    public function isValid($value)
    {
        // Validation ici
    }
}
```

Concernant notre validateur, Il y'a 2 parties : Les messages d'erreurs à définir, et la validation.

## Les messages d'erreur

Cette partie est **essentielle** car s'ils ne sont pas définis, la validation ne fonctionnera pas.

Pour les définir, on va créer un tableau associatif `$messageTemplates`.

```php
<?php
namespace Application\Validator;

class Application\Validator\MotDePassePourri extends Zend\Validator\AbstractValidator
{

    protected $messageTemplates = array(
        'ROTTEN_PASSWORD' => "Votre mot de passe est merdique."
    );

    public function isValid($value)
    {
      // validation ici
    }
}
```

## Validation

Le plus dur est fait. Pour valider les valeurs, il ne vous reste plus qu'a placer la logique dans la méthode **isValid($value)**, vous gèrerez les messages d'erreurs via la méthode **error($messageKey)**, où **messageKey** correspond à une clef que vous avez définit dans le tableau `$messageTemplates` de l'étape précédente.

```php
<?php
namespace Application\Validator;

class Application\Validator\MotDePassePourri extends Zend\Validator\AbstractValidator
{

    protected $messageTemplates = array(
        'ROTTEN_PASSWORD' => "Votre mot de passe est merdique."
    );

    public function isValid($value)
    {
        if( in_array($value, ['azerty', '1234']) ){
            $this->error('ROTTEN_PASSWORD');
            return false;
        }
        return true;
    }
}
```

Pour éviter les dérapages de touche, vous pouvez utiliser des **constantes de classe** pour gérer les messageKey de votre validateur :

```php
<?php
namespace Application\Validator;

class Application\Validator\MotDePassePourri extends Zend\Validator\AbstractValidator
{
    const ROTTEN_PASSWORD = 'rottenPassword';

    protected $messageTemplates = array(
        self::ROTTEN_PASSWORD => "Votre mot de passe est merdique."
    );

    public function isValid($value)
    {
        if( in_array($value, ['azerty', '1234']) ){
            $this->error(self::ROTTEN_PASSWORD);
            return false;
        }
        return true;
    }
}
```

<div class="alert alert-info">
<strong>Pourquoi on s'emm... avec ce système de message ?</strong> Très bonne question. Ce système
permet de simplifier l'internationnalisation (traduction) des messages, de fait, il vous semblera
une source de complication inutile (mais nécessaire) si votre application est monolangue...

</div>


# Validator dans la classe Form

Côté formulaire, vous pourrez utiliser votre validateur dans la méthode **getInputFilterSpecification()** de la même façon qu'un validateur natif :

```php
<?php
namespace Application\Form;

class Application\Form\SuperForm extends Zend\Form\Form
{
    //...
    public function getInputFilterSpecification()
    {
        return [
            'passwordField'=> [
              // Si vous mettez required sur false
              'required' => true,
              'validators' => [
                  new Application\Validator\MotDePassePourri()
              ]
        ],
    }
}
```

# Message d'erreur plus sexy

## Afficher la valeur saisie

Le système de templates permet de réutiliser la valeur renseignée par l'utilisateur avec la séquence de caractères `%value%`. Dans ce cas, vous devrez utiliser la méthode **setValue($value)** pour que la validator ait la valeur saisie à disposition au moment d'écrire la message d'erreur :  

```php
<?php
namespace Application\Validator;

class Application\Validator\MotDePassePourri extends Zend\Validator\AbstractValidator
{
    const ROTTEN_PASSWORD = 'rottenPassword';

    protected $messageTemplates = array(
        // On peut utiliser %value% pour afficher la saisie
        self::ROTTEN_PASSWORD => "'%value%', ce mot de passe est trop merdique."
    );

    public function isValid($value)
    {
        // On mémorise la valeur pour la réafficher avec les templates
        $this->setValue($value);

        if( in_array($value, ['azerty', '1234']) ){
            $this->error(self::ROTTEN_PASSWORD);
            return false;
        }
        return true;
    }
}
```
