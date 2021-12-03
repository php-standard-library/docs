# Class

## Introduction

The class component provides multiple functions to retrieve information about a class.

!> The `Class` component is not a replacement for [PHP Reflection API](https://php.net/manual/en/book.reflection.php).

## Usage

```php
use Psl;
use Psl\Class;

abstract class Foo {
  public function bar(): void {}
}

Psl\invariant(true === Class\is_abstract(Foo::class), '"Foo" should be abstract.');
Psl\invariant(true === Class\has_method(Foo::class, 'bar'), '"Foo" should have a "bar" method.');
Psl\invariant(false === Class\has_constant(Foo::class, 'QUX'), '"Foo" should not have constant "QUX".');
```

## API

### Functions

* `Class\defined(class-string $class_name): bool`
  
  Checks if the class with the given name has already been defined.

  This function will return true if a class exists, but has not been loaded yet.

  ```php
  use Psl\Class;
  use Psl\Collection;

  class Foo {}
  class Bar {}

  Class\defined(Foo::class); // true
  Class\defined(Bar::class); // true
  Class\defined(Baz::class); // false
  Class\defined(Collection\Map::class); // false
  ```

* `Class\exists(class-string $class_name): bool`

  Checks if the class with the given name exists.

  If the given class is not defined, this function will attempt to load it.

  If the given class is not defined and cannot be loaded, this function will return false.

  ```php
    use Psl\Class;
    use Psl\Collection;
  
    class Foo {}
    class Bar {}
  
    Class\exists(Foo::class); // true
    Class\exists(Bar::class); // true
    Class\exists(Baz::class); // false
    Class\exists(Collection\Map::class); // true
    ```

* `Class\has_constant(class-string $class_name, string $constant_name): bool`

  Checks if the class with the given name has a constant with the given name.

  If `$class_name` does not exists, this function will throw `Psl\Exception\InvarientViolationException`.

  ```php
  use Psl\Class;
  
  class Foo {
    const BAR = 'baz';
  }
  
  Class\has_constant(Foo::class, 'BAR'); // true
  Class\has_constant(Foo::class, 'BAZ'); // false
  ```

* `Class\has_method(class-string $class_name, string $method_name): bool`

  Checks if the class with the given name has a method with the given name.

  If `$class_name` does not exists, this function will throw `Psl\Exception\InvarientViolationException`.

  ```php
  use Psl\Class;
  
  class Foo {
    public function bar(): void {}
  }
  
  Class\has_method(Foo::class, 'bar'); // true
  Class\has_method(Foo::class, 'baz'); // false
  ```

* `Class\is_abstract(class-string $class_name): bool`

  Checks if the class with the given name is abstract.

  If `$class_name` does not exists, this function will throw `Psl\Exception\InvarientViolationException`.

  ```php
  use Psl\Class;
  
  class Foo {}
  class Bar {}
  abstract class Baz {}
  
  Class\is_abstract(Foo::class); // false
  Class\is_abstract(Bar::class); // false
  Class\is_abstract(Baz::class); // true
  ```

* `Class\is_final(class-string $class_name): bool`

  Checks if the class with the given name is final.

  If `$class_name` does not exists, this function will throw `Psl\Exception\InvarientViolationException`.

  ```php
  use Psl\Class;
  
  class Foo {}
  class Bar {}
  final class Baz {}
  
  Class\is_final(Foo::class); // false
  Class\is_final(Bar::class); // false
  Class\is_final(Baz::class); // true
  ```

