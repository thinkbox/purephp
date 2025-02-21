# PurePHP Key-Value Storage
This is simple key-value storage written on PHP. It does not use files, or other database, just pure PHP.

## Installation
Via Composer:

```
composer require elfet/pure:~0.1
```

Via Phar file: [Download pure.phar](http://elfet.ru/purephp/pure.phar)

After downloading pure.phar run next commands:

```
mv pure.phar /usr/local/bin/pure
chmod +x /usr/local/bin/pure
```

You can update pure.phar by `pure update` command.

## Quick Guide
Start PurePHP by this command:

```
pure start &
```

Now PurePHP server is running. Run this command:

```
pure client
```

Now you can test PurePHP by simple commands like this:

```
> pure.queue.collection.push('hello')
bool(true)
> pure.queue.collection.push('world')
bool(true)
> pure.queue.collection.pop() ~ ' ' ~ pure.queue.collection.pop()
string(11) "hello world"
```

In pure console you can write commands on [Expression Language](https://github.com/symfony/expression-language). To exit from console type `exit` command.

To update PurePHP to latest version run command `pure update`.

## Documentation

### Connection to PurePHP server
```php
$port = 1337; // Default port value
$host = '127.0.0.1'; // Default host value
//...
$pure = new Pure\Clent($port, $host);
```

### Storages

PurePHP provide diffrent types on stogares. All supported storages are in [src/Storage](https://github.com/elfet/purephp/tree/master/src/Storage). You can access them by next methods and work with them like you work with them directly.

Every storage has separate collection namespace. So you can have for different storages same collection names.
You do not need to manualy create any collection. They will be automatically create at first access.

```php
$pure->of('collection')->...
$pure->stack('collection')->...
$pure->queue('collection')->...
$pure->prioriry('collection')->...
$pure->lifetime('collection')->...
```

Or you can access them by magic methods.

```php
$pure->of->collection->...
$pure->stack->collection->...
$pure->queue->collection->...
//...
```

### Array Storage `->of`

This is simple storage what uses php array to store your data. 
To store date in collection use `push` method:
```php
$pure->of('collection')->push(['hello' => 'world']);
```
Array Storage uses `array_merge` function.

To get value by key from collection use `get` method:
```php
$value = $pure->of('collection')->get('hello'); // will return 'world'.
```

To receive all elements use `all` method:
```php
$all = $pure->of('collection')->all();
```

To delete all elements use `clear` method:
```php
$all = $pure->of('collection')->clear();
```

You can check if key exist by `has` method, and delete element by `delete` method.

### Stack Storage `->stack`

This storage use `SplStack` to store your data.

You can use all `SplStack` methods and also `all`, `clear` methods.

### Queue Storage `->queue`

This storage use `SplQueue` to store your data.

You can use `SplQueue` methods and also `all`, `clear` methods.

`SplQueue` uses `enqueue` and `deenqueue` to push and pop from queue. In QueueStorage your can use `push` and `pop` methods to do this.

### Priority Queue Storage `->priority`

This storage use `SplPriorityQueue` to store your data.

You can use all `SplPriorityQueue` methods and also `all`, `clear` methods.

### Lifetime Storage `->lifetime`

In this storage you can store data which need to be deleted after some period. Life time in seconds.

```php
$pure->lifetime('collection')->set($key, $value, $lifetime);
```

You can get all elements by `all` method, check if key exist by `has` method, and delete element by `delete` method.

### Filtering

Every storage support function `filter`.

Example:

```php
// Get all elements that more than a 100.
$result = $pure->queue('collection')->filter('value > 100');

// Limit to 10.
$result = $pure->prioriry('collection')->filter('value > 100', 10);

// Complex expression.
$result = $pure->of('collection')->filter('value["year"] > 2000 and value["name"] matches "/term/"');
```

Filter rules writen on [Expression Language](https://github.com/symfony/expression-language) and cached on server. 
In expression available two variables: key and value.


## TODO

* Unit Tests
* Dump to file
* Load from file
* Replication 
* Sharding 
