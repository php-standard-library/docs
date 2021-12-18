# Fun

## Introduction

The `Fun` component brings functional programming into PHP.

## Usage

```php
use Psl;
use Psl\Fun;

$admin = Fun\when(
  static fn(User $user): bool => $user->isAdmin(),
  Fun\identity(),
  static fn(User $user): User => $user->withRole(Role::Admin)
)($user);
```

## API

### Functions

<div class="api-functions">

* [`@template T` php] <br />
  [`@pure` php] <br />
  [`Fun\identity(): (Closure(T): T)` php]

  Create a closure that returns the value passed to it as an argument.

  ```php
  use Psl;
  use Psl\Fun;
  use Psl\Collection;

  $vector = Collection\Vector::fromArray([1, 2, 3]);
  $id = Fun\identity();

  $vector2 = $id($vector);

  Psl\invariant($vector === $vector2, '$vector and $vector2 are the same');
  ```

* [`@template I` php] <br />
  [`@template O` php] <br />
  [`@template R` php] <br />
  [`@pure` php] <br />
  [`Fun\after((Closure(I): O) $first, (Closure(O): R) $next): (Closure(I): R)` php]

  Returns a closure that calls the next functions with the result of the first one.

  ```php
  use Psl;
  use Psl\Fun;

  $calculate = Fun\after(
    static fn(int $a): int => $a + 2,
    static fn(int $c): int => $c * 2
  );

  Psl\invariant($calculate(1) === 6, 'Result is 6');
  Psl\invariant($calculate(2) === 8, 'Result is 8');
  ```

* [`template T` php] <br />
  [`@pure` php] <br />
  [`Fun\lazy((Closure(): T) $initializer): (Closure(): T)` php]

  Returns a closure that can be used for lazy evaluation.

  ```php
  use Psl;
  use Psl\IO;
  use Psl\Fun;

  $lazy = Fun\lazy(static function(): int {
    IO\write_line('evaluated.');

    return 1;
  });

  Psl\invariant($lazy() === 1, 'Result is 1');
  Psl\invariant($lazy() === 1, 'Result is 1');
  // Output:
  // evaluated.
  ```

  This function can also be used to create lazy streams.

  ```php
  use Psl;
  use Psl\Dict;
  use Psl\Fun;

  $stream = Fun\lazy(static function(): iterable {
    $i = 0;
    while (true) {
        yield ++$i;
    }
  });

  $a = Dict\take($stream(), 10); // [1...10]
  $b = Dict\take($stream(), 20); // [11...30]
  ```

* [`template T` php] <br />
  [`@pure` php] <br />
  [`Fun\pipe((Closure(T): T) ...$stages): (Closure(): T)` php]

  Performs left-to-right function composition.

  ```php
  use Psl;
  use Psl\Fun;

  $add = static fn(int $a): int => $a + 5;
  $mul = static fn(int $a): int => $a * 3;
  $div = static fn(int $a): int => $a / 2;

  $pipe = Fun\pipe($add, $mul, $div);

  Psl\invariant($pipe(1) === 9, 'Result is 9');
  ```

* [`@pure` php] <br />
  [`Fun\rethrow(): (Closure(Exception): never)` php]

  Returns a closure that rethrows the exception passed to it.

  ```php
  use Psl;
  use Psl\Fun;

  $rethrow = Fun\rethrow();

  try {
    $rethrow(new Exception('foo'));
  } catch (Exception $e) {
    Psl\invariant($e->getMessage() === 'foo', 'Exception message is "foo"');
  }
  ```

  ```php
  use Psl;
  use Psl\Fun;
  use Psl\Async;

  $awaitable = Async\run(static fn() => throw new Exception('foo'))
    ->then(Fun\identity(), Fun\rethrow());

  try {
    $awaitable->await();
  } catch (Exception $e) {
    Psl\invariant($e->getMessage() === 'foo', 'Exception message is "foo"');
  }
  ```

* [`template T` php] <br />
  [`@pure` php] <br />
  [`Fun\tap((Closure(T): void) $callback): (Closure(T): T)` php]

  Returns a closure that calls the callback with the value passed to it and returns the value.

  ```php
  use Psl;
  use Psl\Fun;

  $tap = Fun\tap(static fn(string $a) => IO\write_line($a));

  Psl\invariant($tap('hello') === 'hello', 'Result is "hello"');
  // Output:
  // hello
  ```

* [`template Ti` php] <br />
  [`template To` php] <br />
  [`@pure` php] <br />
  [`Fun\when((Closure(Ti): bool) $condition, (Closure(Ti): To) $then, (Closure(Ti): To) $else): (Closure(Ti): To)` php]

  Returns a closure that returns the result of the [`$then` php] function if the condition is true, otherwise the result of the [`$else` php] function.

  ```php
  use Psl;
  use Psl\Fun;

  $when = Fun\when(
    static fn(int $a): bool => $a > 0,
    static fn(int $a): int => $a * 2,
    static fn(int $a): int => $a * 3
  );

  Psl\invariant($when(1) === 2, 'Result is 2');
  Psl\invariant($when(-1) === 3, 'Result is 3');
  ```
</div>