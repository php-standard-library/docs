# Async

The Async component brings concurrency into PHP using [Cooperative Multitasking](https://en.wikipedia.org/wiki/Cooperative_multitasking), 

?> The Async component is built on top of [RevoltPHP](https://github.com/revoltphp/event-loop), which makes it compatible with [Amphp](https://github.com/amphp), and others.

## Usage

```php
use Psl\IO;
use Psl\Async;
use Psl\Shell;

Async\main(static function(): int {
  $watcher = Async\Scheduler::onSignal(SIGINT, function (): never {
      IO\write_error_line('SIGINT received, stopping...');
      exit(0);
  });

  Async\Scheduler::unreference($watcher);

  IO\write_error_line('Press Ctrl+C to stop');

  Async\parallel([
    static fn(): string => Shell\execute('sleep', ['3']),
    static fn(): string => Shell\execute('echo', ['Hello World!']),
    static fn(): string => Shell\execute('echo', ['Hello World!']),
  ]);

  IO\write_error_line('Done!');

  return 0;
});
```

## API

### Functions

<div class="api-functions">

* [`Async\main((callable(): int|Async\Awaitable<int>) $callable): never` php]

  Execute [`$callable` php] in an async context, then exit with returned exit code.

  If [`$callable` php] returns an [`Async\Awaitable` php], it *MUST* resolve with an exit code.

  After executing [`$callable` php], the event loop will keep running until there's no more callables to be executed.

  * [`$callable` php]: The application entry point.

  ```php
  use Psl\Async;
  use Psl\IO;

  Async\main(static function(): int {
    Async\Scheduler::delay(1.0, static function(): void {
      IO\write_line('hello');
    });

    return 0;
  });

  // Output:
  // hello
  ```


* [`@template T` php] <br />
  [`Async\run((callable(): T) $callable, ?float $timeout = null): Async\Awaitable<T>` php]

  Create a new fiber asynchronously using the given callable, and return an awaitable that resolves to the result of the callable.

  If the callable throws an exception, the awaitable will fail with that exception.

  If [`$timeout` php] is provided, the awaitable will fail with a [`Async\Exception\TimeoutException` php] if the callable does not complete within the given timeout.

  * [`$callable` php]: The callable to execute.
  * [`$timeout` php]: The timeout in seconds.

  ```php
  use Psl\Async;

  $awaitable = Async\run(static function (): string {
    Async\sleep(1);
    return 'Hello world!';
  });
  $result = $awaitable->await(); // 'Hello world!'
  ```

  ```php
  use Psl\Async;

  $awaitable = Async\run(static function (): string {
    throw new Exception('Something went wrong!');
  });

  try {
    $result = $awaitable->await(); // throws Exception
  } catch (Exception $e) {
    // ... handle exception
  }
  ```

  ```php
  use Psl\Async;

  $awaitable = Async\run(static function (): void {
    Async\sleep(1);
  }, timeout: 0.5);

  try {
    $awaitable->await(); // throws Async\Exception\TimeoutException
  } catch (Async\Exception\TimeoutException $e) {
    // The awaitable timed out.
  }
  ```

* [`@template T` php] <br />
  [`Async\await(Awaitable<T> $awaitable): T` php]

  Await the given awaitable, and return the result.

  If the awaitable fails, the exception will be thrown.

  ```php
  use Psl\Async;

  $awaitable = Async\run(static function (): string {
    return 'Hello world!';
  });

  $result = Async\await($awaitable); // 'Hello world!'
  ```

* [`@template Tk of array-key` php] <br />
  [`@template Tv` php] <br />
  [`Async\all(iterable<Tk, Awaitable<Tv>> $awaitable): array<Tk, Tv>` php]

  Awaits all awaitables to complete concurrently.

  If one awaitable fails, the exception will be thrown immediately, and the result of the awaitables will be ignored.

  Once the awaitables have completed, an array containing the results will be returned preserving the original awaitables order.

  * [`$awaitable` php]: The awaitables to await.

  If multiple awaitables failed at once, [`Async\Exception\CompositeException` php] will be thrown.

  ```php
  use Psl\Async;
  use Psl\Shell;

  Async\all([
    Async\run(static fn() => Shell\execute('php', ['vendor/bin/phpunit', '-c', 'phpunit.xml.dist'])),
    Async\run(static fn() => Shell\execute('php', ['vendor/bin/psalm', '-c', 'psalm.xml'])),
    Async\run(static fn() => Shell\execute('php', ['vendor/bin/psalm', '-c', 'psalm.xml', '--taint-analysis'])),
    Async\run(static fn() => Shell\execute('php', ['vendor/bin/php-cs-fixer', 'fix', '--config=.php_cs.dist.php', '--dry-run'])),
    Async\run(static fn() => Shell\execute('php', ['vendor/bin/phpcs', '--standard=.phpcs.xml'])),
  ]);

  try {
    $result = Async\all([
      Async\Awaitable::error(new Exception('Something went wrong!')),
      Async\Awaitable::complete('hello'),
    ]);
  } catch (Exception $e) {
    // ... handle the exception
  }

  try {
    $result = Async\all([
      Async\Awaitable::error(new Exception('Something went wrong!')),
      Async\Awaitable::error(new Exception('Something else went wrong!')),
    ]);
  } catch (Async\Exception\CompositeException $e) {
    $reasons = $e->getReasons(); // [Exception, Exception]
    // ... handle the exceptions
  }
  ```

* [`@template T` php] <br />
  [`Async\any(iterable<Awaitable<T>> $awaitable): T` php]

  Return the first successfully completed awaitable result.

  If you want the first awaitable completed, successful or not, use [`Async\first(...)` php] instead.

  If multiple awaitables completed successfully at once, the first one will be returned.

  * [`$awaitable` php]: The awaitables to await.

  If [`$awaitable` php] is empty, [`Psl\Exception\InvariantViolationException` php] will be thrown.

  If all awaitables failed, [`Async\Exception\CompositeException` php] will be thrown.

  ```php
  use Psl;
  use Psl\Async;

  $result = Async\any([
    Async\Awaitable::error(new Exception('Something went wrong!')),
    Async\Awaitable::complete('hello'),
  ]);

  Psl\invariant($result === 'hello', 'Should be hello!');

  try {
    $result = Async\any([]);
  } catch (Psl\Exception\InvariantViolationException $e) {
    // ... handle the exception
  }

  try {
    $result = Async\any([
      Async\Awaitable::error(new Exception('Something went wrong!')),
      Async\Awaitable::error(new Exception('Something else went wrong!')),
    ]);
  } catch (Async\Exception\CompositeException $e) {
    $reasons = $e->getReasons(); // [Exception, Exception]
    // ... handle the exceptions
  }
  ```

* [`@template T` php] <br />
  [`Async\first(iterable<Awaitable<T>> $awaitable): T` php]

  Return the first completed awaitable result, or throw an exception if the first awaitable failed.

  If you want the first awaitable completed without an error, use [`Async\any(...)` php] instead.

  * [`$awaitable` php]: The awaitables to await.

  If [`$awaitable` php] is empty, [`Psl\Exception\InvariantViolationException` php] will be thrown.

  If all awaitables failed, [`Async\Exception\CompositeException` php] will be thrown.

  ```php
  use Psl;
  use Psl\Async;

  $result = Async\first([
    Async\Awaitable::complete('hello'),
    Async\Awaitable::error(new Exception('Something went wrong!')),
  ]);

  Psl\invariant($result === 'hello', 'Should be hello!');

  try {
    $result = Async\first([
      Async\Awaitable::error(new Exception('Something went wrong!')),
      Async\Awaitable::complete('hello'),
    ]);
  } catch (Exception $e) {
    // ... handle the exception
  }

  try {
    $result = Async\first([]);
  } catch (Psl\Exception\InvariantViolationException $e) {
    // ... handle the exception
  }

  try {
    $result = Async\first([
      Async\Awaitable::error(new Exception('Something went wrong!')),
      Async\Awaitable::error(new Exception('Something else went wrong!')),
    ]);
  } catch (Async\Exception\CompositeException $e) {
    $reasons = $e->getReasons(); // [Exception, Exception]
    // ... handle the exceptions
  }
  ```

* [`@template Tk of array-key` php] <br />
  [`@template Tv` php] <br />
  [`Async\series(iterable<Tk, (callable(): Tv)> $functions): array<Tk, Tv>` php]

  Run the iterable of functions in series, each one running once the previous function has completed.

  If any functions in the series throws, no more functions are run, and the exception is immediately thrown.

  ```php
  use Psl\Async;

  $results = Async\series([
    create_users(...),
    create_organizations(...),
    create_roles(...),
    create_user_organization_roles(...),
  ]);
  ```

* [`@template Tk of array-key` php] <br />
  [`@template Tv` php] <br />
  [`Async\parallel(iterable<Tk, (callable(): Tv)> $functions): array<Tk, Tv>` php]

  Run the iterable of functions in parallel, without waiting until the previous function has completed.

  If one of the functions fails, the exception will be thrown immediately, and the result of the other functions will be ignored.

  Once all the functions have completed, an array containing the results will be returned preserving the original [`$functions` php] order.

  * [`$functions` php]: The functions to run.

  If multiple functions failed at once, [`Async\Exception\CompositeException` php] will be thrown.

  ```php
  use Psl\Async;

  $functions = [
    static function(): string {
      Async\sleep(0.5);
      return 'hello';
    },
    static function(): string {
      return 'world';
    },
  ];

  $results = Async\parallel($functions); // ['hello', 'world']
  ```

  ?> [`Async\parallel(...)` php] is about kicking-off I/O functions in parallel, not about parallel execution of code. <br />
  If your functions do not use any timers or perform any non-blocking I/O, they will actually be executed in series.

  ```php
  use Psl\Async;

  // the following runs in series, as `file_get_contents` is blocking.
  Async\parallel([
    static fn() => file_get_contents('/etc/hosts'),
    static fn() => file_get_contents('/etc/resolv.conf'),
  ]);
  ```

  ?> use [`Async\reflect(...)` php] to continue the execution of other functions when a function fails.

  ```php
  use Psl;
  use Psl\Async;
  use Psl\Shell;

  [$version, $foo] = Async\parallel([
    Async\reflect(static fn() => Shell\execute('php', ['-v'])),
    Async\reflect(static fn() => Shell\execute('php', ['-r', 'foo();'])),
  ]);

  Psl\invariant($version->isSucceeded(), '`$ php -v` should have succeeded.');
  Psl\invariant($foo->isFailed(), '`$ php -r "foo()"` should have failed.');
  ```

* [`@pure` php] <br />
  [`@template T` php] <br />
  [`Async\reflect((callable(): T) $function): (Closure(): Result\ResultInterface<T>)` php]

  Wraps the given function in another function that always completes with a [`Result\Success` php],
  or [`Result\Failure` php] if the original function throws.

  ```php
  use Psl;
  use Psl\Async;

  $results = Async\parallel([
    Async\reflect(static fn() => 'hello'),
    Async\reflect(static fn() => throw new Exception('Something went wrong!')),
  ]);

  Psl\invariant($results[0]->isSucceeded(), '`hello` should have succeeded.');
  Psl\invariant($results[1]->isFailed(), '`Something went wrong!` should have failed.');
  ```

* [`Async\sleep(float $seconds): void` php]

  Non-blocking sleep for the specified number of seconds.

  ```php
  use Psl;
  use Psl\Async;

  use function time;

  $time = time();

  Async\parallel([
    static fn() => Async\sleep(2),
    static fn() => Async\sleep(2),
    static fn() => Async\sleep(2),
  ]);

  $duration = time() - $time;

  Psl\invariant(2 <= $duration < 3, 'Should sleep for 2 seconds.');
  ```

* [`Async\later(): void` php]

  Reschedule the work of an async function until some other time in the future.

  The common use case for this is if your async function actually has to wait for some blocking call,
  you can tell other callbacks in the async scheduler that they can work while this one waits for
  the blocking call to finish (e.g., maybe in a polling situation or something).

  ```php
  use Psl;
  use Psl\Async;

  $deferred = new Async\Deferred();
  Async\Schedule::defer(static fn() => $deferred->complete('hello'));
  Async\later();
  Psl\invariant($deferred->isComplete(), 'Deferred should be complete.');
  ```

</div>

### Classes

<div class="api-classes">

* [`final class Async\Awaitable<T> implements Promise\PromiseInterface<T>` php]

  An awaitable is a promise that can be awaited.

  It can be used to wait for the result of an async function, or to wait for the result of a blocking call.

  It can also be used to wait for the result of a [`Async\Deferred` php].

  ```php
  use Psl\Async;

  $awaitable = Async\run(static fn(): string => 'hello');
  $result = $awaitable->await(); // 'hello'
  ```

  <div class="api-methods">

  * [`@template Tv` php] <br />
    [`static Awaitable::complete(Tv $result): Awaitable<Tv>` php]

    Create an awaitable that completes with the given value.

    ```php
    use Psl\Async;

    $awaitable = Async\Awaitable::complete('hello');
    $result = $awaitable->await(); // 'hello'
    ```

  * [`static Awaitable::error(Exception $exception): Awaitable<never>` php]

    Create an awaitable that fails with the given [`$exception` php].

    ```php
    use Psl\Async;

    $awaitable = Async\Awaitable::error(new Exception('Something went wrong!'));

    try {
      $awaitable->await();
    } catch (Exception $e) {
      // handle the exception
    }
    ```

  * [`@template Tk` php] <br />
    [`@template Tv` php] <br />
    [`static Awaitable::iterate(iterable<Tk, Awaitable<Tv>> $awaitables): Generator<Tk, Awaitable<Tv>, _, _>` php]

    Iterate over the given awaitables in completion order.

    ```php
    use Psl\IO;
    use Psl\Async;

    $awaitables = [
      Async\run(static function() {
        Async\sleep(1);
        return 'a';
      }),
      Async\run(static function() {
        return 'b';
      }),
      Async\run(static function() {
        Async\sleep(0.3);
        return 'c';
      }),
      Async\run(static function() {
        Async\sleep(0.1);
        return 'd';
      }),
    ];

    foreach(Async\Awaitable::iterate($awaitables) as $k => $awaitable) {
      $result = $awaitable->await();
      IO\writeLine($k . ': ' . $result);
    }

    // Output:
    // 1: b
    // 3: d
    // 2: c
    // 0: a
    ```

  * [`Awaitable::isComplete(): bool` php]

    Returns [`true` php] if the awaitable is complete.

    ```php
    use Psl\Async;

    $awaitable = Async\Awaitable::complete('hello');

    Psl\invariant($awaitable->isComplete(), 'Should be complete.');

    $awaitable = Async\run(static fn() => Async\sleep(2));

    Psl\invariant(!$awaitable->isComplete(), 'Should not be complete.');
    ```

  * [`Awaitable::await(): T` php]

    Await the result of the awaitable.

    ```php
    use Psl\Async;

    $awaitable = Async\run(static function(): string {
      Async\sleep(2);
      return 'hello';
    });

    Psl\invariant($awaitable->await() === 'hello', 'Should be "hello".');
    ```
  
  * [`@template Ts` php] <br />
    [`Awaitable::then((Closure(T): Ts) $success, (Closure(Exception): Ts) $failure): Awaitable<Ts>` php]

    Attaches callbacks that are invoked when this awaitable is completed.

    The returned awaitable is resolved with the return value of the callback,
    or rejected with an exception thrown from the callback.

    ```php
    use Psl;
    use Psl\Async;
    use Psl\Str;

    $awaitable = Async\run(static function(): string {
      return 'hello';
    });

    $awaitable = $awaitable->then(
      static fn($result) => Str\format('%s world', $result),
      static fn($error) =>  Psl\invariant_violation('Should not throw.'),
    );

    $result = $awaitable->await(); // 'hello world'
    ```

  * [`@template Ts` php] <br />
    [`Awaitable::map((Closure(T): Ts) $success): Awaitable<Ts>` php]

    Attaches a callback that is invoked if this awaitable is completed successfully.

    The returned awaitable is resolved with the return value of the callback,
    or rejected with an exception thrown from the callback.

    ```php
    use Psl\Async;
    use Psl\Str;

    $awaitable = Async\run(static function(): string {
      return 'hello';
    });

    $awaitable = $awaitable
      ->map(static fn($result) => Str\format('%s world', $result));

    $result = $awaitable->await(); // 'hello world'
    ```

  * [`@template Ts` php] <br />
    [`Awaitable::catch((Closure(Exception): Ts) $failure): Awaitable<T|Ts>` php]

    Attaches a callback that is invoked if this awaitable is completed with an error.

    The returned awaitable is resolved with the return value of the callback,
    or rejected with an exception thrown from the callback.

    ```php
    use Psl\Async;

    $awaitable = Async\run(static function(): string {
      throw new Exception('Something went wrong!');
    });

    $awaitable = $awaitable
      ->catch(static fn($error) => $error->getMessage());

    $result = $awaitable->await(); // 'Something went wrong!'
    ```

  * [`Awaitable::always((Closure(): void) $always): Awaitable<T>` php]

    Attaches a callback that is invoked when this awaitable is completed.

    ```php
    use Psl\IO;
    use Psl\Async;

    $awaitable = Async\run(static function(): string {
      return 'hello';
    });

    $awaitable = $awaitable->always(static function(): void {
      IO\write_line('done');
    });

    $result = $awaitable->await(); // 'hello'
    // Output:
    // done
    ```


  * [`Awaitable::ignore(): this` php]

    Do not forward unhandled errors to the event loop handler.

    ```php
    use Psl\Async;

    Async\run(static function(): string {
      throw new Exception('Something went wrong!');
    })->ignore();

    // No exception thrown
    Async\Scheduler::run();
    ```

  </div>

* [`final class Async\Deferred<T>` php]

  ?> The [`Async\Deferred` php] API described below is an advanced API that many applications probably don’t need.
  Use [`Async\run(...)` php], and other [`Async`] combinators when possible.

  [`Async\Deferred` php] is the abstraction responsible for resolving future values once they become available.` <br />
  A library that completes values asynchronously creates an [`Async\Deferred` php] and uses it to return an [`Async\Awaitable` php] to API consumers. <br />
  Once the async library determines that the value is ready it completes the awaitable held by the API consumer using methods on the linked promisor.

  ```php
  use Psl;
  use Psl\Async;
  use Psl\IO;

  $deferred = new Async\Deferred();

  $deferred->getAwaitable()->then(
    static fn($result) => Psl\invariant($result === 'hello', 'Should be "hello".'),
    static fn($error) => Psl\invariant(false, 'Should not have failed.'),
  );

  $deferred->complete('hello');
  ```

  <div class="api-methods">

  * [`Deferred::getAwaitable()` php]

    Returns the [`Async\Awaitable` php] that will be completed when the value is available.

    [`Async\Deferred` php] and [`Async\Awaitable` php] are separated, so the consumer of the [`Async\Awaitable` php] can’t complete it.
    You should always return [`$deferred->getAwaitable()` php] to API consumers.
    If you’re passing [`Async\Deferred` php] objects around, you’re probably doing something wrong.
    
    ```php
    use Psl\Async;

    /**
     * @return Async\Awaitable<'hello'>
    */
    function get_message(): Async\Awaitable
    {
      $deferred = new Async\Deferred();
      Async\Scheduler::delay(2, static fn() => $deferred->complete('hello'));

      return $deferred->getAwaitable();
    }

    get_message()->await(); // 'hello'
    ```

  * [`Deferred::complete(T $value)` php]

    Completes the [`Async\Awaitable` php] with the given value.

    ```php
    use Psl\Async;

    $deferred = new Async\Deferred();
    $deferred->complete('hello');

    $result = $deferred->getAwaitable()->await(); // 'hello'
    ```

  * [`Deferred::error(Exception $exception): void` php]

    Makes the [`Async\Awaitable` php] fail with the given [`$exception` php].

    ```php
    use Psl\Async;

    $deferred = new Async\Deferred();
    $deferred->error(new Exception('Something went wrong!'));

    try {
      $deferred->getAwaitable()->await();
    } catch (Exception $e) {
      // handle the exception...
    }
    ```
  
  * [`Deferred::isComplete(): bool` php]

    Returns `true` if the [`Async\Awaitable` php] has been completed.

    ```php
    use Psl\Async;

    $deferred = new Async\Deferred();

    Psl\invariant(false === $deferred->isComplete(), 'Should be pending.');

    $deferred->complete('hello');

    Psl\invariant(true === $deferred->isComplete(), 'Should be complete.');
    ```

  </div>

* [`final class Async\Scheduler` php]

  Psl wrapper around Revolt event-loop.

  See [revolt.run](https://revolt.run/) for more information.

  <div class="api-methods">

  * [`static Scheduler::createSuspension(): Revolt\EventLoop\Suspension` php]

    Create an object used to suspend and resume execution, either within a fiber or from {main}.

    ```php
    use Psl\Async;

    $suspension = Async\Scheduler::createSuspension();
    // schedule the resume of the suspension 2 seconds from now.
    Async\Scheduler::delay(2.0, static fn() => $suspension->resume());
    // suspend the suspension until it is resumed.
    $suspension->suspend();
    ```

  * [`static Scheduler::onSignal(int $signal_number, (Closure(string, int): void) $callback): non-empty-string` php]

    Register a callback to be called when the given signal is received.

    Returns a unique identifier that can be used to cancel, enable or disable the callback.

    see [revolt documentation](https://revolt.run/signals) for more information.

  * [`static Scheduler::onReadable(object|resource $stream, (Closure(string, object|resource): void) $callback): non-empty-string` php]

    Execute a callback when a stream resource becomes readable or is closed for reading.

    Returns a unique identifier that can be used to cancel, enable or disable the callback.

    see [revolt documentation](https://revolt.run/streams) for more information.

  * [`static Scheduler::onWritable(object|resource $stream, (Closure(string, object|resource): void) $callback): non-empty-string` php]

    Execute a callback when a stream resource becomes writable or is closed for writing.

    Returns a unique identifier that can be used to cancel, enable or disable the callback.

    see [revolt documentation](https://revolt.run/streams) for more information.

  * [`static Scheduler::defer((Closure(): void) $callback): non-empty-string` php]

    Defer the execution of a callback.

    Returns a unique identifier that can be used to cancel, enable or disable the callback.

    see [revolt documentation](https://revolt.run/timers) for more information.
  
  * [`static Scheduler::delay(float $seconds, (Closure(): void) $callback): non-empty-string` php]

    Delay the execution of a callback.

    Returns a unique identifier that can be used to cancel, enable or disable the callback.

    see [revolt documentation](https://revolt.run/timers) for more information.
  
  * [`static Scheduler::repeat(float $interval, (Closure(): void) $callback): non-empty-string` php]

    Repeatedly execute a callback.

    Returns a unique identifier that can be used to cancel, enable or disable the callback.

    see [revolt documentation](https://revolt.run/timers) for more information.

  * [`static Scheduler::cancel(string $id): void` php]

    Cancel a callback.

    see [revolt documentation](https://revolt.run/fundamentals) for more information.
  
  * [`static Scheduler::enable(string $id): void` php]

    Enable a callback.

    see [revolt documentation](https://revolt.run/fundamentals) for more information.
  
  * [`static Scheduler::disable(string $id): void` php]

    Disable a callback.

    see [revolt documentation](https://revolt.run/fundamentals) for more information.
  
  * [`static Scheduler::reference(string $id): void` php]

    Reference a callback.

    see [revolt documentation](https://revolt.run/fundamentals) for more information.
  
  * [`static Scheduler::unreference(string $id): void` php]

    Unreference a callback.

    see [revolt documentation](https://revolt.run/fundamentals) for more information.

  * [`static Scheduler::queue((Closure(): void) $callback): void` php]

    Queue a microtask.
  
  * [`static Scheduler::run(): void` php]

    Run the event loop.

    This method will wait until there's no more callbacks to execute.

    If a signal callback is registered, this method will block until the signal is received.

    see [revolt documentation](https://revolt.run/fundamentals) for more information.

  </div>
</div>

### Exceptions

<div class="api-exceptions">

* [`final class Async\Exception\ComositeException` php]

  A [`CompositeException` php] that can be used to wrap multiple [`Exception` php]s.

  <div class="api-methods">

  * [`CompositeException::__construct(non-empty-array<array-key, Exception> $reasons)` php]

    Constructs a new [`CompositeException` php] with the given [`$reasons` php].

    ```php
    use Psl\Async;

    $exception = new Async\Exception\CompositeException([
      new Exception('Something went wrong!'),
      new Exception('Something else went wrong!'),
    ]);
    ```

  * [`CompositeException::getReasons(): non-empty-array<array-key, Exception>` php]

    Returns the [`$exceptions` php] that were wrapped.

    ```php
    use Psl\Async;

    $exception = new Async\Exception\CompositeException([
      new Exception('Something went wrong!'),
      new Exception('Something else went wrong!'),
    ]);

    $exceptions = $exception->getReasons();
    ```

  </div>

* [`final class Async\Exception\TimeoutException` php]

  A [`TimeoutException` php] is thrown when a task is not completed within the given [`$timeout` php].

  ```php
  use Psl\Async;
  use Psl\IO;

  $awaitable = Async\run(static function(): void {
    Async\sleep(4);
  }, timeout: 1.0);

  try {
    $awaitable->await();
  } catch (Async\Exception\TimeoutException $exception) {
    IO\write_error_line('Task timed out!');
  }
  ```

* [`final class Async\Exception\UnhandledAwaitableException` php]

  A [`UnhandledAwaitableException` php] is thrown from the scheduler when a failed [`Awaitable` php] is not handled.

  ```php
  use Psl\Async;

  Async\run(static function(): void {
    throw new Exception('Something went wrong!');
  });

  try {
    Async\Scheduler::run();
  } catch (Async\Exception\UnhandledAwaitableException $exception) {
    IO\write_error_line('Unhandled awaitable!');
    IO\write_error_line('Previous exception: %s', $exception->getPrevious()->getMessage());
  }

  // Output:
  // Unhandled awaitable!
  // Previous exception: Something went wrong!
  ```
</div>

