# Add schemaless attributes to Eloquent models

[![Latest Version on Packagist](https://img.shields.io/packagist/v/spatie/laravel-schemaless-attributes.svg?style=flat-square)](https://packagist.org/packages/spatie/laravel-schemaless-attributes)
[![Build Status](https://img.shields.io/travis/spatie/laravel-schemaless-attributes/master.svg?style=flat-square)](https://travis-ci.org/spatie/laravel-schemaless-attributes)
[![Quality Score](https://img.shields.io/scrutinizer/g/spatie/laravel-schemaless-attributes.svg?style=flat-square)](https://scrutinizer-ci.com/g/spatie/laravel-schemaless-attributes)
[![StyleCI](https://styleci.io/repos/132581720/shield?branch=master)](https://styleci.io/repos/132581720)
[![Total Downloads](https://img.shields.io/packagist/dt/spatie/laravel-schemaless-attributes.svg?style=flat-square)](https://packagist.org/packages/spatie/laravel-schemaless-attributes)

Wouldn't it be cool if you could just have a bit of the spirit of nosql available in Eloquent? This package does just that. It provides a trait that, when applied on a model, allows you to store arbritrary values in your model.

Here are a few examples

```php
// add and retrieve an attribute
$yourModel->schemaless_attributes->name = 'value';
$yourModel->schemaless_attributes->name; // returns 'value';

// you can also use the array approach
$yourModel->schemaless_attributes['name'] = 'value';
$yourModel->schemaless_attributes['name'] // returns 'value';

// setting multiple values in one go
$yourModel->schemaless_attributes = [
   'rey' => ['side' => 'light'], 
   'snoke' => ['side' => 'dark']
];

// retrieving values using dot notation
$yourModel->schemaless_attributes->get('rey.side'); // returns 'light';

// it has a scope to retrieve all models with the given schemaless attributes
$yourModel->withSchemalessAttributes(['name' => 'value', 'name2' => 'value2])->get();
```

## Requirements

This package requires a database with support for `json` columns like MySQL 5.7 or higher

## Installation

You can install the package via composer:

```bash
composer require spatie/laravel-schemaless-attributes
```

Add a migration for all models where you want to add schemaless attributes to. You can use `schemalessAttributes` method on `Blueprint` to add the right column.

```php
Schema::table('your_models', function (Blueprint $table) {
    $table->schemalessAttributes();
});
```

Finally add the `Spatie\SchemalessAttributes\HasSchemalessAttributes` to your model.

```php
use Illuminate\Database\Eloquent\Model;
use Spatie\SchemalessAttributes\HasSchemalessAttributes;

class YourModel extends Model
{
    use HasSchemalessAttributes;

    ...
}
```

## Usage

### Getting and setting schemaless attributes

This is the easiest way to get and set schemaless attributes.

```php
$yourModel->schemaless_attributes->name = 'value';
$yourModel->schemaless_attributes->name; // returns 'value';
```

Alternatively you can use an array approach.

```php
$yourModel->schemaless_attributes['name'] = 'value';
$yourModel->schemaless_attributes['name'] // returns 'value';
```

You can replace all schemaless_attributes by assigning an array to it.

```php
// all existing schemaless attributes will be replaced.
$yourModel->schemaless_attributes = ['name' => 'value'];
$yourModel->schemaless_attributes->all(); // returns ['name' => 'value']
```

You can also opt to use `get` and `set`. The methods have support for dot notation.

```
$yourModel->schemaless_attributes = [
   'rey' => ['side' => 'light'], 
   'snoke' => ['side' => 'dark']
];
$yourModel->schemaless_attributes->set('rey.side', 'dark');
$yourModel->schemaless_attributes->get('rey.side'); // returns 'dark
```

### Persisting schemaless attributes

To persist schemaless attributes you should, just like you do for normal attributes call `save()` on the model.

```php
$yourModel->save(); // saves both normal and schemaless attributes
```

### Retrieving models with specific schemaless attributes

The `HasSchemalessAttributes` trait provides a scopes to retrieve models with: `withSchemalessAttributes`.

```php
// returns all models that have all the given schemaless attributes
$yourModel->withSchemalessAttributes(['name' => 'value', 'name2' => 'value2])->get();
```

If you only want to search on a single custom attribute, you can use the scope like this

```php
// returns all models that have a schemaless attribute `name` set to `value`
$yourModel->withSchemalessAttributes('name', 'value')->get();

// to get a more natural feel there's also `withSchemalessAttribute` scope which is just an alias for `withSchemalessAttributes`
$yourModel->withSchemalessAttribute('name', 'value')->get();
```

### Testing

First create a mysql database named `laravel_schemaless_attributes`. After that you can run the tests with:

``` bash
composer test
```

### Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information what has changed recently.

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

### Security

If you discover any security related issues, please email freek@spatie.be instead of using the issue tracker.

## Postcardware

You're free to use this package, but if it makes it to your production environment we highly appreciate you sending us a postcard from your hometown, mentioning which of our package(s) you are using.

Our address is: Spatie, Samberstraat 69D, 2060 Antwerp, Belgium.

We publish all received postcards [on our company website](https://spatie.be/en/opensource/postcards).

## Credits

- [Freek Van der Herten](https://github.com/freekmurze)
- [All Contributors](../../contributors)

## Support us

Spatie is a webdesign agency based in Antwerp, Belgium. You'll find an overview of all our open source projects [on our website](https://spatie.be/opensource).

Does your business depend on our contributions? Reach out and support us on [Patreon](https://www.patreon.com/spatie). 
All pledges will be dedicated to allocating workforce on maintenance and new awesome stuff.

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
