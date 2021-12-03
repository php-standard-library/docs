# Channel

## Introduction

The channel component of PSL provides a way to create a channels for communication between one, or more coroutine(s).

## Usage

```php
use Psl\Channel;

/**
 * @var Channel\SenderInterface<string> $sender
 * @var Channel\ReceiverInterface<string> $reciver
 */
[$reciver, $sender] = Channel\unbounded();

$sender->send('Hello');

$message = $reciver->receive();
```

## API

### Functions

* `Channel\bounded<T>(positive-int $capacity): array{0: Channel\ReceiverInterface<T>, 1: Channel\SenderInterface<T>}`

    Create a bounded channel with the given capacity.

    The created channel has space to hold at most `$capacity` messages at a time.

    ```php
    use Psl;
    use Psl\Channel;

    /**
     * @var Channel\SenderInterface<string> $sender
     * @var Channel\ReceiverInterface<string> $reciver
     */
    [$reciver, $sender] = Channel\bounded(1);

    $sender->send('Hello');

    Psl\invariant('Hello' === $reciver->receive(), 'Should receive "Hello"');
    ```

* `Channel\unbounded<T>(): array{0: Channel\ReceiverInterface<T>, 1: Channel\SenderInterface<T>}`

    Creates an unbounded channel.

    The created channel can hold an unlimited number of messages.

    ```php
    use Psl;
    use Psl\Channel;

    /**
     * @var Channel\SenderInterface<string> $sender
     * @var Channel\ReceiverInterface<string> $reciver
     */
    [$reciver, $sender] = Channel\unbounded();

    $sender->send('Hello');

    Psl\invariant('Hello' === $reciver->receive(), 'Should receive "Hello"');
    ```

### Interfaces

* `Channel\ChannelInterface<T>`

  Common interface for all channel types.

  * `ChannelInterface::getCapacity(): null|0|positive-int`

    Returns the capacity of the channel, or null if the channel is unbounded.

    ```php
    use Psl;
    use Psl\Channel;

    /**
     * @var Channel\SenderInterface<string> $sender
     * @var Channel\ReceiverInterface<string> $reciver
     */
    [$reciver, $sender] = Channel\unbounded();

    Psl\invariant(null === $reciver->getCapacity(), 'Capacity should be null');

    /**
     * @var Channel\SenderInterface<string> $sender
     * @var Channel\ReceiverInterface<string> $reciver
     */
    [$reciver, $sender] = Channel\bounded(10);

    Psl\invariant(10 === $reciver->getCapacity(), 'Capacity should be 10');
    ```

  * `ChannelInterface::isEmpty(): bool`

    Returns true if the channel is empty, false otherwise.

    ```php
    use Psl;
    use Psl\Channel;

    /**
     * @var Channel\SenderInterface<string> $sender
     * @var Channel\ReceiverInterface<string> $reciver
     */
    [$reciver, $sender] = Channel\unbounded();

    Psl\invariant(true === $reciver->isEmpty(), 'Channel should be empty');

    $sender->send('Hello');

    Psl\invariant(false === $reciver->isEmpty(), 'Channel should not be empty');

    $reciver->receive();

    Psl\invariant(true === $reciver->isEmpty(), 'Channel should be empty');
    ```

  * `ChannelInterface::isFull(): bool`

    Returns true if the channel is full, false otherwise.

    Unbounded channels are never full.

    ```php
    use Psl;
    use Psl\Channel;

    /**
     * @var Channel\SenderInterface<string> $sender
     * @var Channel\ReceiverInterface<string> $reciver
     */
    [$reciver, $sender] = Channel\bounded(2);

    Psl\invariant(false === $reciver->isFull(), 'Channel should not be full');

    $sender->send('Hello');
    $sender->send('World');

    Psl\invariant(true === $reciver->isFull(), 'Channel should be full');

    $reciver->receive();

    Psl\invariant(false === $reciver->isFull(), 'Channel should not be full');

    /**
     * @var Channel\SenderInterface<string> $sender
     * @var Channel\ReceiverInterface<string> $reciver
     */
    [$reciver, $sender] = Channel\unbounded();

    Psl\invariant(false === $reciver->isFull(), 'Unbound channel is never full.');
    ```

  * `ChannelInterface::count(): 0|positive-int`

    Returns the number of items in the channel.

    ```php
    use Psl;
    use Psl\Channel;

    /**
     * @var Channel\SenderInterface<string> $sender
     * @var Channel\ReceiverInterface<string> $reciver
     */
    [$reciver, $sender] = Channel\unbounded();

    Psl\invariant(0 === $reciver->count(), 'Channel should be empty');

    $sender->send('Hello');

    Psl\invariant(1 === $reciver->count(), 'Channel should have 1 item');

    $reciver->receive();

    Psl\invariant(0 === $reciver->count(), 'Channel should be empty');
    ```

  * `ChannelInterface::close(): void`

    Closes the channel.

    The remaining messages can still be received.

    ```php
    use Psl;
    use Psl\Channel;

    /**
     * @var Channel\SenderInterface<string> $sender
     * @var Channel\ReceiverInterface<string> $reciver
     */
    [$reciver, $sender] = Channel\unbounded();

    $sender->send('Hello');

    Psl\invariant(false === $reciver->isClosed(), 'Channel should not be closed');

    $reciver->close();

    Psl\invariant(true === $reciver->isClosed(), 'Channel should be closed');

    Psl\invariant('Hello' === $reciver->receive(), 'Should receive "Hello"');
    
    try {
        $reciver->receive();
    } catch (Channel\Exception\ClosedChannelException $e) {
        // Cannot recieve from a closed empty channel.
    }
    
    try {
        $sender->send('World');
    } catch (Channel\Exception\ClosedChannelException $e) {
        // Cannot send to a closed channel.
    }
    ```

  * `ChannelInterface::isClosed(): bool`

    Returns true if the channel is closed.

    ```php
    use Psl;
    use Psl\Channel;

    /**
     * @var Channel\SenderInterface<string> $sender
     * @var Channel\ReceiverInterface<string> $reciver
     */
    [$reciver, $sender] = Channel\unbounded();

    Psl\invariant(false === $reciver->isClosed(), 'Channel should not be closed');
    Psl\invariant(false === $sender->isClosed(), 'Channel should not be closed');

    $reciver->close();

    Psl\invariant(true === $reciver->isClosed(), 'Channel should be closed');
    Psl\invariant(true === $sender->isClosed(), 'Channel should be closed');
    ```

