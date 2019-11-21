---
id: index
title: Composer
---

<!----- Conversion time: 1.451 seconds.


Using this Markdown file:

1. Cut and paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs to Markdown version 1.0β14
* Sun Feb 10 2019 09:42:08 GMT-0800 (PST)
* Source doc: https://docs.google.com/open?id=1G13Mo1F9vIXTDMyWss4LjFDW4tvET7mDKJd9i9UIVOE
----->


[Composer](https://getcomposer.org/) is a dependency manager and autoloader for PHP.

[https://getcomposer.org/doc/articles/versions.md](https://getcomposer.org/doc/articles/versions.md)

**[https://getcomposer.org/doc/01-basic-usage.md](https://getcomposer.org/doc/01-basic-usage.md)**

**[https://packagist.org/](https://packagist.org/)**


## self-update composer in your system


## composer self-update


## initialize into project


## composer init


## Create a project


## composer create-project package/name name-of-project


## include required packagist on the project


## composer require package/name


## remove package on the project


## composer remove package/name



1.  PSR-4 autoload setup
    1.  update composer.json

        ```
"autoload": {
   "psr-4": {
       "PDP\\": "src/"
   }
}
```


    1.  run dump-autoload in composer


## composer dump-autoload



    1.  include vender/autoloader.php in index.php


```
require __DIR__ . '/vendor/autoload.php';
```



## View the versions of all the packages in your project


## composer show -i


## View all the dependencies of the installed packages in a tree


## composer show -t


## If you need help using a specific package then you can open it's documentation in a browser using composer


## composer home spatie/laravel-fractal


## validation of the composer.json file


## composer validate


## Alphabetically sort dependencies in composer.json


## composer require spatie/string --sort-packages


## The composer log file



*   created when we run  composer install/update 
*   Keeps an **exact** record of the dependency versions that have been installed.
*   running composer install with existing composer.log file will install all the exact dependencies which were recorded on log file earlier.
*   versioning the log file?
    *   record the versions that were used in the application?
    *   maintain same versions across the team
    *   deploying the application using git
*   Actions that will update composer.log file
    *   You run composer install for the first time, and the composer.lock file is updated to the installed versions of the dependencies.
    *   You run composer install after adding a new package, and the exact version of the new package is added to the composer.lock file.
    *   You run composer update, and all of your packages are updated to their latest versions according to the composer.json. This will update the exact version records in the composer.lock file.
    *   You run composer install package/name and the version of the specified package is updated to it's latest version observing the package version hint in composer.json. The exact version in the composer.lock file is updated to respect this.
1.  **K**

**[https://seld.be/notes/typo-squatting-and-packagist](https://seld.be/notes/typo-squatting-and-packagist)**


## Using composer with legacy PHP applications

[https://yuloh.github.io/2015/using_composer_with_legacy_applications/](https://yuloh.github.io/2015/using_composer_with_legacy_applications/)


### The issue

Most legacy applications do not follow a consistent standard like PSR-0 or psr-4.

Normally composer takes a classname like this: Acme\Foo\Bar

And checks the autoload section of composer.json. If you had something like this:


```
"autoload": {
   "psr-4": {
       "Acme\\": ["src/"]
   }
}
```


It would check the src folder for a file at src/Foo/Bar.php. The legacy application I work on uses folders for organization, yet namespaces are virtually nonexistent. PSR-4 autoloading is not going to work without renaming, reorganizing, & namespacing a few hundred files. After this I would be fixing all of the manual includes developers used because the autoloader was unreliable, and spot testing a ton of files.


### Classmap

I thought this would be a problem but composer works fine with any naming convention. The trick is to setup your composer.json with a[ classmap](https://getcomposer.org/doc/04-schema.md#classmap) key.

On production, composer recommends you use the --optimize-autoloader (-o) flag. This takes PSR-0 / PSR-4 autoloading and generates a classmap.

A classmap is a big array of class names and their corresponding paths. You can register your classes folder under a classmap key and it will always generate a classmap instead of trying to autoload using a PSR standard.


```
"autoload": {
   "classmap": ["src/", "lib/", "Something.php"]
}
```


Now running composer **dump-autoload** will generate an associative array of all your class names and their corresponding files.

The first time you dump-autoload you may have some class conflicts. In my situation developers somehow (using includes?) managed to have multiple classes with the same non-namespaced names in our autoloaded folder.

You may have to manually namespace a couple files to prevent conflicts, than dump-autoload again. It still beats namespacing and renaming **every** file.


### Autoloading functions

moving all functions to static methods on classes to allow autoloading as a first step towards modernizing legacy code.

This is a great first step as you can now move global functions into namespaced classes. Legacy applications seem to like including page specific functions on those pages only, and as a result have conflicting names with other functions.

The other benefit is functions are only loaded when you need them now, instead of every function being included on every request, regardless of being used.

If you can't do this yet, you can use composer to load functions instead of includes. Just add a[ files](https://getcomposer.org/doc/04-schema.md#files) key to your autoload section.


```
"autoload": {
   "files": ["src/MyLibrary/functions.php"]
}
```



### Benefits

In my unscientific tests, composer was able to load classes twice as fast on average, and sometimes 10x faster than the legacy autoloader we were using. This is due to using a classmap instead of recursively scanning directories for a file. Files that were nested deeply loaded incredibly slow, as the legacy autoloader was doing file_exists over and over.

When refactoring legacy code, its really helpful to get vendor code out of the source tree so you can see what is going on. Using Composer for it's primary purpose, as a dependency manager, allows moving other people's code out of your repository. It makes project wide search easier when refactoring, and also helps discourage modifying vendor files without making it explicit.

[http://blog.wyrihaximus.net/2015/07/composer-cache-on-travis/](http://blog.wyrihaximus.net/2015/07/composer-cache-on-travis/)


## Local Composer

Before booting up the **homestead**, make sure you share an arbitrary port. This can be done by editing the bottom section of Homestead.yaml. I picked 6789, so that my local "packagist" will be hosted on IP:6789 where IP will be the IP address of my host machine.

[https://www.sitepoint.com/local-composer-for-everyone-a-conference-friendly-satis-setup/](https://www.sitepoint.com/local-composer-for-everyone-a-conference-friendly-satis-setup/)


### Satis

Inside the VM, in an arbitrary location of your choice (I picked /home/vagrant/Code/) install a new Satis project with:


```
composer create-project composer/satis --stability=dev --keep-vcs
```


Check the code on: ~/Code/satis


### How Satis Works

Satis accepts a satis.json file which tells it which repositories to download, which versions of those repos to download, where to place them after the download, and more. For a more comprehensive overview of all options you can use in satis.json, see[ the docs](https://getcomposer.org/doc/articles/handling-private-packages-with-satis.md#options-explained).

Secret Details:

Token: e65874059b9800fc66e55f2882f125bdc983b11c

 It will be stored in "/home/vagrant/.composer/auth.json" for future use by Composer.


### composer-patches

[https://github.com/cweagans/composer-patches](https://github.com/cweagans/composer-patches)

[http://mamchenkov.net/wordpress/2017/01/31/composer-patches-simple-patches-plugin-for-composer/](http://mamchenkov.net/wordpress/2017/01/31/composer-patches-simple-patches-plugin-for-composer/)


<!-- Docs to Markdown version 1.0β14 -->
