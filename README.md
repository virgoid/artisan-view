![artisan-view](https://cloud.githubusercontent.com/assets/11269635/14457826/a3bde82a-00ad-11e6-8161-0c218937156a.jpg)

# Artisan View
[![Latest Version on Packagist][ico-version]][link-packagist]
[![Total Downloads][ico-downloads]][link-downloads]
[![Software License][ico-license]](LICENSE.md)
[![Build Status][ico-tests]][link-tests]
[![StyleCI][ico-styleci]][link-styleci]
[![PhpStan][ico-phpstan]][link-phpstan]

This package adds a handful of view-related commands to Artisan in your Laravel
project. Generate blade files that extend other views, scaffold out sections
to add to those templates, and more. All from the command line we know and love!

## Index
- [Installation](#installation)
  - [Downloading](#downloading)
  - [Registering the service provider](#registering-the-service-provider)
- [Usage](#usage)
  - [Creating a view](#creating-a-view)
  - [The file extension](#the-file-extension)
  - [Extending a view](#extending-a-view)
  - [Adding sections](#adding-sections)
  - [Creating RESTful resources](#creating-restful-resources)
  - [Scrapping a view](#scrapping-a-view)
  - [Scrapping resources](#scrapping-resources)
- [Contributing](#contributing)
- [License](#license)

## Installation
You'll have to follow a couple of simple steps to install this package.

### Downloading
Via [composer](http://getcomposer.org):

```bash
$ composer require sven/artisan-view --dev
```

Or add the package to your development dependencies in `composer.json` and run
`composer update sven/artisan-view` to download the package:

```json
{
    "require-dev": {
        "sven/artisan-view": "^3.1"
    }
}
```

### Registering the service provider
If you're using Laravel 5.5, you can skip this step. The service provider will have already been registered
thanks to auto-discovery. 

## Usage
When running `php artisan` now, you will see two new commands in the list: `make:view` and `scrap:view`.

### Creating a view
To create a single view file, you can invoke the `make:view` command:

```bash
$ php artisan make:view dashboard # Creates 'dashboard.blade.php'
$ php artisan make:view users.create # Creates 'users/create.blade.php'
```

### The file extension
This package defaults to using `.blade.php` as its file extension, but you may change that by specifying the
`--extension` option:

```bash
$ php artisan make:view dashboard --extension=html.twig # Creates 'dashboard.html.twig'
```

### Extending a view
If you want to pre-fill the generated view file with an `@extends` tag, use the `--extends` option:

```bash
$ php artisan make:view dashboard --extends=layouts.master
```

<details>
<summary>See result</summary>
    
```blade
@extends('layouts.master')
    
```
</details>

### Adding sections
To add sections to a view, you can use the `--section` option:

```bash
$ php artisan make:view posts.create --section=content
```

<details>
<summary>See result</summary>

```blade
@section('content')

@endsection

```
</details>

You can even add inline sections (and pre-fill them) by using a colon (`:`) in the
name of the section:

```bash
$ php artisan make:view posts.create --section=title:Awesome
$ php artisan make:view posts.edit --section="title:Edit the post" # Use quotes if you want to add spaces
```

<details>
<summary>See result</summary>

```blade
@section('title', 'Awesome')

```

```blade
@section('title', 'Edit the post')

```
</details>

You can add multiple sections to the view by using the `--section` option multiple times:

```bash
$ php artisan make:view posts.show --section="title:Show an existing post" --section=content
```

<details>
<summary>See result</summary>

```blade
@section('title', 'Show an existing post')

@section('content')

@endsection

```
</details>

### Creating RESTful resources
You can use the `make:view` command to create 4 views (one for each logical RESTful verb) in one go. To do so, use the
`--resource` option:

```bash
$ php artisan make:view posts --resource
```

This will create 4 views: `posts/index.blade.php`, `posts/show.blade.php`, `posts/create.blade.php`, and 
`posts/edit.blade.php`. This command can be used in combination with the other options described above.

You may want to only create a subset of the verbs. To do this, a `--verb` option is available. This is an array
of names for the view to create:

```bash
$ php artisan make:view posts --resource --verb=index --verb=edit
```

This will only create 2 views: `posts/index.blade.php` and `posts/edit.blade.php`.

### Scrapping a view
You may use the `scrap:view` command to remove one or more views from your project:

```bash
$ php artisan scrap:view index # Removes 'index.blade.php'
$ php artisan scrap:view posts.show # Removes 'posts/show.blade.php'
```

This will ask you for confirmation. If you are sure you want to remove the view, you
can pass the `--force` flag: 

```bash
$ php artisan scrap:view index --force # Do not ask for confirmation before removing 'index.blade.php'
```

### Scrapping resources
Just like with `make:view`, you can use the `--resource` and `--verb` flags to scrap (parts of) a RESTful
resource of views:

```bash
$ php artisan scrap:view posts --resource
```

This will remove `index.blade.php`, `show.blade.php`, `edit.blade.php`, and `create.blade.php` from
`posts/`. If this directory is empty after deleting these views, it will also be removed.

You may wish to only remove part of a resource. You can do so with the `--verb` flag:

```bash
$ php artisan scrap:view posts --resource --verb=index --verb=show
```

This will remove `posts/index.blade.php` and `posts/show.blade.php`. If the `posts/` directory is
empty after deleting these views, it will also be deleted.

## Contributing
All contributions (in the form on pull requests, issues and feature-requests) are
welcome. See the [contributors page](../../graphs/contributors) for all contributors.

## License
`sven/artisan-view` is licenced under the MIT License (MIT). Please see the
[license file](LICENSE.md) for more information.

[ico-version]: https://img.shields.io/packagist/v/sven/artisan-view.svg
[ico-license]: https://img.shields.io/badge/license-MIT-green.svg
[ico-downloads]: https://img.shields.io/packagist/dt/sven/artisan-view.svg
[ico-tests]: https://github.com/svenluijten/artisan-view/workflows/Tests%20(PHP)/badge.svg
[ico-styleci]: https://styleci.io/repos/56054783/shield?style=flat
[ico-phpstan]: https://img.shields.io/badge/phpstan-enabled-blue.svg

[link-packagist]: https://packagist.org/packages/sven/artisan-view
[link-downloads]: https://packagist.org/packages/sven/artisan-view
[link-tests]: https://github.com/svenluijten/artisan-view/actions?query=workflow%3ATests%20(PHP)
[link-styleci]: https://styleci.io/repos/56054783
[link-phpstan]: https://github.com/phpstan/phpstan
