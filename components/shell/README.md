# Shell

## Introduction

The Shell component provides the ability to run shell commands in a subprocess, in a non-blocking manner.

?> The Shell component serves as replacement for [shell_exec()](https://php.net/manual/en/function.shell-exec.php), [exec()](https://php.net/manual/en/function.exec.php), [system()](https://php.net/manual/en/function.system.php) functions.

!> The Shell component is *not* a replacement for [amphp/process](https//github.com/amphp/process), or [symfony/process](https://github.com/symfony/process).

## Usage

```php
use Psl\Shell;

$result = Shell\execute('echo', arguments: ['Hello, World!']); // Hello, World!
```

## API

### Functions

<div class="api-functions">

* [`Shell\execute(string $command, list<string> $arguments = [], ?string $working_directory = null, array<string, string> $environment = [], bool $escape_arguments = true, ?float $timeout = null): string` php]

  Execute an external program.

  * [`$command` php]: The command to execute.
  * [`$arguments` php]: The command arguments listed as separate entries.
  * [`$working_directory` php]: The initial working directory for the command.
  * [`$environment` php]: A dict with the environment variables for the command that will be run.
  * [`$escape_arguments` php]: If set to true ( default ), all [`$arguments` php] will be escaped using [`Shell\escape_argument(...)` php].
  * [`$timeout` php]: The timeout in seconds. If set to null, the timeout will be disabled.

  If the command returns a non-zero exit code, [`Shell\Exception\FailedExecutionException` php] will be thrown. <br />
  If the command being run is suspicious ( e.g: contains NULL byte ), [`Shell\Exception\PossibleAttackException` php] will be thrown.  <br />
  If the [`$working_directory` php] doesn't exist, or unable to create a new process, [`Shell\Exception\RuntimeException` php] will be thrown.  <br />
  If the [`$timeout` php] is reached before being able to read the process stream, [`Shell\Exception\TimeoutException` php] will be thrown. <br />

  ```php
  use Psl\Shell;

  $result = Shell\execute('echo', arguments: ['Hello, World!'], timeout: 1.0); // Hello, World!
  ```

  [`Shell\execute(...)` php] waits for the underlying process output in a non-blocking manner, this means that while a process is running, the script will not block, and allows other I/O operations to be performed.

  This non-blocking nature is useful when you want to execute a lot of commands concurrently, and you don't want to block the script.

  ```php
  use Psl\Async;
  use Psl\Shell;

  // execute `sleep 1` 4 times concurrently
  Async\concurrently([
    fn() => Shell\execute('sleep', arguments: ['1']),
    fn() => Shell\execute('sleep', arguments: ['1']),
    fn() => Shell\execute('sleep', arguments: ['1']),
    fn() => Shell\execute('sleep', arguments: ['1']),
  ]);
  ```

  All commands in the example above will be executed concurrently, and the script will finish in ~1 second.

  We can also set a timeout for the read operation, and if the process is still running after the timeout, [`Shell\Exception\TimeoutException` php] will be thrown.

  ```php
  use Psl\Shell;

  try {
    Shell\execute('sleep', ['1'], timeout: 0.5);
  } catch (Shell\Exception\TimeoutException $e) {
    // The command timed out.
  }
  ```

* [`Shell\escape_argument(string $argument): string` php]

  Escape a command argument.

  This function should only be used in combination with [`Shell\execute(...)` php], as the escape format is specific to the [`Shell\execute(...)` php] function.

  * [`$argument` php]: The command argument to escape.

  ```php
  use Psl\Shell;

  $arguments = [
    $bar, // intentionally not escaped
    Shell\escape_argument($baz),
    Shell\escape_argument($qux),
  ];

  $result = Shell\execute($foo, arguments: $arguments, escape_arguments: false);
  ```

</div>

### Exceptions

<div class="api-exceptions">

* [`final class Shell\Exception\FailedExecutionException implements Shell\Exception\ExceptionInterface extends Execption` php]

  Thrown when the command returns a non-zero exit code.

  ```php
  use Psl\Shell;

  try {
    Shell\execute('php', arguments: ['-r', 'exit(1);'], timeout: 1.0);
  } catch (Shell\Exception\FailedExecutionException $e) {
    $exit_code = $e->getCode();
    $stdout_content = $e->getOutput();
    $stderr_content = $e->getErrorOutput();

    // ... do something.
  }

  ```

* [`final class Shell\Exception\PossibleAttackException implements Shell\Exception\ExceptionInterface extends Execption` php]

  Thrown when the command being run is suspicious ( e.g: contains NULL byte ).

  ```php
  use Psl\Shell;

  try {
    Shell\execute('echo', arguments: ["Hello, World!\0"]);
  } catch (Shell\Exception\PossibleAttackException $e) {
    // ... do something.
  }
  ```

* [`final class Shell\Exception\RuntimeException implements Shell\Exception\ExceptionInterface extends Execption` php]

  Thrown when the [`$working_directory` php] doesn't exist, or unable to create a new process.

  ```php
  use Psl\Shell;

  try {
    Shell\execute('echo', arguments: ['Hello, World!'], working_directory: '/foo/bar');
  } catch (Shell\Exception\RuntimeException $e) {
    // ... do something.
  }
  ```

* [`final class Shell\Exception\TimeoutException` implements Shell\Exception\ExceptionInterface extends Execption` php]

  Thrown when the [`$timeout` php] is reached before being able to read the process stream.

  ```php
  use Psl\Shell;

  try {
    Shell\execute('sleep', ['1'], timeout: 0.5);
  } catch (Shell\Exception\TimeoutException $e) {
    // ... do something.
  }
  ```

</div>
