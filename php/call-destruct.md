# When does PHP call __destruct()?

- [Original article by Mohamed Said](https://divinglaravel.com/when-does-php-call-__destruct)

[BAck to PHP tips](#./php.md#php-tips)

In PHP,
`__construct()
` is called while creating an object and
`__destruct()
` is called while the object is being removed from memory. Using this knowledge, we can create more fluent APIs as demonstrated by Freek Van der Herten in this short video.

Now let's see when PHP calls
`__destruct()
` exactly.

An object is removed from memory if you explicitly remove it:

```php
$object = new Object();

unset($object); // __destruct will be called immediately.

$object = null; // __destruct will be called immediately.
```

It's also called when the scope where the object live is about to be terminated, for example at the end of a controller method:

```php
function store(Request $request)
{
   $object = new Object();

   User::create(...);

   // __destruct will be called here.

   return view('welcome');
}
```

Even if we're within a long running process, queued job for example, __destruct will be called before the end of the handle method:

```php
function handle()
{
   $object = new Object();

   User::create(...);

   // __destruct will be called here.
}
```

It'll also be called when the script is being terminated:

```php
function handle()
{
   $object = new Object();

   User::create(...);

   // __destruct will be called here.

   exit(1);
}
```
