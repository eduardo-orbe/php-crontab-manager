PHP Crontab Manager
===================

Last build status: [![Build Status](https://secure.travis-ci.org/MediovskiTechnology/php-crontab-manager.png?branch=master)](http://travis-ci.org/MediovskiTechnology/php-crontab-manager)

Manages crontab file by adding and deleting the appropriate entries. It is able
to track the source file so that after the changes in this file to upgrade a 
crontab file to the target.

Installation
------------

* copy files to your project
* include files from src directory or use some autoloader
* use it as described below

Usage
-----

Here is a simple example of use. Adding a simple task to crontab:

```php
<?php
use php\manager\crontab\CrontabManager;

$crontab = new CrontabManager();
$job = $crontab->newJob();
$job->on('* * * * *');
$job->onMinute('20-30')->doJob("echo foo");
$crontab->add($job);
$job->onMinute('35-40')->doJob("echo bar");
$crontab->add($job);
$crontab->save();
```
    
A more complex example, but simpler to write. Adding and removing files to 
manage by the cron job. Files will be updated so as not to disrupt other tasks
in the cron:

```php
<?php
use php\manager\crontab\CrontabManager;

$crontab = new CrontabManager();
$crontab->enableOrUpdate('/tmp/my/crontab.txt');
$crontab->disable('/tmp/toremove.txt');
$crontab->save();
```

You can also use the built-in tools from the console: cronman located in the
directory `bin/` for ex.:

    bin/cronman --enable /var/www/myproject/.cronfile --user www-data