# Promise

The Promise component defines an extended Promise/A+ interface.

## Usage

```php
use Psl\IO;

$promise
    ->map(function (string $value): never {
      throw new Exception($value);
    })
    ->catch(function (Exception $exception): string {
      return $exception->getMessage() . ', world!';
    })
    ->always(function () {
      IO\write('Promise is complete!');
    });
```

## API

### Interfaces

<div class="api-interfaces">

* [`interface PromiseInterface<T>` php]

  <div class="api-methods">
  
  * [`@template Ts` php] <br />
    [`PromiseInterface::then((Closure(T): Ts) $success, (Closure(Exception): Ts) $failure): PromiseInterface<Ts>` php]

    Attaches callbacks that are invoked when this promise is completed.

    The returned promise is resolved with the return value of the callback,
    or rejected with an exception thrown from the callback.

    ```php
    use Psl;
    use Psl\Async;
    use Psl\Str;

    $promise = Async\run(static function(): string {
      return 'hello';
    });

    $promise = $promise->then(
      static fn($result) => Str\format('%s world', $result),
      static fn($error) =>  Psl\invariant_violation('Should not throw.'),
    );

    $result = $promise->await(); // 'hello world'
    ```

  * [`@template Ts` php] <br />
    [`PromiseInterface::map((Closure(T): Ts) $success): PromiseInterface<Ts>` php]

    Attaches a callback that is invoked if this promise is completed successfully.

    The returned promise is resolved with the return value of the callback,
    or rejected with an exception thrown from the callback.

    ```php
    use Psl\Async;
    use Psl\Str;

    $promise = Async\run(static function(): string {
      return 'hello';
    });

    $promise = $promise
      ->map(static fn($result) => Str\format('%s world', $result));

    $result = $promise->await(); // 'hello world'
    ```

  * [`@template Ts` php] <br />
    [`PromiseInterface::catch((Closure(Exception): Ts) $failure): PromiseInterface<T|Ts>` php]

    Attaches a callback that is invoked if this promise is completed with an error.

    The returned promise is resolved with the return value of the callback,
    or rejected with an exception thrown from the callback.

    ```php
    use Psl\Async;

    $promise = Async\run(static function(): string {
      throw new Exception('Something went wrong!');
    });

    $promise = $promise
      ->catch(static fn($error) => $error->getMessage());

    $result = $promise->await(); // 'Something went wrong!'
    ```

  * [`PromiseInterface::always((Closure(): void) $always): PromiseInterface<T>` php]

    Attaches a callback that is invoked when this promise is completed.

    ```php
    use Psl\IO;
    use Psl\Async;

    $promise = Async\run(static function(): string {
      return 'hello';
    });

    $promise = $promise->always(static function(): void {
      IO\write_line('done');
    });

    $result = $promise->await(); // 'hello'
    // Output:
    // done
    ```

  </div>

</div>