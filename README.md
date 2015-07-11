Laravel Tag Plugin
============

[![Latest Stable Version](https://poser.pugx.org/rtconner/laravel-tagging/v/stable.svg)](https://packagist.org/packages/rtconner/laravel-tagging)
[![Total Downloads](https://poser.pugx.org/rtconner/laravel-tagging/downloads.svg)](https://packagist.org/packages/rtconner/laravel-tagging)
[![License](https://poser.pugx.org/rtconner/laravel-tagging/license.svg)](https://packagist.org/packages/rtconner/laravel-tagging)
[![Build Status](https://travis-ci.org/rtconner/laravel-tagging.svg?branch=master)](https://travis-ci.org/rtconner/laravel-tagging)

This package is not meant to handle javascript or html in any way. This package handles database storage and read/writes only.

There are no real limits on what characters can be used in a tag. It uses a slug transform to determine if two tags are identical ("sugar-free" and "Sugar Free" would be treated as the same tag). Tag display names are run through Str::title()

[Laravel 5 Documentation](https://github.com/rtconner/laravel-tagging/tree/laravel-5)  
[Laravel 4 Documentation](https://github.com/rtconner/laravel-tagging/tree/laravel-4)

#### Composer Install (for Laravel 5)
	
```shell
composer require rtconner/laravel-tagging "~1.0.7"
```

#### Install and then Run the migrations

The service provider does not load on every page load, so it should not slow down your app.

```php
'providers' => array(
	'Conner\Tagging\TaggingServiceProvider',
);
```
```bash
php artisan vendor:publish --provider="Conner\Tagging\TaggingServiceProvider"
php artisan migrate
```

After these two steps are done, you can edit config/tagging.php with your prefered settings.
	
#### Setup your models
```php
class Article extends \Illuminate\Database\Eloquent\Model {
	use \Conner\Tagging\TaggableTrait;
}
```

#### Quick Sample Usage

```php
$article = Article::with('tagged')->first(); // eager load

foreach($article->tagged as $tagged) {
	echo $tagged->name . ' with url slug of ' . $tagged->tag_slug;
}

$article->tag('Gardening'); // attach the tag

$article->untag('Cooking'); // remove Cooking tag
$article->untag(); // remove all tags

$article->retag(array('Fruit', 'Fish')); // delete current tags and save new tags

$tagged = $article->tagged; // return Collection of rows tagged to article
$tags = $article->tags; // return Collection the actual tags (is slower than using tagged)

$article->tagNames(); // get array of related tag names	

Article::withAnyTag('Gardening, Cooking')->get() // fetch articles with any tag listed
Article::withAnyTag(array('Gardening','Cooking'))->get() // different sytax same result as above

Article::withAllTags('Gardening, Cooking')->get() // only fetch articles with all the tags

Conner\Tagging\Tag::where('count', '>', 2)->get(); // return all tags used more than twice

Article::existingTags(); // return collection of all existing tags on any articles
```
### Configure

[See config/tagging.php](config/tagging.php) for configuration options.

#### Upgrading Laravel 4 to 5

This library stores full model class names into the database. When you upgrade laravel and you add namespaces to your models, you will need to update the records stored in the database.
Alternatively you can override Model::$morphClass on your model class to match the string stored in the database.

#### Credits

 - Robert Conner - http://smartersoftware.net

#### Further Reading

 - 3rd Party Posting on installation with Twitter Bootstrap 2.3 : http://blog.stickyrice.net/archives/2015/laravel-tagging-bootstrap-tags-input-rtconner/
