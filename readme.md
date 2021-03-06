# Git ChangeLog

A simple service to parse the git log of an application to a readable changelog.

## Requirements
#### Mandatory Requirements
- [PHP](https://php.net) >= 5.6.4
- [Carbon](http://carbon.nesbot.com/) >= 1.18: Used to format the date of the git commits

#### Optional Requirements
- An existing [>= Laravel 5.3](https://laravel.com/docs/master/installation) project to use the global view variable `$gitVersion`

## Installation

1. To get started, install the Git ChangeLog service via the Composer package manager: 

    ```bash
    composer require epicarrow/git-changelog
    ```
    
2. Optional: If you are using Laravel and you want to use the global view variable `$gitVersion` add the following entry to your providers array in `config/app.php`:
    
    ```php
    'providers' => [
       ...
       ...
       EpicArrow\GitChangeLog\Providers\GitChangeLogServiceProvider::class
    ]
    ```

## Documentation
### Services
The following services are currently available:

___

```php 
EpicArrow\GitChangeLog\GitChangeLog::get([int $count = null])
``` 
Fetches the latest unique git commits. If two contiguous commits have the same commit message only one commit will be
retrieved.

**Parameters:**
- `$count` (_int_): The number of results to retrieve.

**Return Values:**

The retrieved commits as an `array` of `EpicArrow\GitChangeLog\Models\Commit`s.

___

```php 
EpicArrow\GitChangeLog\GitChangeLog::version()
``` 
Gets the latest version of the git repository.

**Return Values:**

The retrieved latest version of the git repository as a `string` or `null` if no version exists.

___

### Global Variables
If you are using Laravel and you've registered the `GitChangeLogServiceProvider` within your `config/app.php` providers array
you can access the following variables from every blade view:
- `$gitVersion`: Corresponds to the service `EpicArrow\GitChangeLog\GitChangeLog::version()` and gets you the latest version of the git repository.

### The commit model `EpicArrow\GitChangeLog\Models\Commit`
When retrieving the latest git commit through this service you will get an `array` of `EpicArrow\GitChangeLog\Models\Commit`s. This model has the following properties:
- `$id`(_string_): The commit hash/id
- `$date`(_Carbon\Carbon_): The date of the commit
- `$message`(_string_): The commit message
- `$version`(_string|null_): The version (tag) the commit belongs to
- `$author`(_string_): The author of the commit
- `$email`(_string_): The author's email address of the commit
- `$merge`(_string|null_): The merge info of the commit

