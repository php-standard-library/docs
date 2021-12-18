# TCP

The TCP component provides a TCP server and client implementation of [Network](../network/).

## Usage

```php
use Psl\IO;
use Psl\TCP;
use Psl\Async;

$server = TCP\Server::create('localhost', 3030);

IO\write_error_line('Server is listening on tcp://localhost:3030');

while(true) {
  $connection = $server->nextConnection();

  Async\Scheduler::defer(
    function () use ($connection) {
      $request = $connection->read();

      $connection->writeAll("HTTP/1.1 200 OK\n");
      $connection->writeAll("Connection: close\n");
      $connection->writeAll("Content-Type: text/plain; charset=utf-8\n\n");
      $connection->writeAll("Hello, World!");
      $connection->close();
    }
  );
}
```

## API

### Functions

<div class="api-functions">

* [`TCP\connect(non-empty-string $host, positive-int|0 $port, ?ConnectOptions $options = null, float $timeout = null): Network\StreamSocketInterface` php]

  Connect to a TCP server.
  
  * [`$host` php]: The hostname or IP address of the server.
  * [`$port` php]: The port of the server.
  * [`$options` php]: The options to use when connecting.
  * [`$timeout` php]: The timeout in seconds.

  If failed to connect to client on the given address, [`Network\Exception\RuntimeException` php] is thrown. <br />
  If [`$timeout` php] is non-null, and the operation timed-out. [`Network\Exception\TimeoutException` php] is thrown. 

  ```php
  use Psl\IO;
  use Psl\TCP;

  $connection = TCP\connect('example.com', 80);
  $connection->writeAll("GET / HTTP/1.1\r\nHost: example.com\r\nConnection: close\r\n\r\n");
  $response = $connection->read();
  $connection->close();

  IO\write_line($response);
  ```
</div>

### Classes

<div class="api-classes">

* [`final class TCP\Server` php]

  A TCP implementation of [`Network\StreamServerInterface` php].

  <div class="api-methods">

  * [`TCP\Server::create(non-empty-string $host, positive-int $port, ?ServerOptions $options = null)` php]

    Create a new TCP server.

    * [`$host` php]: The hostname or IP address of the server.
    * [`$port` php]: The port of the server.
    * [`$options` php]: The options to use when creating the server.

    If failed to listen to on given address, [`Network\Exception\RuntimeException` php] is thrown.

    ```php
    use Psl\Network;
    use Psl\TCP;

    $server = TCP\Server::create('localhost', 3030, TCP\ServerOptions::create(
      noDelay: false,
      socketOptions: Network\SocketOptions::create(
        addressReuse: false,
        portReuse: false,
        broadcast: false,
      )
    ));
    ```

  * [`TCP\Server::nextConnection(): Network\StreamSocketInterface` php]

    Get the next connection from the server.

    If the server is closed, [`Network\Exception\AlreadyStoppedException` php] is thrown.
    If failed to accept incoming connection, [`Network\Exception\RuntimeException` php] is thrown.

    ```php
    use Psl\TCP;

    $server = TCP\Server::create('localhost', 3030);
    while(true) {
      $connection = $server->nextConnection();

      // ...
    }
    ```

  * [`TCP\Server::getLocalAddress(): Network\Address` php]

    Return the local (listening) address for the server.

    If the server is closed, [`Network\Exception\AlreadyStoppedException` php] is thrown.
    If failed to retrieve local address, [`Network\Exception\RuntimeException` php] is thrown.

    ```php
    use Psl\IO;
    use Psl\TCP;

    $server = TCP\Server::create('localhost', 3030);
    $address = $server->getLocalAddress();

    // Server address scheme: tcp
    IO\write_error_line('Server address scheme: %s', $address->scheme->value);
    // Server address host: localhost
    IO\write_error_line('Server address host: %s', $address->host);
    // Server address port: 3030
    IO\write_error_line('Server address port: %d', $address->port);
    ```
  
  * [`TCP\Server::close(): void` php]

    Stop listening; open connection are not closed.

    ```php
    use Psl\TCP;

    $server = TCP\Server::create('localhost', 3030);
    $server->close();
    ```

  * [`TCP\Server::getStream(): resource|object|null` php]

    Get the underlying server stream.

    ?> The [`TCP\Server::getStream(...)` php] method is an advanced API that many applications probably don’t need.

    If the server is closed, [`null` php] is returned.

      ```php
    use Psl\TCP;

    $server = TCP\Server::create('localhost', 3030);
    $stream = $server->getStream();

    // ...
    ```

  </div>

* [`final class TCP\ServerOptions` php]

  Options for [`TCP\Server::create(...)` php] method.

  <div class="api-methods">

  * [`static TCP\ServerOptions::create(bool $noDelay = false, Network\SocketOptions $socketOptions = new Network\SocketOptions()): TCP\ServerOptions` php]

    Create a new TCP server options.

    * [`$noDelay` php]: Whether to disable the Nagle algorithm.
    * [`$socketOptions` php]: The socket options to use.

    ```php
    use Psl\TCP;

    $options = TCP\ServerOptions::create(
      noDelay: false,
      socketOptions: Network\SocketOptions::create(
        addressReuse: false,
        portReuse: false,
        broadcast: false,
      )
    );
    ```

  * [`TCP\ServerOptions::withNoDelay(bool $noDelay = false): TCP\ServerOptions` php]

    Clone the options with the given noDelay value.

    * [`$noDelay` php]: The new no delay value.

    ```php
    use Psl\TCP;

    $options = TCP\ServerOptions::create(
      noDelay: false,
      socketOptions: Network\SocketOptions::create(
        addressReuse: false,
        portReuse: false,
        broadcast: false,
      )
    );

    $withNoDelay = $options->withNoDelay(true);
    ```
  
  * [`TCP\ServerOptions::withSocketOptions(Network\SocketOptions $socketOptions): TCP\ServerOptions` php]

    Clone the options with the given socket options.

    * [`$socketOptions` php]: The new socket options.

    ```php
    use Psl\TCP;

    $options = TCP\ServerOptions::create(
      noDelay: false,
      socketOptions: Network\SocketOptions::create(
        addressReuse: false,
        portReuse: false,
        broadcast: false,
      )
    );

    $withSocketOptions = $options->withSocketOptions(Network\SocketOptions::create(
      addressReuse: false,
      portReuse: true,
      broadcast: false,
    ));
    ```

  </div>

* [`final class TCP\ConnectOptions` php]

  Options for [`TCP\connect(...)` php] function.

  <div class="api-methods">

  * [`static TCP\ConnectOptions::create(bool $noDelay = false): TCP\ConnectOptions` php]

    Create a new TCP connect options.

    * [`$noDelay` php]: Whether to disable the Nagle algorithm.

    ```php
    use Psl\TCP;

    $options = TCP\ConnectOptions::create(
      noDelay: false,
    );
    ```
  
  * [`TCP\ConnectOptions::withNoDelay(bool $noDelay = false): TCP\ConnectOptions` php]

    Clone the options with the given noDelay value.

    * [`$noDelay` php]: The new no delay value.

    ```php
    use Psl\TCP;

    $options = TCP\ConnectOptions::create(
      noDelay: false,
    );

    $withNoDelay = $options->withNoDelay(true);
    ```

  </div>
</div>