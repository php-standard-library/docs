# Channel

## Introduction

The channel component of PSL provides a way to create a channels for communication between one, or more coroutine(s).

## Usage

```php
use Psl\Channel;

/**
 * @var Channel\ReceiverInterface<string> $receiver
 * @var Channel\SenderInterface<string> $sender
 */
[$receiver, $sender] = Channel\unbounded();

$sender->send('Hello');

$message = $receiver->receive();
```

## API

### Functions

<div class="api-functions">

* [`@template T` php] <br />
  [`Channel\bounded(int<1, max> $capacity): array{0: Channel\ReceiverInterface<T>, 1: Channel\SenderInterface<T>}` php]
    
  Create a bounded channel with the given capacity.
  
  The created channel has space to hold at most [`$capacity` php] messages at a time.
  
  * [`$capacity` php]: The capacity of the channel.

  ```php
  use Psl;
  use Psl\Channel;

  /**
   * @var Channel\SenderInterface<string> $sender
   * @var Channel\ReceiverInterface<string> $receiver
   */
  [$receiver, $sender] = Channel\bounded(1);

  $sender->send('Hello');

  Psl\invariant('Hello' === $receiver->receive(), 'Should receive "Hello"');
  ```

* [`@template T` php] <br />
  [`Channel\unbounded(): array{0: Channel\ReceiverInterface<T>, 1: Channel\SenderInterface<T>}` php]

  Creates an unbounded channel.

  The created channel can hold an unlimited number of messages.

  ```php
  use Psl;
  use Psl\Channel;

  /**
   * @var Channel\ReceiverInterface<string> $receiver
   * @var Channel\SenderInterface<string> $sender
   */
  [$receiver, $sender] = Channel\unbounded();

  $sender->send('Hello');

  Psl\invariant('Hello' === $receiver->receive(), 'Should receive "Hello"');
  ```

</div>

### Interfaces

<div class="api-interfaces">

* [`interface Channel\ChannelInterface<T> extends Countable` php]

  Common interface for all channel types.

  <div class="api-methods">

  * [`ChannelInterface::getCapacity(): ?int<0, max>` php]

    Returns the capacity of the channel, or null if the channel is unbounded.

    ```php
    use Psl;
    use Psl\Channel;

    /**
     * @var Channel\ReceiverInterface<string> $receiver
     * @var Channel\SenderInterface<string> $sender
     */
    [$receiver, $sender] = Channel\unbounded();

    Psl\invariant(null === $receiver->getCapacity(), 'Capacity should be null');

    /**
     * @var Channel\ReceiverInterface<string> $receiver
     * @var Channel\SenderInterface<string> $sender
     */
    [$receiver, $sender] = Channel\bounded(10);

    Psl\invariant(10 === $receiver->getCapacity(), 'Capacity should be 10');
    ```

  * [`ChannelInterface::isEmpty(): bool` php]

    Returns true if the channel is empty, false otherwise.

    ```php
    use Psl;
    use Psl\Channel;

    /**
     * @var Channel\ReceiverInterface<string> $receiver
     * @var Channel\SenderInterface<string> $sender
     */
    [$receiver, $sender] = Channel\unbounded();

    Psl\invariant(true === $receiver->isEmpty(), 'Channel should be empty');

    $sender->send('Hello');

    Psl\invariant(false === $receiver->isEmpty(), 'Channel should not be empty');

    $receiver->receive();

    Psl\invariant(true === $receiver->isEmpty(), 'Channel should be empty');
    ```

  * [`ChannelInterface::isFull(): bool` php]

    Returns true if the channel is full, false otherwise.

    Unbounded channels are never full.

    ```php
    use Psl;
    use Psl\Channel;

    /**
     * @var Channel\ReceiverInterface<string> $receiver
     * @var Channel\SenderInterface<string> $sender
     */
    [$receiver, $sender] = Channel\bounded(2);

    Psl\invariant(false === $receiver->isFull(), 'Channel should not be full');

    $sender->send('Hello');
    $sender->send('World');

    Psl\invariant(true === $receiver->isFull(), 'Channel should be full');

    $receiver->receive();

    Psl\invariant(false === $receiver->isFull(), 'Channel should not be full');

    /**
     * @var Channel\ReceiverInterface<string> $receiver
     * @var Channel\SenderInterface<string> $sender
     */
    [$receiver, $sender] = Channel\unbounded();

    Psl\invariant(false === $receiver->isFull(), 'Unbound channel is never full.');
    ```

  * [`ChannelInterface::count(): 0|positive-int` php]

    Returns the number of items in the channel.

    ```php
    use Psl;
    use Psl\Channel;

    /**
     * @var Channel\ReceiverInterface<string> $receiver
     * @var Channel\SenderInterface<string> $sender
     */
    [$receiver, $sender] = Channel\unbounded();

    Psl\invariant(0 === $receiver->count(), 'Channel should be empty');

    $sender->send('Hello');

    Psl\invariant(1 === $receiver->count(), 'Channel should have 1 item');

    $receiver->receive();

    Psl\invariant(0 === $receiver->count(), 'Channel should be empty');
    ```

  * [`ChannelInterface::close(): void` php]

    Closes the channel.

    The remaining messages can still be received.

    ```php
    use Psl;
    use Psl\Channel;

    /**
     * @var Channel\ReceiverInterface<string> $receiver
     * @var Channel\SenderInterface<string> $sender
     */
    [$receiver, $sender] = Channel\unbounded();

    $sender->send('Hello');

    Psl\invariant(false === $receiver->isClosed(), 'Channel should not be closed');

    $receiver->close();

    Psl\invariant(true === $receiver->isClosed(), 'Channel should be closed');

    Psl\invariant('Hello' === $receiver->receive(), 'Should receive "Hello"');
    
    try {
        $receiver->receive();
    } catch (Channel\Exception\ClosedChannelException $e) {
        // Cannot receive from a closed empty channel.
    }
    
    try {
        $sender->send('World');
    } catch (Channel\Exception\ClosedChannelException $e) {
        // Cannot send to a closed channel.
    }
    ```

  * [`ChannelInterface::isClosed(): bool` php]

    Returns true if the channel is closed.

    ```php
    use Psl;
    use Psl\Channel;

    /**
     * @var Channel\ReceiverInterface<string> $receiver
     * @var Channel\SenderInterface<string> $sender
     */
    [$receiver, $sender] = Channel\unbounded();

    Psl\invariant(false === $receiver->isClosed(), 'Channel should not be closed');
    Psl\invariant(false === $sender->isClosed(), 'Channel should not be closed');

    $receiver->close();

    Psl\invariant(true === $receiver->isClosed(), 'Channel should be closed');
    Psl\invariant(true === $sender->isClosed(), 'Channel should be closed');
    ```

  </div>

