# Interface

## Introduction

The interface component provides a set functions to retrieve information about interfaces.

!> The `Interface` component is not a replacement for [PHP Reflection API](https://php.net/manual/en/book.reflection.php).

## Usage

```php
use Psl;
use Psl\Interface;

interface FooInterface {}
interface BarInterface {}

Psl\invariant(true === Interface\defined(FooInterface::class), '"FooInterface" should be defined.');
Psl\invariant(true === Interface\defined(BarInterface::class), '"BarInterface" should be defined.');
Psl\invariant(false === Interface\defined(BazInterface::class), '"BazInterface" should not be defined.');
```

## API

### Functions

<div class="api-functions">

* [`Interface\defined(string $interface_name): bool` php]
  
  Checks if the interface with the given name has already been defined.

  This function will return true if a interface exists, but has not been loaded yet.

  * [`$interface_name` php]: The name of the interface to check.

  ```php
  use Psl\Interface;
  use Psl\IO;

  interface FooInterface {}
  interface BarInterface {}

  Interface\defined(FooInterface::class); // true
  Interface\defined(BarInterface::class); // true
  Interface\defined(BazInterface::class); // false
  Interface\defined(IO\ReadHandleInterface::class); // false
  ```

* [`Interface\exists(string $interface_name): bool` php]

  Checks if the interface with the given name exists.

  If the given interface is not defined, this function will attempt to load it.

  If the given interface is not defined and cannot be loaded, this function will return false.

  * [`$interface_name` php]: The name of the interface to check.

  ```php
    use Psl\Interface;
    use Psl\IO;

    interface FooInterface {}
    interface BarInterface {}

    Interface\exists(FooInterface::class); // true
    Interface\exists(BarInterface::class); // true
    Interface\exists(BazInterface::class); // false
    Interface\exists(IO\ReadHandleInterface::class); // true
    ```

</div>