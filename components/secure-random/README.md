# SecureRandom

## Introduction

The SecureRandom component is a set of multiple functions to generate cryptographically secure random numbers, strings, and bytes.

?> The SecureRandom component is meant as a direct replacement for PHP [`random_bytes()`](https://secure.php.net/manual/en/function.random-bytes.php) and [`random_int()`](https://secure.php.net/manual/en/function.random-int.php) functions.

## Usage

```php
use Psl\SecureRandom;

$random_number = SecureRandom\int();
$random_float = SecureRandom\float();
$random_byte = SecureRandom\bytes(256);
$random_string = SecureRandom\string(256);
```

## API

### Functions

<div class="api-functions">

* [`@external-mutation-free` php] <br/>
  [`SecureRandom\int(int $min = Math\INT64_MIN, int $max = Math\INT64_MAX): int` php]

  Returns a cryptographically secure random integer in the given range.

  * [`$min` php]: The minimum value of the random number.
  * [`$max` php]: The maximum value of the random number.

  If [`$min` php] is greater than [`$max` php], [`Psl\Exception\InvariantViolationException` php] is thrown. <br />
  If it is not possible to gather sufficient entropy, [`SecureRandom\Exception\InsufficientEntropyException` php] is thrown.  <br />

  The returned value is always greater than or equal to [`$min` php] and less than or equal to [`$max` php].

  ```php
  use Psl\SecureRandom;

  $random_number = SecureRandom\int(0, 4);

  // 0 <= $random_number <= 4
  ```

* [`@external-mutation-free` php] <br/>
  [`SecureRandom\string(0|positive-int $length, ?string $alphabet = null): string` php]

  Returns a securely generated random string of the given length. <br />
  The string is composed of characters from the given alphabet string. <br />

  If the alphabet argument is not specified, the returned string will be composed of the alphanumeric characters. <br />

  If the alphabet argument is specified, its length must be within the [2^1, 2^56] range. <br />

  * [`$length` php]: The length of the string to generate.
  * [`$alphabet` php]: The alphabet to use.

  If [`$length` php] is negative, [`Psl\Exception\InvariantViolationException` php] is thrown. <br />
  If it is not possible to gather sufficient entropy, [`SecureRandom\Exception\InsufficientEntropyException` php] is thrown.  <br />

  ```php
  use Psl\SecureRandom;

  $random_string = SecureRandom\string(32);

  // $random_string is a 32-character string composed of alphanumeric characters
  ```

* [`@external-mutation-free` php] <br/>
  [`SecureRandom\bytes(0|positive-int $length): string` php]

  Returns a securely generated random string of the given length. <br />

  * [`$length` php]: The length of the string to generate.

  If [`$length` php] is negative, [`Psl\Exception\InvariantViolationException` php] is thrown. <br />
  If it is not possible to gather sufficient entropy, [`SecureRandom\Exception\InsufficientEntropyException` php] is thrown.  <br />

  ```php
  use Psl\SecureRandom;

  $random_bytes = SecureRandom\bytes(32);

  // $random_bytes is a 32-byte string
  ```

* [`@external-mutation-free` php] <br/>
  [`SecureRandom\float(): float` php]

  Returns a cryptographically secure random float. <br />

  If it is not possible to gather sufficient entropy, [`SecureRandom\Exception\InsufficientEntropyException` php] is thrown.  <br />

  ```php
  use Psl\SecureRandom;

  $random_float = SecureRandom\float();

  // 0.0 <= $random_float <= 1.0
  ```

</div>

### Exceptions

<div class="api-exceptions">

* [`final class SecureRandom\Exception\InsufficientEntropyException implements SecureRandom\Exception\ExceptionInterface extends Exception` php]

  Thrown when it is not possible to gather sufficient entropy.

</div>
