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

* `Iter\all<T>(iterable<T> $iterable, (callable(T): bool) $predicate): bool`

  Returns true if all values in the iterable satisfy the predicate.

  This function is short-circuiting, i.e. if the predicate fails for one element the remaining elements will not be considered anymore.

  * `$iterable`: The iterable to test.
  * `$predicate`: The predicate to test each item against.

  ```php
  use Psl\Iter;

  $result = Iter\all([1, 2, 3, 4, 5], static fn(int $item) => $item > 0);
  // $result === true

  $result = Iter\all([1, 2, 3, 4, 5], static fn(int $item) => $item > 4);
  // $result === false

  $result = Iter\all([1, 2, 3, 4, 5], static fn(int $item) => $item > 5);
  // $result === false
  ```

* `Iter\any<T>(iterable<T> $iterable, (callable(T): bool) $predicate): bool`

  Returns true if any value in the iterable satisfies the predicate.

  This function is short-circuiting, i.e. if the predicate matches for one element the remaining elements will not be considered anymore.

  * `$iterable`: The iterable to test.
  * `$predicate`: The predicate to test each item against.

  ```php
  use Psl\Iter;

  $result = Iter\any([1, 2, 3, 4, 5], static fn(int $item) => $item > 0);
  // $result === true

  $result = Iter\any([1, 2, 3, 4, 5], static fn(int $item) => $item > 4);
  // $result === true

  $result = Iter\any([1, 2, 3, 4, 5], static fn(int $item) => $item > 5);
  // $result === false
  ```

* `Iter\apply<T>(iterable<T> $iterable, (callable(T): void) $function): void`

  Applies a function to all values of an iterable.

  * `$iterable`: The iterable to apply the function to.
  * `$function`: The function to apply to each element.

  ```php
  use Psl\Iter;

  Iter\apply(
    $records,
    static fn($record) => $record->save(),
  );
  ```

* `Iter\contains<T>(iterable<T> $iterable, T $value): bool`

  Returns true if the given iterable contains the value. Strict equality is used.

  * `$iterable`: The iterable to test.
  * `$value`: The value to test.

  ```php
  use Psl\Iter;

  $result = Iter\contains([1, 2, 3, 4, 5], 3);
  // $result === true

  $result = Iter\contains([1, 2, 3, 4, 5], 6);
  // $result === false

  $result = Iter\contains([1, 2, 3, 4, 5], '3');
  // $result === false
  ```

* `Iter\contains_key<Tk, Tv>(iterable<Tk, Tv> $iterable, Tk $key): bool`

  Returns true if the given iterable contains the key. Strict equality is used.

  * `$iterable`: The iterable to test.
  * `$key`: The key to test.

  ```php
  use Psl\Iter;

  $result = Iter\contains_key([1 => 'a', 2 => 'b', 3 => 'c'], 2);
  // $result === true

  $result = Iter\contains_key([1 => 'a', 2 => 'b', 3 => 'c'], 4);
  // $result === false

  $result = Iter\contains_key([1 => 'a', 2 => 'b', 3 => 'c'], '2');
  // $result === false
  ```

* `Iter\count<T>(iterable<T> $iterable): int`

  Returns the number of elements an iterable contains.

  This function is not recursive, it counts only the number of elements in the iterable itself, not its children.

  If the iterable implements `Countable` its `Countable::count()` method will be used.

  * `$iterable`: The iterable to count.

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

* `Iter\first<T>(iterable<T> $iterable): ?T`

  Returns the first element of an iterable, if the iterable is empty, null will be returned.

  * `$iterable`: The iterable to get the first element from.

  ```php
  use Psl\Iter;

  $result = Iter\first([1, 2, 3, 4, 5]);
  // $result === 1

  $result = Iter\first([]);
  // $result === null
  ```

* `Iter\first_key<Tk, Tv>(iterable<Tk, Tv> $iterable): ?Tk`

  Returns the first key of an iterable, if the iterable is empty, null will be returned.

  * `$iterable`: The iterable to get the first key from.

  ```php
  use Psl\Iter;

  $result = Iter\first_key([1 => 'a', 2 => 'b', 3 => 'c']);
  // $result === 1

  $result = Iter\first_key([]);
  // $result === null
  ```

* `Iter\last<T>(iterable<T> $iterable): ?T`

  Returns the last element of an iterable, if the iterable is empty, null will be returned.

  * `$iterable`: The iterable to get the last element from.

  ```php
  use Psl\Iter;

  $result = Iter\last([1, 2, 3, 4, 5]);
  // $result === 5

  $result = Iter\last([]);
  // $result === null
  ```

* `Iter\last_key<Tk, Tv>(iterable<Tk, Tv> $iterable): ?Tk`

  Returns the last key of an iterable, if the iterable is empty, null will be returned.

  * `$iterable`: The iterable to get the last key from.

  ```php
  use Psl\Iter;

  $result = Iter\last_key([1 => 'a', 2 => 'b', 3 => 'c']);
  // $result === 3

  $result = Iter\last_key([]);
  // $result === null
  ```

* `Iter\is_empty<T>(iterable<T> $iterable): bool`

  Returns true if the given iterable is empty.

  * `$iterable`: The iterable to test.

  > This function asserts that $iterable is empty if true is returned.

  ```php
  use Psl\Iter;

  $result = Iter\is_empty([1, 2, 3, 4, 5]);
  // $result === false

  $result = Iter\is_empty([]);
  // $result === true
  ```

