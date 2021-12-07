# Iter

## Introduction

The Iter component provides a set of functions that facilitate working with iterators.

## Usage

```php
use Psl\Iter;

$iterator = Iter\to_iterator([1, 2, 3, 4, 5]);

$result = Iter\reduce($iterator, static fn(int $carry, int $item) => $carry + $item, 0);

// $result === 15
```

## API

### Functions

<div class="api-functions">

* [`@template T` php] <br/>
  [`Iter\all(iterable<T> $iterable, (callable(T): bool) $predicate): bool` php]

  Returns true if all values in the iterable satisfy the predicate.

  This function is short-circuiting, i.e. if the predicate fails for one element the remaining elements will not be considered anymore.

  * [`$iterable` php]: The iterable to test.
  * [`$predicate` php]: The predicate to test each item against.

  ```php
  use Psl\Iter;

  $result = Iter\all([1, 2, 3, 4, 5], static fn(int $item) => $item > 0);
  // $result === true

  $result = Iter\all([1, 2, 3, 4, 5], static fn(int $item) => $item > 4);
  // $result === false

  $result = Iter\all([1, 2, 3, 4, 5], static fn(int $item) => $item > 5);
  // $result === false
  ```

* [`@template T` php] <br/>
  [`Iter\any(iterable<T> $iterable, (callable(T): bool) $predicate): bool` php]

  Returns true if any value in the iterable satisfies the predicate.

  This function is short-circuiting, i.e. if the predicate matches for one element the remaining elements will not be considered anymore.

  * [`$iterable` php]: The iterable to test.
  * [`$predicate` php]: The predicate to test each item against.

  ```php
  use Psl\Iter;

  $result = Iter\any([1, 2, 3, 4, 5], static fn(int $item) => $item > 0);
  // $result === true

  $result = Iter\any([1, 2, 3, 4, 5], static fn(int $item) => $item > 4);
  // $result === true

  $result = Iter\any([1, 2, 3, 4, 5], static fn(int $item) => $item > 5);
  // $result === false
  ```

* [`@template T` php] <br/>
  [`Iter\apply(iterable<T> $iterable, (callable(T): void) $function): void` php]

  Applies a function to all values of an iterable.

  * [`$iterable` php]: The iterable to apply the function to.
  * [`$function` php]: The function to apply to each element.

  ```php
  use Psl\Iter;

  Iter\apply(
    $records,
    static fn($record) => $record->save(),
  );
  ```

* [`@template T` php] <br/>
  [`Iter\contains(iterable<T> $iterable, T $value): bool` php]

  Returns true if the given iterable contains the value. Strict equality is used.

  * [`$iterable` php]: The iterable to test.
  * [`$value` php]: The value to test.

  ```php
  use Psl\Iter;

  $result = Iter\contains([1, 2, 3, 4, 5], 3);
  // $result === true

  $result = Iter\contains([1, 2, 3, 4, 5], 6);
  // $result === false

  $result = Iter\contains([1, 2, 3, 4, 5], '3');
  // $result === false
  ```

* [`@template Tk` php] <br/>
  [`@template Tv` php] <br/>
  [`Iter\contains_key(iterable<Tk, Tv> $iterable, Tk $key): bool` php]

  Returns true if the given iterable contains the key. Strict equality is used.

  * [`$iterable` php]: The iterable to test.
  * [`$key` php]: The key to test.

  ```php
  use Psl\Iter;

  $result = Iter\contains_key([1 => 'a', 2 => 'b', 3 => 'c'], 2);
  // $result === true

  $result = Iter\contains_key([1 => 'a', 2 => 'b', 3 => 'c'], 4);
  // $result === false

  $result = Iter\contains_key([1 => 'a', 2 => 'b', 3 => 'c'], '2');
  // $result === false
  ```

* [`@template T` php] <br/>
  [`Iter\count(iterable<T> $iterable): int` php]

  Returns the number of elements an iterable contains.

  This function is not recursive, it counts only the number of elements in the iterable itself, not its children.

  If the iterable implements [`Countable` php] its [`Countable::count()` php] method will be used.

  * [`$iterable` php]: The iterable to count.

  ```php
  use Psl\Iter;
  use ArrayIterator;

  $result = Iter\count([1, 2, 3, 4, 5]);
  // $result === 5

  $result = Iter\count([]);
  // $result === 0

  $result = Iter\count([
    new ArrayIterator([1, 2, 3, 4, 5]),
    new ArrayIterator([1, 2, 3, 4, 5]),
  ]);
  // $result === 2
  ```

