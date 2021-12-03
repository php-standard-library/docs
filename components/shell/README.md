# Shell

## Introduction

The shell component serves as replacement for [shell_exec()](https://php.net/manual/en/function.shell-exec.php), [exec()](https://php.net/manual/en/function.exec.php), [system()](https://php.net/manual/en/function.system.php) functions. 

!> The shell component is not a replacement for [amphp/process](https//github.com/amphp/process), or [symfony/process](https://github.com/symfony/process).

## Usage

```php
use Psl;
use Psl\Shell;

$result = Shell\execute('echo', arguments: ['Hello, World!']); // Hello, World!
```

## API

### Functions

* `Shell\execute(string $command, list<string> $arguments = [], ?string $working_directory = null, array<string, string> $environment = [], bool $escape_arguments = true, ?float $timeout = null): string`

  Execute an external program.

  * `$command`: The command to execute.
  * `$arguments`: The command arguments listed as separate entries.
  * `$working_directory`: The initial working directory for the command.
  * `$environment`: A dict with the environment variables for the command that will be run.
  * `$escape_arguments`: If set to true ( default ), all $arguments will be escaped using `Shell\escape_argument`.
  * `$timeout`: The timeout in seconds. If set to null, the timeout will be disabled.

  If the command returns a non-zero exit code, `Shell\Exception\FailedExecutionException` will be thrown. <br />
  If the command being run is suspicious ( e.g: contains NULL byte ), `Shell\Exception\PossibleAttackException` will be thrown.  <br />
  If the `$working_directory` doesn't exist, or unable to create a new process, `Shell\Exception\RuntimeException` will be thrown.  <br />
  If the `$timeout` is reached before being able to read the process stream, `Shell\Exception\TimeoutException` will be thrown. <br />

  ```php
  use Psl\Shell;
  use Psl\Async;

  $result = Shell\execute('echo', arguments: ['Hello, World!'], timeout: 1.0); // Hello, World!

  // execute `sleep 1` in 4 different coroutines, with a timeout of 1.2 second.
  Async\parallel([
    fn() => Shell\execute('sleep', arguments: ['1'], timeout: 1.2),
    fn() => Shell\execute('sleep', arguments: ['1'], timeout: 1.2),
    fn() => Shell\execute('sleep', arguments: ['1'], timeout: 1.2),
    fn() => Shell\execute('sleep', arguments: ['1'], timeout: 1.2),
  ]);

  try {
    Shell\execute('sleep', ['1'], timeout: 0.5);
  } catch (Shell\Exception\TimeoutException $e) {
    // The command timed out.
  }
  ```

* `Shell\escape_argument(string $argument): string`

  Escape a command argument.

  This function should only be used with combination with `Shell\execute()`, as the escape format is specific to the `Shell\execute()`.

  * `$argument`: The command argument to escape.

  ```php
  use Psl\Shell;

  $arguments = [
    $bar, // intentionally not escaped
    Shell\escape_argument($baz),
    Shell\escape_argument($qux),
  ];

  $result = Shell\execute($foo, arguments: $arguments, escape_arguments: false);
  ```