* `Iter\random<T>(iterable<T> $iterable): T`

  Retrieve a random value from a non-empty iterable.

  * `$iterable`: The iterable to get the random element from.

  If the iterable is empty, `Psl\Exception\InvarViolationException` will be thrown.

  ```php
  use Psl\Iter;

  $result = Iter\random([1, 2, 3, 4, 5]);
  // $result === 1

  $result = Iter\random([]);
  // $result === null
  ```

* `Iter\reduce<T, Ts>(iterable<T> $iterable, (callable(Ts, T): Ts) $function, Ts $initial): Ts`

  Reduces an iterable to a single value.

  The reduction function is passed an accumulator value and the current iterator value and returns a new accumulator. <br />
  The accumulator is initialized to $initial.

  * `$iterable`: The iterable to reduce.
  * `$function`: The function to apply to each element.
  * `$initial`: The initial value.

  ```php
  use Psl\Iter;

  $result = Iter\reduce(
    [1, 2, 3, 4],
    static fn(int $carry, int $item): int => $carry + $item,
    0
  );

  // $result === 10
  ```

* `Iter\reduce_keys<Tk, Tv, Ts>(iterable<Tk, Tv> $iterable, (callable(Ts, Tk): Ts) $function, Ts $initial): Ts`

  Reduces an iterable to a single value.

  The reduction function is passed an accumulator value and the current iterator key and returns a new accumulator. <br />
  The accumulator is initialized to $initial.

  * `$iterable`: The iterable to reduce.
  * `$function`: The function to apply to each key.
  * `$initial`: The initial value.

  ```php
  use Psl\Iter;

  $result = Iter\reduce_keys(
    [1 => 'a', 2 => 'b', 3 => 'c'],
    static fn(int $carry, int $item): int => $carry + $item,
    0
  );

  // $result === 6
  ```

* `Iter\reduce_with_keys<Tk, Tv, Ts>(iterable<Tk, Tv> $iterable, (callable(Ts, Tk, Tv): Ts) $function, Ts $initial): Ts`

  Reduces an iterable to a single value.

  The reduction function is passed an accumulator value and the current iterator key and value and returns a new accumulator. <br />
  The accumulator is initialized to $initial.

  * `$iterable`: The iterable to reduce.
  * `$function`: The function to apply to each key.
  * `$initial`: The initial value.

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

* `Iter\search<T>(iterable<T> $iterable, (callable(T): bool) $predicate): ?int`

  Searches an iterable until a predicate returns true, then returns the value of the matching element.

  * `$iterable`: The iterable to search.
  * `$predicate`: The predicate to apply to each element.

  ```php
  use Psl\Iter;

  $azjezz = Iter\search($users, static fn(User $user): bool => $user->username === 'azjezz');
  if (null !== $azjezz) {
    // $azjezz is the user with username 'azjezz'
  }
  ```

* `Iter\rewindable<Tk, Tv>(Generator<Tk, Tv> $generator): Iter\Iterator<Tk, Tv>`

  Create a rewindable iterator from the given generator without exhausting the generator immediately.

  * `$generator`: The generator to create a rewindable iterator from.

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

* `Iter\to_iterator<Tk, Tv>(iterable<Tk, Tv> $iterable): Iter\Iterator<Tk, Tv>`

  Copy the iterable into an Iterator.

  * `$iterable`: The iterable to create an iterator from.

  ```php
  use Psl\Iter;

  $iterator = Iter\to_iterator([1, 2, 3, 4]);

  // $iterator is Iter\Iterator<int, int>

  foreach ($iterator as $key => $value) {
    // $key => $value
  }
  ```

### Classes

* `Iter\Iterator<Tk, Tv> extends Countable, SeekableIterator<Tk, Tv>`

  The `Iter\Iterator` class is a lazy, seekable, rewindable iterator.

  * `static Iter\Iterator::from<Tsk, Tsv>((callable(): iterable<Tsk, Tsv>) $factory): Iter\Iterator<Tsk, Tsv>`

    Create an iterator from a factory.

    * `$factory`: The factory to create the iterator from.

    ```php
    use Psl\Iter;

    $iterator = Iter\Iterator::from(static function(): iterable {
      yield 1 => 10;
      yield 2 => 20;
      yield 3 => 30;
    });

    // $iterator is Iter\Iterator<int, int>
    ```

  * `static Iter\Iterator::create<Tsk, Tsv>(iterable $iterable): Iter\Iterator<Tsk, Tsv>`

    Create an iterator from an iterable.

    * `$iterable`: The iterable to create the iterator from.

    ```php
    use Psl\Iter;

    $iterator = Iter\Iterator::create([1, 2, 3, 4]);

    // $iterator is Iter\Iterator<int, int>
    ```

  * `Iter\Iterator::__construct(Generator<Tk, Tv, _, _> $generator)`

    Instantiate an iterator from a generator.

    * `$generator`: The generator to create the iterator from.

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

  * `Iter\Iterator::current(): Tv`

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

  * `Iter\Iterator::key(): Tk`

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

  * `Iter\Iterator::next(): void`

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

  * `Iter\Iterator::valid(): bool`

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

  * `Iter\Iterator::seek(0|positive-int $position): void`

    Seek to the given position.

    * `$position`: The position to seek to.

    If `$position` is out-of-bounds, `Psl\Exception\InvariantViolationException` is thrown.

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

  * `Iter\Iterator::rewind(): void`

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

  * `Iter\Iterator::count(): int`

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