* [`@template T` php] <br/>
  [`Iter\first(iterable<T> $iterable): ?T` php]

  Returns the first element of an iterable, if the iterable is empty, null will be returned.

  * [`$iterable` php]: The iterable to get the first element from.

  ```php
  use Psl\Iter;

  $result = Iter\first([1, 2, 3, 4, 5]);
  // $result === 1

  $result = Iter\first([]);
  // $result === null
  ```

* [`@template Tk` php] <br/>
  [`@template Tv` php] <br/>
  [`Iter\first_key(iterable<Tk, Tv> $iterable): ?Tk` php]

  Returns the first key of an iterable, if the iterable is empty, null will be returned.

  * [`$iterable` php]: The iterable to get the first key from.

  ```php
  use Psl\Iter;

  $result = Iter\first_key([1 => 'a', 2 => 'b', 3 => 'c']);
  // $result === 1

  $result = Iter\first_key([]);
  // $result === null
  ```

* [`@template T` php] <br/>
  [`Iter\last(iterable<T> $iterable): ?T` php]

  Returns the last element of an iterable, if the iterable is empty, null will be returned.

  * [`$iterable` php]: The iterable to get the last element from.

  ```php
  use Psl\Iter;

  $result = Iter\last([1, 2, 3, 4, 5]);
  // $result === 5

  $result = Iter\last([]);
  // $result === null
  ```

* [`@template Tk` php] <br/>
  [`@template Tv` php] <br/>
  [`Iter\last_key(iterable<Tk, Tv> $iterable): ?Tk` php]

  Returns the last key of an iterable, if the iterable is empty, null will be returned.

  * [`$iterable` php]: The iterable to get the last key from.

  ```php
  use Psl\Iter;

  $result = Iter\last_key([1 => 'a', 2 => 'b', 3 => 'c']);
  // $result === 3

  $result = Iter\last_key([]);
  // $result === null
  ```

* [`@template T` php] <br/>
  [`Iter\is_empty(iterable<T> $iterable): bool` php]

  Returns true if the given iterable is empty.

  * [`$iterable` php]: The iterable to test.

  > This function asserts that $iterable is empty if true is returned.

  ```php
  use Psl\Iter;

  $result = Iter\is_empty([1, 2, 3, 4, 5]);
  // $result === false

  $result = Iter\is_empty([]);
  // $result === true
  ```

* [`@template T` php] <br/>
  [`Iter\random(iterable<T> $iterable): T` php]

  Retrieve a random value from a non-empty iterable.

  * [`$iterable` php]: The iterable to get the random element from.

  If the iterable is empty, `Psl\Exception\InvarViolationException` will be thrown.

  ```php
  use Psl\Iter;

  $result = Iter\random([1, 2, 3, 4, 5]);
  // $result === 1

  $result = Iter\random([]);
  // $result === null
  ```

* [`@template T` php] <br/>
  [`@template Ts` php] <br/>
  [`Iter\reduce(iterable<T> $iterable, (callable(Ts, T): Ts) $function, Ts $initial): Ts` php]

  Reduces an iterable to a single value.

  The reduction function is passed an accumulator value and the current iterator value and returns a new accumulator. <br />
  The accumulator is initialized to $initial.

  * [`$iterable` php]: The iterable to reduce.
  * [`$function` php]: The function to apply to each element.
  * [`$initial` php]: The initial value.

  ```php
  use Psl\Iter;

  $result = Iter\reduce(
    [1, 2, 3, 4],
    static fn(int $carry, int $item): int => $carry + $item,
    0
  );

  // $result === 10
  ```