* [`interface Channel\SenderInterface<T> extends Channel\ChannelInterface<T>` php]

  Sender side of the channel

  <div class="api-methods">

  * [`SenderInterface::send(T $message): void` php]

    Sends a message to the channel, waiting if the channel is full.

    * [`$message` php]: The message to send.

    If the channel is full, this method waits until there is space for a message. <br />
    If the channel is closed, this method throws [`Channel\Exception\ClosedChannelException` php]. <br />

    ```php
    use Psl;
    use Psl\Async;
    use Psl\Channel;

    /**
     * @var Channel\ReceiverInterface<string> $receiver
     * @var Channel\SenderInterface<string> $sender
     */
    [$receiver, $sender] = Channel\bounded(1);

    // Send a message to the channel immediately.
    $sender->send('Hello');

    Async\Scheduler::delay(0.1, function () use ($receiver) {
        Psl\invariant('Hello' === $receiver->receive(), 'Should receive "Hello"');
    });

    // Send a message to the channel, waiting if the channel is full.
    $sender->send('World');
    ```

  * [`SenderInterface::trySend(T $message): void` php]

    Sends a message to the channel.

    * [`$message` php]: The message to send.

    If the channel is full, this method throws [`Channel\Exception\FullChannelException` php]. <br />
    If the channel is closed, this method throws [`Channel\Exception\ClosedChannelException` php]. <br />

    ```php
    use Psl;
    use Psl\Async;
    use Psl\Channel;

    /**
     * @var Channel\ReceiverInterface<string> $receiver
     * @var Channel\SenderInterface<string> $sender
     */
    [$receiver, $sender] = Channel\bounded(1);

    // Try to send a message to the channel immediately, without waiting.
    $sender->trySend('Hello');

    Async\Scheduler::delay(0.1, function () use ($receiver) {
        Psl\invariant('Hello' === $receiver->receive(), 'Should receive "Hello"');
    });

    try {
        // Try to send a message to the channel immediately, without waiting.
        $sender->trySend('World');
    } catch (Channel\Exception\FullChannelException $e) {
        // Cannot send to a full channel.
        // The first message 'Hello' has not been received yet.
    }
    ```

    </div>