* `Channel\SenderInterface<T> extends Channel\ChannelInterface<T>`

  Sender side of the channel

  * `SenderInterface<T>::send(T $message): void`

    Sends a message to the channel, waiting if the channel is full.

    If the channel is full, this method waits until there is space for a message.
    If the channel is closed, this method throws `Channel\Exception\ClosedChannelException`.

    ```php
    use Psl;
    use Psl\Async;
    use Psl\Channel;

    /**
     * @var Channel\SenderInterface<string> $sender
     * @var Channel\ReceiverInterface<string> $reciver
     */
    [$reciver, $sender] = Channel\bounded(1);

    // Send a message to the channel immediately.
    $sender->send('Hello');

    Async\Scheduler::delay(0.1, function () use ($reciver) {
        Psl\invariant('Hello' === $reciver->receive(), 'Should receive "Hello"');
    });

    // Send a message to the channel, waiting if the channel is full.
    $sender->send('World');
    ```

  * `SenderInterface<T>::trySend(T $message): void`

    Sends a message to the channel.

    If the channel is full, this method throws `Channel\Exception\FullChannelException`.
    If the channel is closed, this method throws `Channel\Exception\ClosedChannelException`.

    ```php
    use Psl;
    use Psl\Async;
    use Psl\Channel;

    /**
     * @var Channel\SenderInterface<string> $sender
     * @var Channel\ReceiverInterface<string> $reciver
     */
    [$reciver, $sender] = Channel\bounded(1);

    // Try to send a message to the channel immediately, without waiting.
    $sender->trySend('Hello');

    Async\Scheduler::delay(0.1, function () use ($reciver) {
        Psl\invariant('Hello' === $reciver->receive(), 'Should receive "Hello"');
    });

    try {
        // Try to send a message to the channel immediately, without waiting.
        $sender->trySend('World');
    } catch (Channel\Exception\FullChannelException $e) {
        // Cannot send to a full channel.
        // The first message 'Hello' has not been received yet.
    }
    ```

