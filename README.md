# Running Multiple Applications with one CodeIgniter Installation

See https://codeigniter4.github.io/CodeIgniter4/general/managing_apps.html#running-multiple-applications-with-one-codeigniter-installation

## Folder Structure

```
codeigniter4-multiple-apps-sample/
├── bar/
│   ├── app/
│   ├── env
│   ├── phpunit.xml.dist
│   ├── public/
│   ├── spark
│   ├── tests/
│   └── writable/
├── foo/
│   ├── app/
│   ├── env
│   ├── phpunit.xml.dist
│   ├── public/
│   ├── spark
│   ├── tests/
│   └── writable/
├── vendor/
│    ├── autoload.php
│    ├── bin/
│    ├── codeigniter4/framework/
│    └── composer/
├── LICENSE
├── README.md
├── builds
├── composer.json
└── composer.lock
```

## Install CodeIgniter4

Install CodeIgniter4 with Composer.

```
$ composer create-project codeigniter4/appstarter multiple
```

## Create Folders for Apps

Create `foo/` for one app.

```
$ cd multiple/
$ mkdir foo
```

Move files into `foo/`.

```
$ mv app/ public/ tests/ writable/ spark env phpunit.xml.dist foo/
```

Create `bar/` for another app.

```
$ cp -pr foo/ bar/
```

## Fix Composer autoload

Fix `composer.json`.

```diff
--- a/composer.json
+++ b/composer.json
@@ -17,10 +17,6 @@
         "ext-fileinfo": "Improves mime type detection for files"
     },
     "autoload": {
-        "psr-4": {
-            "App\\": "app",
-            "Config\\": "app/Config"
-        },
         "exclude-from-classmap": [
             "**/Database/Migrations/**"
         ]
```

Update Composer autoload config.

```
$ composer dump-autoload
```

## Update Paths

Update path configurations.

### foo

```diff
--- a/foo/app/Config/Constants.php
+++ b/foo/app/Config/Constants.php
@@ -23,7 +23,7 @@ defined('APP_NAMESPACE') || define('APP_NAMESPACE', 'App');
  | The path that Composer's autoload file is expected to live. By default,
  | the vendor folder is in the Root directory, but you can customize that here.
  */
-defined('COMPOSER_PATH') || define('COMPOSER_PATH', ROOTPATH . 'vendor/autoload.php');
+defined('COMPOSER_PATH') || define('COMPOSER_PATH', ROOTPATH . '../vendor/autoload.php');
 
 /*
  |--------------------------------------------------------------------------
```

```diff
--- a/foo/app/Config/Paths.php
+++ b/foo/app/Config/Paths.php
@@ -25,7 +25,7 @@ class Paths
      *
      * @var string
      */
-    public $systemDirectory = __DIR__ . '/../../vendor/codeigniter4/framework/system';
+    public $systemDirectory = __DIR__ . '/../../../vendor/codeigniter4/framework/system';
 
     /**
      * ---------------------------------------------------------------
```

### bar

```diff
--- a/bar/app/Config/Constants.php
+++ b/bar/app/Config/Constants.php
@@ -23,7 +23,7 @@ defined('APP_NAMESPACE') || define('APP_NAMESPACE', 'App');
  | The path that Composer's autoload file is expected to live. By default,
  | the vendor folder is in the Root directory, but you can customize that here.
  */
-defined('COMPOSER_PATH') || define('COMPOSER_PATH', ROOTPATH . 'vendor/autoload.php');
+defined('COMPOSER_PATH') || define('COMPOSER_PATH', ROOTPATH . '../vendor/autoload.php');
 
 /*
  |--------------------------------------------------------------------------
```

```diff
--- a/bar/app/Config/Paths.php
+++ b/bar/app/Config/Paths.php
@@ -25,7 +25,7 @@ class Paths
      *
      * @var string
      */
-    public $systemDirectory = __DIR__ . '/../../vendor/codeigniter4/framework/system';
+    public $systemDirectory = __DIR__ . '/../../../vendor/codeigniter4/framework/system';
 
     /**
      * ---------------------------------------------------------------
```
