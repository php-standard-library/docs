# SecureRandom

## Introduction

The SecureRandom component provides multiple functions to generate cryptographically secure random numbers, strings, and bytes.

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

* `SecureRandom\int(int $min = Math\INT64_MIN, int $max = Math\INT64_MAX): int`

  Returns a cryptographically secure random integer in the given range.

  * `$min`: The minimum value of the random number.
  * `$max`: The maximum value of the random number.

  If `$min` is greater than `$max`, `Psl\Exception\InvarViolationException` is thrown. <br />
  If it is not possible to gather sufficient entropy, `Exception\InsufficientEntropyException` is thrown.  <br />

  The returned value is always greater than or equal to `$min` and less than or equal to `$max`.

  > This function does not cause any external mutations.

  ```php
  use Psl\SecureRandom;

  $random_number = SecureRandom\int(0, 4);

  // 0 <= $random_number <= 4
  ```

* `SecureRandom\string(0|positive-int $length, ?string $alphabet = null): string`

  Returns a securely generated random string of the given length. <br />
  The string is composed of characters from the given alphabet string. <br />

  If the alphabet argument is not specified, the returned string will be composed of the alphanumeric characters. <br />

  If the alphabet argument is specified, its length must be within the [2^1, 2^56] range. <br />

  * `$length`: The length of the string to generate.
  * `$alphabet`: The alphabet to use.

  If `$length` is 0, an empty string is returned. <br />
  If `$length` is negative, `Psl\Exception\InvalidArgumentException` is thrown. <br />
  If it is not possible to gather sufficient entropy, `Exception\InsufficientEntropyException` is thrown.  <br />

  > This function does not cause any external mutations.

  ```php
  use Psl\SecureRandom;

  $random_string = SecureRandom\string(32);

  // $random_string is a 32-character string composed of alphanumeric characters
  ```

* `SecureRandom\bytes(0|positive-int $length): string`

  Returns a securely generated random string of the given length. <br />

  If `$length` is 0, an empty string is returned. <br />
  If `$length` is negative, `Psl\Exception\InvalidArgumentException` is thrown. <br />
  If it is not possible to gather sufficient entropy, `Exception\InsufficientEntropyException` is thrown.  <br />

  > This function does not cause any external mutations.

  ```php
  use Psl\SecureRandom;

  $random_bytes = SecureRandom\bytes(32);

  // $random_bytes is a 32-byte string
  ```

* `SecureRandom\float(): float`

  Returns a cryptographically secure random float. <br />

  If it is not possible to gather sufficient entropy, `Exception\InsufficientEntropyException` is thrown.  <br />

  > This function does not cause any external mutations.

  ```php
  use Psl\SecureRandom;

  $random_float = SecureRandom\float();

  // 0.0 <= $random_float <= 1.0
  ```

### Exceptions

* `Exception\InsufficientEntropyException`

  Thrown when it is not possible to gather sufficient entropy.