* `Channel\ReceiverInterface<T> extends Channel\ChannelInterface<T>`

  Receiver side of the channel

  * `ReceiverInterface<T>::receive(): T`

    Receives a message from the channel, waiting if the channel is empty.

    If the channel is empty, this method waits until there is a message.
    If the channel is closed and empty, this method throws `Channel\Exception\ClosedChannelException`.

    ```php
    use Psl;
    use Psl\Async;
    use Psl\Channel;

    /**
     * @var Channel\SenderInterface<string> $sender
     * @var Channel\ReceiverInterface<string> $reciver
     */
    [$reciver, $sender] = Channel\bounded(1);

    $sender->send('Hello');

    Async\Scheduler::delay(0.1, function () use ($sender) {
        $sender->send('World');
    });

    // Receive a message from the channel immediately.
    Psl\invariant('Hello' === $reciver->receive(), 'Should receive "Hello"');

    // Receive a message from the channel, waiting if the channel is empty.
    Psl\invariant('World' === $reciver->receive(), 'Should receive "World"');
    ```

  * `ReceiverInterface<T>::tryReceive(): ?T`

    Receives a message from the channel.

    If the channel is empty, this method throws `Channel\Exception\EmptyChannelException`.
    If the channel is closed and empty, this method throws `Channel\Exception\ClosedChannelException`.

    ```php
    use Psl;
    use Psl\Async;
    use Psl\Channel;

    /**
     * @var Channel\SenderInterface<string> $sender
     * @var Channel\ReceiverInterface<string> $reciver
     */
    [$reciver, $sender] = Channel\bounded(1);

    $sender->send('Hello');

    Async\Scheduler::delay(0.1, function () use ($sender) {
        $sender->send('World');
    });

    // Try to receive a message from the channel immediately, without waiting.
    Psl\invariant('Hello' === $reciver->tryReceive(), 'Should receive "Hello"');

    try {
      // Try to receive a message from the channel immediately, without waiting.
      $reciver->tryReceive();
    } catch (Channel\Exception\EmptyChannelException $e) {
        // Cannot receive from an empty channel.
        // The second message 'World' has not been sent yet.
    }
    ```

### Exceptions

* `Channel\Exception\FullChannelException`

    Thrown when calling `Channel\SenderInterface<T>::trySend()` on a full channel.

    ```php
    use Psl;
    use Psl\Channel;

    /**
     * @var Channel\SenderInterface<string> $sender
     * @var Channel\ReceiverInterface<string> $reciver
     */
    [$reciver, $sender] = Channel\bounded(1);

    $sender->send('Hello');

    try {
        // Try to send a message to the channel immediately, without waiting.
        $sender->trySend('World');
    } catch (Channel\Exception\FullChannelException $e) {
        // Cannot send to a full channel without waiting.
    }
    ```

* `Channel\Exception\EmptyChannelException`

    Thrown when calling `Channel\ReceiverInterface<T>::tryReceive()` on an empty channel.

    ```php
    use Psl;
    use Psl\Async;
    use Psl\Channel;

    /**
     * @var Channel\SenderInterface<string> $sender
     * @var Channel\ReceiverInterface<string> $reciver
     */
    [$reciver, $sender] = Channel\bounded(1);

    try {
        // Try to receive a message from the channel immediately, without waiting.
        $reciver->tryReceive();
    } catch (Channel\Exception\EmptyChannelException $e) {
        // Cannot receive from an empty channel.
    }
    ```

* `Channel\Exception\ClosedChannelException`

    Thrown when calling attempting to send or receive on a closed channel.

    ```php
    use Psl;
    use Psl\Async;
    use Psl\Channel;

    /**
     * @var Channel\SenderInterface<string> $sender
     * @var Channel\ReceiverInterface<string> $reciver
     */
    [$reciver, $sender] = Channel\unbounded();

    $sender->send('Hello');

    $reciver->close();

    Psl\invaraint('Hello' === $reciver->receive(), 'Should receive "Hello"');

    try {
        $reciver->receive();
    } catch (Channel\Exception\ClosedChannelException $e) {
        // Cannot recieve from a closed empty channel.
    }

    try {
        $sender->send('World');
    } catch (Channel\Exception\ClosedChannelException $e) {
        // Cannot send to a closed channel.
    }
    ```