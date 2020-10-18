YiiSoft news
============

News about yii progress and its team can be found on these sources:
- [opencollective](https://opencollective.com/yiisoft/#section-updates). Newsletters are sent for those who supports YiiSoft. You can also check for new entries by yourself.
- [@samdark](https://github.com/samdark) publishes the same news at [habr.com](https://habr.com/ru/users/samdark/posts/) in both russian and english languages.

Why do I need getters and setters instead of public properties?
===============================================================
When you have an object with, i.e., `name` property, there is a seduction to avoid adding a getter and a setter for it
in cases when there is no logic for getting and setting this property:
```php
public function getName()
{
    return $this->name;
}

public function setName($name)
{
    $this->name = $name;
}
```

For this implementation it is ok to have just a public property instead of private property, getter and setter.
But there is a big chance that there will appear some new requirements in the future. I.e., name must be trimmed
after two years of development: `$this->name = trim($name)`. If you used a private property and a setter, you
won't have any troubles. But if you used a public property, you have to find all the places you're setting a new value
into it and change `$variable->name = $value` to `$variable->name = trim($value)`. You also have to find all the places
like `$variable->$fieldName = $value`, where `$fieldName` is `name`. This leads to lots of extra work and bugs.

The conclusion is as simple as _"External code must not know anything about internal structure of class or module"_.
Use interfaces instead.

Why so many classes are final?
==============================
1. Static helpers are `final` because there is no any situation they can be extended. If you lack some functionality -
   please feel free to make an issue or a PR.
1. Some default interface implementations are `final` because they are just defaults. Making them non-final 
   won't let us refactor their internal logic, as it will be used by heirs. For these classes lack of configuration is a bug, and we will thank you
   for an issue. When the default implementation doesn't suite your needs at all, you have to create your own.
   If you want to read more about when and why `final` is good, have a look at the 
   [@ocramius](https://github.com/ocramius) article 
   "[When to declare classes final](https://ocramius.github.io/blog/when-to-declare-classes-final/)".
