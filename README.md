[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![MIT License][license-shield]][license-url]

<br />
<p align="center">

  <h3 align="center">Akvo Flow PHP SDK</h3>

  <p align="center">
  This package generates <b>Database Migrations</b> and <b>ORM Models</b>. <br/>Easily to use for developing custom website based on <a href="https://akvo.org/capture-and-understand-data-that-matters/">Akvo Flow</a> data.
    <br />
    <a href="https://github.com/akvo/akvo-flow-php-sdk/issues">Report Bug</a>
    ·
    <a href="https://github.com/akvo/akvo-flow-php-sdk/issues">Request Feature</a>
    ·
    <a href="https://github.com/akvo/akvo-flow-php-sdk/issues">Request Consultancy</a>
  </p>
</p>

<!-- GETTING STARTED -->

## System requirements

You must have the following tools on the command line of your *host operating system*:

* [Git](https://git-scm.com/)
* [PHP 7.3+](http://php.net/manual/en/install.php)
* [Laravel 7+](https://laravel.com/docs/8.x/installation)

### Prerequisites

To quickly install Composer in the current directory, run the following script in your terminal.

1. Install Composer

```sh
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '8a6138e2a05a8c28539c9f0fb361159823655d7ad2deecb371b04a83966c61223adc522b0189079e3e9e277cd72b8897') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```
2. Create new Laravel Project

```sh
composer create-project --prefer-dist laravel/laravel <PROJECT_NAME>
```

## Installation

Create your new Laravel project then add the package to `composer.json`.
```json
{
    "require": {
        "akvo/akvo-flow-php-sdk": "1.0.*"
    }
}
```
```bash
composer install
```

or you can also directly run the composer command:

```bash
composer require akvo/akvo-flow-php-sdk
```

## Usage

### Setup .env file

In your project simply add the following environment variables to start using Akvo Flow API

```
AKVOFLOW_AUTH_URL='https://akvo.auth0.com/oauth/token'
AKVOFLOW_CLIENT_ID=''
AKVOFLOW_API_URL='https://api-auth0.akvo.org/flow/orgs'
AKVOFLOW_INSTANCE='organisation_subdomain'
AKVOFLOW_USERNAME='youremail@gmail.org'
AKVOFLOW_PASSWORD=''
AKVOFLOW_FORM_URL='https://tech-consultancy.akvo.org/akvo-flow-web-api'
```

More details:

- [Akvo Flow Sync API](https://github.com/akvo/akvo-flow-api/wiki/Akvo-Flow-Sync-API)
- [Akvo Flow REST API](https://github.com/akvo/akvo-flow-api/wiki/Akvo-Flow-REST-API)


### Running Commands

To view a list of all available Akvo Flow commands, you may use the list command with [`php artisan` -  Laravel Artisan](https://laravel.com/docs/8.x/artisan).
- `akvo:migrate`, Migrate DB Schema to store Akvo Flow contents.
- `akvo:seed`, Seed Database with Akvo Flow data via [Akvo Flow REST API](https://github.com/akvo/akvo-flow-api/wiki/Akvo-Flow-REST-API)

### Database Schema

Once you run `php artisan akvo:migrate` you will see several tables migrated to your Database.

### Eloquent ORM

The Eloquent ORM included with the package provides a simple ActiveRecord implementation for working with your Akvo Flow database. Each database table has a corresponding "Model" which is used to interact with the table. You can load all the **[Akvo Flow Models](https://github.com/akvo/akvo-flow-php-sdk/tree/master/src/Models)** directly to your Controller.

```php
use Akvo\Models\Survey;
use Akvo\Models\Answer;

public function surveyAndForms(Survey $surveys)
{
  return $surveys->with('forms');
}

public function answerAndForm(Answer $answer)
{
  return $answer->load('question.form');
}
```

If you wish to extend different Schema to Models, you could also extend them into your Model directory (Laravel 7+).

Before:

```json
[{
  "id":4310019,
  "name": "Just an example survey (Test)",
  "registration_id": 24390001,
  "path": "/Farmfit surveys/Cases 2019/Agri-Wallet (Kenya)/Survey Agri-Wallet",
  "created_at": "2020-09-10T19:17:22",
  "updated_at": "2020-09-10T19:17:22"
}]
```

Extend `App\Model`, adding new object named __short__:

```php
Namespace App\Model;

use Illuminate\Support\Str;
use Akvo\Models\Survey as AkvoSurvey;

class Survey extends AkvoSurvey
{
  $protected $appends = ['short'];
  
  public function getShortAttribute()
  {
        $text = trim(preg_replace('!\s+-!', ' -', $this->name));
        $text = Str::beforeLast($text, ' (');
        return Str::upper($text);
  }
}

```

Results:

```json
[{
  "id":4310019,
  "name": "Just an example survey (Test)",
  "registration_id": 24390001,
  "path": "/Farmfit surveys/Cases 2019/Agri-Wallet (Kenya)/Survey Agri-Wallet",
  "short": "Just an example survey",
  "created_at": "2020-09-10T19:17:22",
  "updated_at": "2020-09-10T19:17:22"
}]
```

### Rollback

To roll back the latest migration operation, you may have to run `php artisan migrate:reset` or you could also re-run `php artisan akvo:migrate`.

## About Akvo

Akvo is a not-for-profit internet and software developer, headquartered in Amsterdam, Netherlands. The foundation specializes primarily in building and operating data collection and visualization systems to be used in international development and aid activity.

### Akvo Flow

[Akvo Flow](http://akvo.org/products/akvoflow/) is a tool for collecting, evaluating and displaying of geographically referenced data. It is composed of an [android mobile app](https://github.com/akvo/akvo-flow-mobile/) and an online web-based platform. This repository contains code for the web-based platform that comprises a [backend engine](https://github.com/akvo/akvo-flow/tree/master/GAE) and a [dashboard user interface](https://github.com/akvo/akvo-flow/tree/master/Dashboard).  Alongside the dashboard and mobile apps, is a [data import and export component](https://github.com/akvo/akvo-flow-services).

### Akvo Tech Consultancy

Akvo offers data consultancy and a digital platform, to support our partner's design their projects with building something on-top, the solutions are built with robust products like Akvo’s as the core workhorse, and then a layer of customisations which goes sufficiently close to aligning with the partner requirements.

<!-- MARKDOWN LINKS & IMAGES -->
[contributors-shield]: https://img.shields.io/github/contributors/akvo/akvo-flow-php-sdk.svg?style=flat-square
[contributors-url]: https://github.com/akvo/akvo-flow-php-sdk/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/akvo/akvo-flow-php-sdk.svg?style=flat-square
[forks-url]: https://github.com/akvo/akvo-flow-php-sdk/network/members
[stars-shield]: https://img.shields.io/github/stars/akvo/akvo-flow-php-sdk.svg?style=flat-square
[stars-url]: https://github.com/akvo/akvo-flow-php-sdk/stargazers
[issues-shield]: https://img.shields.io/github/issues/akvo/akvo-flow-php-sdk.svg?style=flat-square
[issues-url]: https://github.com/akvo/akvo-flow-php-sdk/issues
[license-shield]: https://img.shields.io/github/license/akvo/akvo-flow-php-sdk.svg?style=flat-square
[license-url]: https://github.com/akvo/akvo-flow-php-sdk/blob/master/LICENSE.txt

