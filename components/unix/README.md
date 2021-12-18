# Unix

The Unix component provides a Unix server and client implementation of [Network](../network/).

!> The Unix component is not supported on Windows platforms.

## Usage

```php
use Psl\Async;
use Psl\IO;
use Psl\Unix;
use Psl\Str;

$server = Unix\Server::create('temporary.sock');

IO\write_error_line('Server is listening on unix://temporary.sock');

while(true) {
  $connection = $server->nextConnection();

  Async\Scheduler::defer(
    function () use ($connection) {
      $request = $connection->read();

      $connection->writeAll(Str\reverse($request));

      $connection->close();
    }
  );
}
```

## API

### Functions

<div class="api-functions">

* [`Unix\connect(non-empty-string $sock, float $timeout = null): Network\StreamSocketInterface` php]

  Connect to a unix server.

  * [`$sock` php]: The unix socket to connect to.
  * [`$timeout` php]: The timeout in seconds.

  If failed to connect to client on the given address, [`Network\Exception\RuntimeException` php] is thrown. <br />
  If [`$timeout` php] is non-null, and the operation timed-out. [`Network\Exception\TimeoutException` php] is thrown. 

  ```php
  use Psl\IO;
  use Psl\Unix;

  $connection = Unix\connect('temporary.sock');
  $connection->writeAll('hello');
  $response = $connection->readAll();
  $connection->close();

  IO\write_line($response);
  ```
</div>

### Classes

<div class="api-classes">

* [`final class Unix\Server` php]

  A Unix implementation of [`Network\StreamServerInterface` php].

  <div class="api-methods">

  * [`static Unix\Server::create(non-empty-string $sock)` php]

    Create a new Unix server.

    * [`$sock` php]: The unix socket to listen on.

    If failed to listen to on given address, [`Network\Exception\RuntimeException` php] is thrown.

    ```php
    use Psl\Network;
    use Psl\Unix;

    $server = Unix\Server::create('temp.sock');
    ```

  * [`Unix\Server::nextConnection(): Network\StreamSocketInterface` php]

    Get the next connection from the server.

    If the server is closed, [`Network\Exception\AlreadyStoppedException` php] is thrown.

    ```php
    use Psl\Unix;

    $server = Unix\Server::create('temp.sock');
    while(true) {
      $connection = $server->nextConnection();

      // ...
    }
    ```

  * [`Unix\Server::getLocalAddress(): Network\Address` php]

    Return the local (listening) address for the server.

    If the server is closed, [`Network\Exception\AlreadyStoppedException` php] is thrown.
    If failed to retrieve local address, [`Network\Exception\RuntimeException` php] is thrown.

    ```php
    use Psl\IO;
    use Psl\TCP;

    $server = Unix\Server::create('temp.sock');
    $address = $server->getLocalAddress();

    // Server address scheme: unix
    IO\write_error_line('Server address scheme: %s', $address->scheme->value);
    // Server address host: /path/to/temp.sock
    IO\write_error_line('Server address host: %s', $address->host);
    ```
  
  * [`Unix\Server::close(): void` php]

    Stop listening; open connection are not closed.

    ```php
    use Psl\Unix;

    $server = Unix\Server::create('temp.sock');
    $server->close();
    ```

  * [`Unix\Server::getStream(): resource|object|null` php]

    Get the underlying server stream.

    ?> The [`Unix\Server::getStream(...)` php] method is an advanced API that many applications probably don’t need.

    If the server is closed, [`null` php] is returned.

      ```php
    use Psl\Unix;

    $server = Unix\Server::create('temp.sock');
    $stream = $server->getStream();

    // ...
    ```

  </div>
</div>