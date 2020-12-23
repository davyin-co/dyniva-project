# 介绍

[Composer](https://getcomposer.org/) 模版，初始化话dyniva发行版.

更多细节查看[dyniva](https://github.com/davyin-co/dyniva)

## 使用

先安装 [composer](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-osx).


安装成功后创建项目:
```
composer create-project davyin/dyniva-project dyniva-project --stability dev  --no-interaction
```

如果需要引入其他drupal 模块/主题，使用 `composer require ...` 下载安装:

```
cd dyniva-project
composer require "drupal/devel:1.x-dev"
```

The `composer create-project` command passes ownership of all files to the
project that is created. You should create a new git repository, and commit
all files not excluded by the .gitignore file.

## 该项目做了什么？

* Drupal 安装在 `docroot` 目录.
* drupal官方模块 (packages of type `drupal-module`) are placed in `docroot/modules/contrib/`
* dyniva自定义模块 (packages of type `drupal-custom-module`) are placed in `docroot/modules/custom/`
* drupal官方主题(packages of type `drupal-theme`) are placed in `docroot/themes/contrib/`
* dyniva自定义主题(packages of type `drupal-custom-theme`) are placed in `docroot/themes/custom/`
* Profiles (packages of type `drupal-profile`) are placed in `docroot/profiles/contrib/`
* dyniva自定义Profiles (packages of type `drupal-custom-profile`) are placed in `docroot/profiles/custom/`
* - Creates default writable versions of `settings.php` and `services.yml` -
* 创建 `docroot/sites/default/files` directory.
* - Latest version of DrupalConsole is installed locally for use at `bin/drupal`. -
* 删除项目非根目录下的.git(因为有些模块通过branch引入的，如果不删除会生成.git文件，导致git提交的时候为git submodule，这样在部署的时候会带来很多麻烦）

## 补丁管理
补丁使用[cweagans/composer-patches](https://github.com/cweagans/composer-patches)来管理。使用本项目的补丁通过composer.json引入，强烈不建议通过单独的补丁文件管理

## Updating Drupal Core

This project will attempt to keep all of your Drupal Core files up-to-date; the
project [drupal/core-composer-scaffold](https://github.com/drupal/core-composer-scaffold)
is used to ensure that your scaffold files are updated every time drupal/core is
updated. If you customize any of the "scaffolding" files (commonly .htaccess),
you may need to merge conflicts if any of your modified files are updated in a
new release of Drupal core.

Follow the steps below to update your core files.

1. Run `composer update drupal/core drupal/core-dev --with-dependencies` to update Drupal Core and its dependencies.
2. Run `git diff` to determine if any of the scaffolding files have changed.
   Review the files for any changes and restore any customizations to
  `.htaccess` or `robots.txt`.
1. Commit everything all together in a single commit, so `web` will remain in
   sync with the `core` when checking out branches or running `git bisect`.
1. In the event that there are non-trivial conflicts in step 2, you may wish
   to perform these steps on a branch, and use `git merge` to combine the
   updated core files with your customized files. This facilitates the use
   of a [three-way merge tool such as kdiff3](http://www.gitshah.com/2010/12/how-to-setup-kdiff-as-diff-tool-for-git.html). This setup is not necessary if your changes are simple;
   keeping all of your modifications at the beginning or end of the file is a
   good strategy to keep merges easy.

## FAQ

### Should I commit the contrib modules I download?

Composer recommends **no**. They provide [argumentation against but also
workrounds if a project decides to do it anyway](https://getcomposer.org/doc/faqs/should-i-commit-the-dependencies-in-my-vendor-directory.md).

### Should I commit the scaffolding files?

The [Drupal Composer Scaffold](https://github.com/drupal/core-composer-scaffold) plugin can download the scaffold files (like
index.php, update.php, …) to the web/ directory of your project. If you have not customized those files you could choose
to not check them into your version control system (e.g. git). If that is the case for your project it might be
convenient to automatically run the drupal-scaffold plugin after every install or update of your project. You can
achieve that by registering `@composer drupal:scaffold` as post-install and post-update command in your composer.json:

```json
"scripts": {
    "post-install-cmd": [
        "@composer drupal:scaffold",
        "..."
    ],
    "post-update-cmd": [
        "@composer drupal:scaffold",
        "..."
    ]
},
```
### How can I apply patches to downloaded modules?

If you need to apply patches (depending on the project being modified, a pull
request is often a better solution), you can do so with the
[composer-patches](https://github.com/cweagans/composer-patches) plugin.

To add a patch to drupal module foobar insert the patches section in the extra
section of composer.json:
```json
"extra": {
    "patches": {
        "drupal/foobar": {
            "Patch description": "URL to patch"
        }
    }
}
```

### How can I add js/css libraries using composer.json?

It is possible to use frontend libraries with composer thanks to the
asset-packagist repository (https://asset-packagist.org/).

For example, to use colorbox:
```
composer require npm-asset/colorbox:"^0.4"

```
Composer will detect new versions of the library that meet your constraints.
In the above example it will download anything from 0.4.* series of colorbox.

When managing libraries with composer this way, you may not want to add it to
version control. In that case, add specific directories to the .gitignore file.
```
# Specific libraries (which we manage with composer)
web/libraries/colorbox
```

For more details, see https://asset-packagist.org/site/about

### How do I specify a PHP version ?

This project supports PHP 7.0 as minimum version (see [Drupal 8 PHP requirements](https://www.drupal.org/docs/8/system-requirements/drupal-8-php-requirements)), however it's possible that a `composer update` will upgrade some package that will then require PHP 7+.

To prevent this you can add this code to specify the PHP version you want to use in the `config` section of `composer.json`:
```json
"config": {
    "sort-packages": true,
    "platform": {
        "php": "7.0.33"
    }
},
```
