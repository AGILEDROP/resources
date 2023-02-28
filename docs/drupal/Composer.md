---
id: composer
title: Composer
description: Tips and tricks for working with Composer in Drupal.
---

## Patch a module that is missing core requirement
If you want to install a module which hasn't been updated for Drupal 10 (core_version_requirements missing) but there is
a patch available you can use a Composer plugin to install it anyway and patch later. 

``` bash
composer require mglaman/composer-drupal-lenient
```

Full information on [drupal.org](https://www.drupal.org/docs/develop/using-composer/using-drupals-lenient-composer-endpoint) or 
directly on the [project github](https://github.com/mglaman/composer-drupal-lenient).