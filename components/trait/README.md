# Trait

## Introduction

The trait component provides multiple functions to retrieve information about traits.

!> The `Trait` component is not a replacement for [PHP Reflection API](https://php.net/manual/en/book.reflection.php).

## Usage

```php
use Psl;
use Psl\Trait;

trait FooTrait {}
trait BarTrait {}

Psl\invariant(true === Trait\defined(FooTrait::class), '"FooTrait" should be defined.');
Psl\invariant(true === Trait\defined(BarTrait::class), '"BarTrait" should be defined.');
Psl\invariant(false === Trait\defined(BazTrait::class), '"BazTrait" should not be defined.');
```

## API

### Functions

* `Trait\defined(string $trait_name): bool`
  
  Checks if the trait with the given name has already been defined.

  This function will return true if a trait exists, but has not been loaded yet.

  ```php
  use Psl\Trait;
  use Psl\IO;

  trait FooTrait {}
  trait BarTrait {}

  Trait\defined(FooTrait::class); // true
  Trait\defined(BarTrait::class); // true
  Trait\defined(BazTrait::class); // false
  Trait\defined(IO\ReadHandleConvenienceMethodsTrait::class); // false
  ```

* `Trait\exists(string $trait_name): bool`

  Checks if the trait with the given name exists.

  If the given trait is not defined, this function will attempt to load it.

  If the given trait is not defined and cannot be loaded, this function will return false.

  ```php
    use Psl\Trait;
    use Psl\IO;

    trait FooTrait {}
    trait BarTrait {}

    Trait\exists(FooTrait::class); // true
    Trait\exists(BarTrait::class); // true
    Trait\exists(BazTrait::class); // false
    Trait\exists(IO\ReadHandleConvenienceMethodsTrait::class); // true
    ```
