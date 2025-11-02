# 🐦 Angry bearded

<img src="resources/angry-bearderd.png" align="left" width="128" alt="logo"/>

This tool is a pre-configured [GrumPHP](https://github.com/phpro/grumphp) for Drupal 10 & 11 projects.

The rules are intentionally strict — feel free to customize the .dist files to suit your needs.

A `.editorconfig` file and a Qodana template are also included.

## 🔧 Included tools
- ComposerRequireChecker
- Drupal Coder
- PHP Assumptions (phpa)
- PHP Copy/Paste Detector (phpcpd)
- PHP Magic Number Detector (phpmnd)
- PHP Parallel Lint
- PHPMD
- PHPStan
  - PHPStan Banned Code
  - PHPStan Deprecation Rules
  - PHPStan Extension Installer
  - phpstan-drupal
  - Dead code detector for PHP
- PHP_CodeSniffer
  - PHP Compatibility Coding Standard for PHP CodeSniffer
  - Slevomat Coding Standard
- PhpDeprecationDetector (phpdd)
- PHPUnit
- Twigcs
  - Twig CS Fixer
- composer-normalize

## 🚀 Installation

> [!IMPORTANT]  
> This tool is compatible with multiple versions of Drupal as well as multiple versions of PHP.
> 
> Choose the appropriate release for your situation using the table below:
>
>| 	             | **Drupal 10** 	 | **Drupal 11** 	 | **Drupal 12** 	 |
>|---------------|-----------------|-----------------|-----------------|
>| **PHP 8.1** 	 | `1.6.*`       	 | NA            	 | NA            	 |
>| **PHP 8.2** 	 | `1.7.*`       	 | NA            	 | NA            	 |
>| **PHP 8.3** 	 | `2.*`         	 | `2.*`         	 | NA            	 |
>| **PHP 8.4** 	 | `2.*`         	 | `2.*`         	 | NA           	 |
>| **PHP 8.5** 	 | NA 	           | `3.*` 	         | `3.*` 	         |

### ➡️ When using drupal/core-composer-scaffold (recommended)

In most cases, you would already be using the [`drupal/core-composer-scaffold`](https://packagist.org/packages/drupal/core-composer-scaffold) package if you have set up using the latest Drupal templates. This package uses `core-composer-scaffold` to set up configuration files in your project. To make this work, add this package name in your composer's `extra.drupal-scaffold.allowed-packages` section.

```json
 "name": "my/project",
  ...
  "extra": {
    "drupal-scaffold": {
      "allowed-packages": [
        "johnatas-x/angry-bearded"
      ],
      ...
    }
  }
```

Now, run `composer require` to include the package in your application. Since the package is now allowed, the `core-composer-scaffold` package will copy the configuration files.

```bash
# Replace "2.*" with your compatible release (see below).
composer require --dev johnatas-x/angry-bearded:2.*
```

#### 💡 More about scaffolding

As described before, this package uses [`drupal/core-composer-scaffold`](https://github.com/drupal/core-composer-scaffold) plugin to scaffold a few files to the project root. While not strictly required, it’s likely already part of your setup if you're building a Drupal site.

The scaffolding process runs during every Composer operation and may overwrite files. Only the file `grumphp.yml.dist` is not overwritten during later operations. If you are customizing any of the other configuration files and don't want the updates to overwrite your changes, you can override the behavior in your composer.json file. For example, to skip `phpmd.xml.dist` from being overwritten, add this to your `composer.json`:

```json
  "name": "my/project",
  ...
  "extra": {
    "drupal-scaffold": {
      "file-mapping": {
        "[project-root]/phpmd.xml.dist": false
      }
    }
  }
```

For more details, read the ["Excluding Scaffold files"](https://github.com/drupal/core-composer-scaffold#excluding-scaffold-files) section of the [documentation](https://github.com/drupal/core-composer-scaffold/blob/8.8.x/README.md) for the core-composer-scaffold plugin.

### ➡️ Without drupal/core-composer-scaffold

If you're not using the scaffolding plugin, the package won't automatically copy the configuration files.

First, run `composer require` to include the package in your application.

```bash
# Replace "2.*" with your compatible release (see below).
composer require --dev johnatas-x/angry-bearded:2.*
```

If you don't already have a `grumphp.yml` file in your project, GrumPHP will prompt you to create one — select "No."

Then, copy all `*.dist` files from the package to your project root. The files copied are:

* .editorconfig.dist
* grumphp.yml.dist
* phpcs.xml.dist
* phpmd.xml.dist
* phpstan.neon.dist
* phpstan-drupal.neon.dist
* qodana.yaml.dist

> [!NOTE]
> For deprecated checks only with PHPStan Drupal, copy `phpstan-drupal-deprecated.neon.dist` instead of both `phpstan.neon.dist` and `phpstan-drupal.neon.dist`.

> [!TIP]
> To copy all files, you can run:
> ```bash
> cp vendor/johnatas-x/angry-bearded/*.dist .
> ```

## 🚧 Usage

No additional setup is required. However, if Git hooks aren’t triggering, run: `php ./vendor/bin/grumphp git:init`.

For additional commands, look at [grumhp's documentation](https://github.com/phpro/grumphp/blob/master/doc/commands.md).

## 💈 Customising

* Most customizations start by copying the `grumphp.yml.dist` file to your project root. Make sure you have the file.
* By default, the Drupal logo is used in successful outputs, but you can use GrumPHP's logo modifying `ascii` in `grumphp.yml` or using your own logo.

### ➕ Adding tasks

You can add and customize various tasks in your `grumphp.yml`.

Read the [online documentation for GrumPHP tasks](https://github.com/phpro/grumphp/blob/master/doc/tasks.md) to see the tasks you can use and configure.

### 🛂 Forcing commit message format

To configure the commit message structure, use the [git_commit_message task](https://github.com/phpro/grumphp/blob/master/doc/tasks/git_commit_message.md). For example, to enforce the commit message contains the YouTrack issue ID, use the rule as the following snippet. More options are [documented online](https://github.com/phpro/grumphp/blob/master/doc/tasks/git_commit_message.md).

```yaml
# grumphp.yml
grumphp:
  tasks:
    git_commit_message:
      matchers:
        Must contain issue number: /YouTrack #\d+/
```

### 🚫 Disable commit banners

GrumPHP supports banners to celebrate (or scold) on your commit. This is fun, but it is possible it gets on your nerves. If you don’t want it, edit the grumphp.yml file and replace the following parameters:

```yaml
# grumphp.yml
grumphp:
    ascii: ~
```

You could even disable specific ones like this:

```yaml
# grumphp.yml
grumphp:
    ascii:
        succeeded: ~
```
