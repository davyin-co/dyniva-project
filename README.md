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
* -- Creates default writable versions of `settings.php` and `services.yml` --
* 创建 `docroot/sites/default/files` directory.
* 最新版本的drupal console安装在 `bin/drupal`.
* 删除项目非根目录下的.git(因为有些模块通过branch引入的，如果不删除会生成.git文件，导致git提交的时候为git submodule，这样在部署的时候会带来很多麻烦）
* 安装ergebnis/composer-normalize插件，在执行composer update/install之后自动会composer.json进行格式化（使用4个空格的缩进）






## FAQ

### 补丁管理
补丁使用[cweagans/composer-patches](https://github.com/cweagans/composer-patches)来管理。使用本项目的补丁通过composer.json引入，强烈不建议通过单独的补丁文件管理。
例如，给drupal core打补丁，可以参考如下格式：
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

### Drupal 内核版本说明

该项目不限定Drupal内核版本，版本受dyniva约束。具体可以查看[Dyniva](https://github.com/davyin-co/dyniva)说明。

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

### 如何指定PHP版本？

To prevent this you can add this code to specify the PHP version you want to use in the `config` section of `composer.json`:
```json
"config": {
    "sort-packages": true,
    "platform": {
        "php": "7.2.22"
    }
},
```