* [`interface Channel\ReceiverInterface<T> extends Channel\ChannelInterface<T>` php]

  Receiver side of the channel

  <div class="api-methods">

  * [`ReceiverInterface::receive(): T` php]

    Receives a message from the channel, waiting if the channel is empty.

    If the channel is empty, this method waits until there is a message. <br />
    If the channel is closed and empty, this method throws [`Channel\Exception\ClosedChannelException` php]. <br />

    ```php
    use Psl;
    use Psl\Async;
    use Psl\Channel;

    /**
     * @var Channel\ReceiverInterface<string> $receiver
     * @var Channel\SenderInterface<string> $sender
     */
    [$receiver, $sender] = Channel\bounded(1);

    $sender->send('Hello');

    Async\Scheduler::delay(0.1, function () use ($sender) {
        $sender->send('World');
    });

    // Receive a message from the channel immediately.
    Psl\invariant('Hello' === $receiver->receive(), 'Should receive "Hello"');

    // Receive a message from the channel, waiting if the channel is empty.
    Psl\invariant('World' === $receiver->receive(), 'Should receive "World"');
    ```

  * [`ReceiverInterface::tryReceive(): T` php]

    Receives a message from the channel.

    If the channel is empty, this method throws [`Channel\Exception\EmptyChannelException` php]. <br />
    If the channel is closed and empty, this method throws [`Channel\Exception\ClosedChannelException` php]. <br />

    ```php
    use Psl;
    use Psl\Async;
    use Psl\Channel;

    /**
     * @var Channel\ReceiverInterface<string> $receiver
     * @var Channel\SenderInterface<string> $sender
     */
    [$receiver, $sender] = Channel\bounded(1);

    $sender->send('Hello');

    Async\Scheduler::delay(0.1, function () use ($sender) {
        $sender->send('World');
    });

    // Try to receive a message from the channel immediately, without waiting.
    Psl\invariant('Hello' === $receiver->tryReceive(), 'Should receive "Hello"');

    try {
      // Try to receive a message from the channel immediately, without waiting.
      $receiver->tryReceive();
    } catch (Channel\Exception\EmptyChannelException $e) {
        // Cannot receive from an empty channel.
        // The second message 'World' has not been sent yet.
    }
    ```

    </div>

</div>

### Exceptions

<div class="api-exceptions">

* [`final class Channel\Exception\FullChannelException implements Channel\Exception\ExceptionInterface extends Exception` php]

    Thrown when calling `Channel\SenderInterface<T>::trySend()` on a full channel.

    ```php
    use Psl;
    use Psl\Channel;

    /**
     * @var Channel\ReceiverInterface<string> $receiver
     * @var Channel\SenderInterface<string> $sender
     */
    [$receiver, $sender] = Channel\bounded(1);

    $sender->send('Hello');

    try {
        // Try to send a message to the channel immediately, without waiting.
        $sender->trySend('World');
    } catch (Channel\Exception\FullChannelException $e) {
        // Cannot send to a full channel without waiting.
    }
    ```

* [`final class Channel\Exception\EmptyChannelException implements Channel\Exception\ExceptionInterface extends Exception` php]

    Thrown when calling `Channel\ReceiverInterface<T>::tryReceive()` on an empty channel.

    ```php
    use Psl;
    use Psl\Async;
    use Psl\Channel;

    /**
     * @var Channel\ReceiverInterface<string> $receiver
     * @var Channel\SenderInterface<string> $sender
     */
    [$receiver, $sender] = Channel\bounded(1);

    try {
        // Try to receive a message from the channel immediately, without waiting.
        $receiver->tryReceive();
    } catch (Channel\Exception\EmptyChannelException $e) {
        // Cannot receive from an empty channel.
    }
    ```

* [`final class Channel\Exception\ClosedChannelException implements Channel\Exception\ExceptionInterface extends Exception` php]

    Thrown when calling attempting to send or receive on a closed channel.

    ```php
    use Psl;
    use Psl\Async;
    use Psl\Channel;

    /**
     * @var Channel\ReceiverInterface<string> $receiver
     * @var Channel\SenderInterface<string> $sender
     */
    [$receiver, $sender] = Channel\unbounded();

    $sender->send('Hello');

    $receiver->close();

    Psl\invaraint('Hello' === $receiver->receive(), 'Should receive "Hello"');

    try {
        $receiver->receive();
    } catch (Channel\Exception\ClosedChannelException $e) {
        // Cannot receive from a closed empty channel.
    }

    try {
        $sender->send('World');
    } catch (Channel\Exception\ClosedChannelException $e) {
        // Cannot send to a closed channel.
    }
    ```

</div>