* [`@template Tk` php] <br/>
  [`@template Tv` php] <br/>
  [`@template Ts` php] <br/>
  [`Iter\reduce_keys(iterable<Tk, Tv> $iterable, (callable(Ts, Tk): Ts) $function, Ts $initial): Ts` php]

  Reduces an iterable to a single value.

  The reduction function is passed an accumulator value and the current iterator key and returns a new accumulator. <br />
  The accumulator is initialized to $initial.

  * [`$iterable` php]: The iterable to reduce.
  * [`$function` php]: The function to apply to each key.
  * [`$initial` php]: The initial value.

  ```php
  use Psl\Iter;

  $result = Iter\reduce_keys(
    [1 => 'a', 2 => 'b', 3 => 'c'],
    static fn(int $carry, int $item): int => $carry + $item,
    0
  );

  // $result === 6
  ```

* [`@template Tk` php] <br/>
  [`@template Tv` php] <br/>
  [`@template Ts` php] <br/>
  [`Iter\reduce_with_keys(iterable<Tk, Tv> $iterable, (callable(Ts, Tk, Tv): Ts) $function, Ts $initial): Ts` php]

  Reduces an iterable to a single value.

  The reduction function is passed an accumulator value and the current iterator key and value and returns a new accumulator. <br />
  The accumulator is initialized to $initial.

  * [`$iterable` php]: The iterable to reduce.
  * [`$function` php]: The function to apply to each key.
  * [`$initial` php]: The initial value.

  ```php
  use Psl\Iter;
  use Psl\Str;

  $result = Iter\reduce_with_keys(
    [1 => 'hello', 2 => 'world'],
    static fn(int $carry, int $key, string $value): int => $carry + $key + Str\length($value),
    0
  );

  // $result === 13
  ```

* [`@template T` php] <br/>
  [`Iter\search(iterable<T> $iterable, (callable(T): bool) $predicate): ?int` php]

  Searches an iterable until a predicate returns true, then returns the value of the matching element.

  * [`$iterable` php]: The iterable to search.
  * [`$predicate` php]: The predicate to apply to each element.

  ```php
  use Psl\Iter;

  $azjezz = Iter\search($users, static fn(User $user): bool => $user->username === 'azjezz');
  if (null !== $azjezz) {
    // $azjezz is the user with username 'azjezz'
  }
  ```

* [`@template Tk` php] <br/>
  [`@template Tv` php] <br/>
  [`Iter\rewindable(Generator<Tk, Tv> $generator): Iter\Iterator<Tk, Tv>` php]

  Create a rewindable iterator from the given generator without exhausting the generator immediately.

  * [`$generator` php]: The generator to create a rewindable iterator from.

  ```php
  use Psl\Iter;
  use Psl\IO;

  $factory = static function(): Generator {
    IO\write_line('factory: start');

    yield 1;
    yield 2;
    yield 3;

    IO\write_line('factory: end');
  };

  $generator = $factory();

  $iterator = Iter\rewindable($generator);

  // $iterator is Iter\Iterator<int, int>

  IO\write_line('iterator: start');
  foreach ($iterator as $key => $value) {
    IO\write_line("$key => $value");
  }

  IO\write_line('iterator: rewind');
  foreach ($iterator as $key => $value) {
    IO\write_line("$key => $value");
  }
  IO\write_line('iterator: end');

  // output:
  // iterator: start
  // factory: start
  // 1 => 1
  // 2 => 2
  // 3 => 3
  // factory: end
  // iterator: rewind
  // 1 => 1
  // 2 => 2
  // 3 => 3
  // iterator: end
  ```

* [`@template Tk` php] <br/>
  [`@template Tv` php] <br/>
  [`Iter\to_iterator(iterable<Tk, Tv> $iterable): Iter\Iterator<Tk, Tv>` php]

  Copy the iterable into an [`Iter\Iterator` php].

  * [`$iterable` php]: The iterable to create an iterator from.

  ```php
  use Psl\Iter;

  $iterator = Iter\to_iterator([1, 2, 3, 4]);

  // $iterator is Iter\Iterator<int, int>

  foreach ($iterator as $key => $value) {
    // $key => $value
  }
  ```

</div>

### Classes

<div class="api-classes">

