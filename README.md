# LKT Hooks

# Installation

```shell
composer require lkt/hooks
```

# Register hooks

```php
use Lkt\Hooks\Hook;

Hook::register('hook-name', 'hook-action', [callable])
```

For example:

```php
use Lkt\Hooks\Hook;

Hook::register('create-user', 'mail-set-up-password', [callable])
Hook::register('create-user', 'mail-admin-new-user-created', [callable])
```

Every action can be registered as many times as you need. All results will be returned when triggered.

The only limitation it's all action handler must have the same method definition.

For example:


```php
class HookStack {

    public static function mailPassword(int $userId, string $name, string $lastname): bool
    {
        // ... your stuff
        return true;
    }

    // If we want to define another method with different code,
    // the method must have the same arguments and return type.
    // This is a valid method:
    public static function mailPassword2(int $userId, string $name, string $lastname): bool
    {
        // ... another stuff
        return true;
    }

    // This is an invalid method:
    public static function mailPassword3(int $userId, string $name): bool
    {
        // ... third stuff
        return true;
    }
}
use Lkt\Hooks\Hook;

Hook::register('create-user', 'mail-set-up-password', [HookStack::class, 'mailPassword'])
Hook::register('create-user', 'mail-set-up-password', [HookStack::class, 'mailPassword2'])
Hook::register('create-user', 'mail-set-up-password', [HookStack::class, 'mailPassword3'])
```


# Trigger a hook

```php
use Lkt\Hooks\Hook;

Hook::run('hook-name', ...$args)
```

For example:

```php
use Lkt\Hooks\Hook;

$id = 1;
$name = 'John';
$lastname = 'Doe';

$response = Hook::register('create-user', $id, $name, $lastname);
```

The previous example will return a `HookResponse` object containing the result of all triggers called.