* [`final class Iter\Iterator<Tk, Tv> extends Countable, SeekableIterator<Tk, Tv>` php]

  The [`Iter\Iterator` php] class is a lazy, seekable, rewindable iterator.

  <div class="api-methods">

  * [`@template Tsk` php] <br />
    [`@template Tsv` php] <br />
    [`static Iter\Iterator::from((callable(): iterable<Tsk, Tsv>) $factory): Iter\Iterator<Tsk, Tsv>` php]

    Create an iterator from a factory.

    * [`$factory` php]: The factory to create the iterator from.

    ```php
    use Psl\Iter;

    $iterator = Iter\Iterator::from(static function(): iterable {
      yield 1 => 10;
      yield 2 => 20;
      yield 3 => 30;
    });

    // $iterator is Iter\Iterator<int, int>
    ```

  * [`@template Tsk` php] <br />
    [`@template Tsv` php] <br />
    [`static Iter\Iterator::create(iterable $iterable): Iter\Iterator<Tsk, Tsv>` php]

    Create an iterator from an iterable.

    * [`$iterable` php]: The iterable to create the iterator from.

    ```php
    use Psl\Iter;

    $iterator = Iter\Iterator::create([1, 2, 3, 4]);

    // $iterator is Iter\Iterator<int, int>
    ```

  * [`Iter\Iterator::__construct(Generator<Tk, Tv, _, _> $generator)` php]

    Instantiate an iterator from a generator.

    * [`$generator` php]: The generator to create the iterator from.

    ```php
    use Psl\Iter;

    $factory = static function(): Generator {
      yield 1 => 10;
      yield 2 => 20;
      yield 3 => 30;
    };

    $generator = $factory();

    $iterator = new Iter\Iterator($generator);

    // $iterator is Iter\Iterator<int, int>
    ```

  * [`Iter\Iterator::current(): Tv` php]

    Return the current element.

    ```php
    use Psl\Iter;

    $iterator = Iter\Iterator::from(static function(): iterable {
      yield 1 => 10;
      yield 2 => 20;
      yield 3 => 30;
    });

    $value = $iterator->current();

    // $value === 10
    ```

  * [`Iter\Iterator::key(): Tk` php]

    Return the key of the current element.

    ```php
    use Psl\Iter;

    $iterator = Iter\Iterator::from(static function(): iterable {
      yield 1 => 10;
      yield 2 => 20;
      yield 3 => 30;
    });

    $key = $iterator->key();

    // $key === 1
    ```

  * [`Iter\Iterator::next(): void` php]

    Move forward to the next element.

    ```php
    use Psl\Iter;

    $iterator = Iter\Iterator::from(static function(): iterable {
      yield 1 => 10;
      yield 2 => 20;
      yield 3 => 30;
    });

    $iterator->next();

    $value = $iterator->current();

    // $value === 20
    ```

  * [`Iter\Iterator::valid(): bool` php]

    Checks if current position is valid.

    ```php
    use Psl\Iter;

    $iterator = Iter\Iterator::from(static function(): iterable {
      yield 1 => 10;
      yield 2 => 20;
      yield 3 => 30;
    });

    $valid = $iterator->valid();
    // $valid === true

    $iterator->next();
    $iterator->next();
    $iterator->next();
    $valid = $iterator->valid();
    // $valid === false

    ```

  * [`Iter\Iterator::seek(0|positive-int $position): void` php]

    Seek to the given position.

    * [`$position` php]: The position to seek to.

    If [`$position` php] is out-of-bounds, [`Psl\Exception\InvariantViolationException` php] is thrown.

    ```php
    use Psl\Iter;

    $iterator = Iter\Iterator::from(static function(): iterable {
      yield 1 => 10;
      yield 2 => 20;
      yield 3 => 30;
    });

    $iterator->seek(1);

    $value = $iterator->current();

    // $value === 20
    ```

  * [`Iter\Iterator::rewind(): void` php]

    Rewind the iterator to the first element.

    ```php
    use Psl\Iter;

    $iterator = Iter\Iterator::from(static function(): iterable {
      yield 1 => 10;
      yield 2 => 20;
      yield 3 => 30;
    });

    $iterator->next();
    $iterator->next();

    $iterator->rewind();

    $value = $iterator->current();

    // $value === 10
    ```

  * [`Iter\Iterator::count(): int` php]

    Return the number of elements.

    ```php
    use Psl\Iter;

    $iterator = Iter\Iterator::from(static function(): iterable {
      yield 1 => 10;
      yield 2 => 20;
      yield 3 => 30;
    });

    $count = $iterator->count();

    // $count === 3
    ```

    </div>

</div>