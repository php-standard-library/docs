# Math

## Introduction

The Math component provides a set of functions for performing mathematical operations.

## Usage

```php
use Psl\Math;

$a = Math\sin(2); // sin(2)
$b = Math\cos(2); // cos(2)
$c = Math\tan(2); // tan(2)
```

## API

### Constants

<div class="api-constants">

* [`const Math\INFINITY` php]

  The value of `INFINITY` is `1 / 0` (positive infinity).

* [`const Math\NAN` php]

  The value of `NAN` is `0 / 0` (not a number).

* [`const Math\E` php]

  The base of the natural system of logarithms, or approximately 2.7182818284590452353602875.

* [`const Math\PI` php]

  The ratio of the circumference of a circle to its diameter, approximately 3.141592653589793238462643.

* [`const Math\INT64_MAX` php]

  The maximum integer value representable in a 64-bit binary-coded decimal.

* [`const Math\INT64_MIN` php]

  The minimum integer value representable in a 64-bit binary-coded decimal.

* [`const Math\INT32_MAX` php]

  The maximum integer value representable in a 32-bit binary-coded decimal.

* [`const Math\INT32_MIN` php]

  The minimum integer value representable in a 32-bit binary-coded decimal.

* [`const Math\INT16_MAX` php]

  The maximum integer value representable in a 16-bit binary-coded decimal.

* [`const Math\INT16_MIN` php]

  The minimum integer value representable in a 16-bit binary-coded decimal.

* [`const Math\UINT32_MAX` php]

  The maximum unsigned integer value representable in a 32-bit binary-coded decimal.

* [`const Math\UINT16_MAX` php]

  The maximum unsigned integer value representable in a 16-bit binary-coded decimal.

</div>

### Functions

<div class="api-functions">

* [`@pure` php] <br />
  [`Math\sin(float $number): float` php]

  Returns the sine of the given number.

  * [`$number` php]: The number to calculate the sine of.

  ```php
  use Psl\Math;

  $a = Math\sin(5.0); // -0.9589242746631385
  ```

* [`@pure` php] <br />
  [`Math\cos(float $number): float` php]

  Returns the cosine of the given number.

  * [`$number` php]: The number to calculate the cosine of.

  ```php
  use Psl\Math;

  $a = Math\cos(1.0); // 0.5403023058681398
  ```

* [`@pure` php] <br />
  [`Math\tan(float $number): float` php]

  Returns the tangent of the given number.

  * [`$number` php]: The number to calculate the tangent of.

  ```php
  use Psl\Math;

  $a = Math\tan(4.8); // -11.384870654242922
  ```

* [`@pure` php] <br />
  [`Math\asin(float $number): float` php]

  Returns the arc sine of the given number.

  * [`$number` php]: The number to calculate the arc sine of.

  ```php
  use Psl\Math;

  $a = Math\asin(0.5); // 0.5235987755982989
  ```

* [`@pure` php] <br />
  [`Math\acos(float $number): float` php]

  Returns the arc cosine of the given number.

  * [`$number` php]: The number to calculate the arc cosine of.

  ```php
  use Psl\Math;

  $a = Math\acos(0.5); // 1.0471975511965979
  ```

* [`@pure` php] <br />
  [`Math\atan(float $number): float` php]

  Returns the arc tangent of the given number.

  * [`$number` php]: The number to calculate the arc tangent of.

  ```php
  use Psl\Math;

  $a = Math\atan(-0.5); // -0.4636476090008061
  ```

* [`@pure` php] <br />
  [`Math\atan2(float $y, float $x): float` php]

  Returns the arc tangent of the given coordinates.

  * [`$y` php]: The y-coordinate of the point.

  * [`$x` php]: The x-coordinate of the point.

  ```php
  use Psl\Math;

  $a = Math\atan2(1.0, 0.8); // 0.8960553845713439
  ```

* [`@template T of int|float` php] <br />
  [`@pure` php] <br />
  [`Math\abs(T $number): T` php]

  Returns the absolute value of the given number.

  * [`$number` php]: The number to calculate the absolute value of.

  ```php
  use Psl\Math;

  $a = Math\abs(2);     // 2
  $b = Math\abs(-2);    // 2
  $c = Math\abs(2.5);   // 2.5
  $d = Math\abs(-2.5);  // 2.5
  ```

* [`@pure` php] <br />
  [`Math\ceil(float $number): float` php]

  Returns the smallest integer greater than or equal to the given number.

  * [`$number` php]: The number to calculate the ceiling of.

  ```php
  use Psl\Math;

  $a = Math\ceil(2);      // 2.0
  $b = Math\ceil(-2);     // -2.0
  $c = Math\ceil(2.5);    // 3.0
  $d = Math\ceil(-2.5);   // -2.0
  ```

* [`@pure` php] <br />
  [`Math\floor(float $number): float` php]

  Returns the largest integer less than or equal to the given number.

  * [`$number` php]: The number to calculate the floor of.

  ```php
  use Psl\Math;

  $a = Math\floor(2);     // 2.0
  $b = Math\floor(-2);    // -2.0
  $c = Math\floor(2.5);   // 2.0
  $d = Math\floor(-2.5);  // -3.0
  ```

* [`@pure` php] <br />
  [`Math\round(float $number, int $precision = 0): float` php]

  Returns the given number rounded to the specified precision.

  A positive precision rounds to the nearest decimal place whereas a negative precision rounds to the nearest power of ten.

  For example, a precision of 1 rounds to the nearest tenth whereas a precision of -1 rounds to the nearst nearest.

  * [`$number` php]: The number to round.

  ```php
  use Psl\Math;

  $a = Math\round(2.5);       // 3.0
  $b = Math\round(-2.5);      // -2.0

  $c = Math\round(2.5, 1);    // 2.5
  $d = Math\round(-2.5, 1);   // -2.5

  $e = Math\round(2.5, -1);   // 3.0
  $f = Math\round(-2.5, -1);  // -3.0
  ```

* [`@pure` php] <br />
  [`Math\sqrt(float $number): float` php]

  Returns the square root of the given number.

  * [`$number` php]: The number to calculate the square root of.

  If the given number is negative, [`Psl\Exception\InvariantViolationException` php] is thrown.

  ```php
  use Psl\Math;

  $a = Math\sqrt(2); // 1.4142135623730951
  ```

* [`@pure` php] <br />
  [`Math\log(float $number, ?float $base = null): float` php]

  Returns the logarithm of the given number.

  * [`$number` php]: The number to calculate the logarithm of.

  If the given number is equal to, or less than, zero, [`Psl\Exception\InvariantViolationException` php] is thrown. <br />
  If the given base is equal to, or less than, zero, [`Psl\Exception\InvariantViolationException` php] is thrown. <br />
  If the given base is equal to one, [`Psl\Exception\InvariantViolationException` php] is thrown.

  ```php
  use Psl\Math;

  $a = Math\log(2); // 0.6931471805599453
  $b = Math\log(2, 2); // 1.0
  ```

* [`@pure` php] <br />
  [`Math\exp(float $number): float` php]

  Returns the exponential of the given number.

  * [`$number` php]: The number to calculate the exponential of.

  ```php
  use Psl\Math;

  $a = Math\exp(2); // 7.38905609893065
  ```

* [`@pure` php] <br />
  [`Math\div(int $numerator, int $denominator): int` php]

  Returns the integer division of the given numerator by the given denominator.

  * [`$numerator` php]: The numerator.
  * [`$denominator` php]: The denominator.

  If the given denominator is zero, `Exception\DivisionByZeroException` is thrown. <br />
  If the given numerator is equal to `Math\INT64_MIN` and the given denominator is equal to `-1`, `Exception\ArithmeticException` is thrown.

  ```php
  use Psl\Math;

  $a = Math\div(2, 2);    // 1
  $b = Math\div(2, 3);    // 0
  $d = Math\div(3, 3);    // 1
  $d = Math\div(30, 3);   // 10
  ```

* [`@pure` php] <br />
  [`Math\sum(list<int> $numbers): int` php]

  Returns the sum of all the given numbers.

  * [`$numbers` php]: The numbers to sum.

  ```php
  use Psl\Math;

  $a = Math\sum([1, 2, 3]);     // 6
  $b = Math\sum([1, 2, 3, 4]);  // 10
  ```

* [`@pure` php] <br />
  [`Math\sum_floats(list<float|int> $numbers): float` php]

  Returns the sum of all the given numbers.

  * [`$numbers` php]: The numbers to sum.

  ```php
  use Psl\Math;

  $a = Math\sum_floats([1.1, 2.2, 3.3]);      // 6.6
  $b = Math\sum_floats([1.1, 2.2, 3.3, 4.4]); // 10.4
  ```

* [`@template T of int|float` php] <br />
  [`@pure` php] <br />
  [`Math\clamp(T $number, T $min, T $max): T` php]

  Returns the given number clamped to the given range.

  * [`$number` php]: The number to clamp.
  * [`$min` php]: The minimum value.
  * [`$max` php]: The maximum value.

  If the given number is less than the given minimum, the minimum is returned. <br />
  If the given number is greater than the given maximum, the maximum is returned. <br />

  If [`$min` php] is greater than [`$max` php], [`Psl\Exception\InvariantViolationException` php] is thrown.

  ```php
  use Psl\Math;

  $a = Math\clamp(2, 1, 3); // 2
  $b = Math\clamp(1, 1, 3); // 1
  $c = Math\clamp(3, 1, 3); // 3
  ```

* [`@pure` php] <br />
  [`Math\base_convert(non-empty-string $value, int $from_base, int $to_base): string` php]

  Converts the given string in base [`$from_base` php] to base [`$to_base` php], assuming letters a-z are used for digits for bases greater than 10. <br />
  The conversion is done to arbitrary precision. <br />

  * [`$value` php]: The string to convert.
  * [`$from_base` php]: The base of the given string.
  * [`$to_base` php]: The base to convert to.

  If [`$value` php] is not a valid base [`$from_base` php] number, [`Psl\Exception\InvariantViolationException` php] is thrown. <br />
  If [`$from_base` php] and [`$to_base` php] are out of the [2, 36] range, [`Psl\Exception\InvariantViolationException` php] is thrown. <br />

  ```php
  use Psl\Math;

  $a = Math\base_convert('101', 2, 10); // '5'
  $b = Math\base_convert('5', 10, 2); // '101'

  $c = Math\base_convert('2014587925987', 10, 36); // 'pphlmw9v'
  $d = Math\base_convert('pphlmw9v', 36, 10); // '2014587925987'
  ```

* [`@pure` php] <br />
  [`Math\from_base(non-empty-string $number, int $from_base): int` php]

    Converts the given string in base [`$from_base` php] to an integer,  assuming letters a-z are used for digits when [`$from_base` php] > 10. <br />

    * [`$number` php]: The string to convert.
    * [`$from_base` php]: The base of the given string.

    If [`$number` php] is not a valid base [`$from_base` php] number, [`Psl\Exception\InvariantViolationException` php] is thrown. <br />
    If [`$from_base` php] is out of the [2, 36] range, [`Psl\Exception\InvariantViolationException` php] is thrown. <br />

    ```php
    use Psl\Math;

    $a = Math\from_base('101', 2); // 5
    $b = Math\from_base('5', 10); // 5
    $c = Math\from_base('pphlmw9v', 36); // 2014587925987
    ```

* [`@pure` php] <br />
  [`Math\to_base(0|positive-int $number, positive-int $base): non-empty-string` php]

    Converts the given non-negative integer to a string in base [`$base` php], using letters a-z for digits when [`$base` php] > 10. <br />

    * [`$number` php]: The integer to convert.
    * [`$base` php]: The base to convert to.

    If [`$base` php] is out of the [2, 36] range, [`Psl\Exception\InvariantViolationException` php] is thrown. <br />
    If [`$number` php] is negative, [`Psl\Exception\InvariantViolationException` php] is thrown. <br />

    ```php
    use Psl\Math;

    $a = Math\to_base(5, 2); // '101'
    $b = Math\to_base(5, 10); // '5'
    $c = Math\to_base(2014587925987, 36); // 'pphlmw9v'
    ```

* [`@template T of int|float` php] <br />
  [`@pure` php] <br />
  [`Math\max(list<T> $numbers): ?T` php]

  Returns the largest element of the given list, or null if the list is empty.

  * [`$numbers` php]: The values to compare.

  ```php
  use Psl\Math;

  $a = Math\max([1, 2, 3]);     // 3
  $b = Math\max([1, 2, 3, 4]);  // 4
  $c = Math\max([]);            // null
  ```

* [`@template T of int|float` php] <br />
  [`@pure` php] <br />
  [`Math\min(list<T> $numbers): ?T` php]

  Returns the smallest element of the given list, or null if the list is empty.

  * [`$numbers` php]: The values to compare.

  ```php
  use Psl\Math;

  $a = Math\min([1, 2, 3]);     // 1
  $b = Math\min([1, 2, 3, 4]);  // 1
  $c = Math\min([]);            // null
  ```

* [`@template T` php] <br />
  [`Math\max_by(iterable<T> $numbers, (Closure(T $value): numeric) $numeric_function): ?T` php]

  Returns the largest element of the given iterable, or null if the iterable is empty. <br />
  The value for comparison is determined by the given function. <br />
  In the case of duplicate values, later values overwrite previous ones. <br />

  * [`$numbers` php]: The values to compare.
  * [`$numeric_function` php]: A function that returns the numeric value used for comparison.

  ```php
  use Psl\Math;

  $a = Math\max_by([1, 2, 3], static fn (int $value): int => $value * 2);     // 6
  $b = Math\max_by([1, 2, 3, 4], static fn (int $value): int => $value * 2);  // 8
  $c = Math\max_by([]);                                                       // null
  ```

* [`@template T` php] <br />
  [`Math\min_by(iterable<T> $numbers, (Closure(T $value): numeric) $numeric_function): ?T` php]

  Returns the smallest element of the given iterable, or null if the iterable is empty. <br />
  The value for comparison is determined by the given function. <br />
  In the case of duplicate values, later values overwrite previous ones. <br />

  * [`$numbers` php]: The values to compare.
  * [`$numeric_function` php]: A function that returns the numeric value used for comparison.

  ```php
  use Psl\Math;

  $a = Math\min_by([1, 2, 3], static fn (int $value): int => $value * 2);     // 2
  $b = Math\min_by([1, 2, 3, 4], static fn (int $value): int => $value * 2);  // 2
  $c = Math\min_by([]);                                                       // null
  ```

* [`@template T of int|float` php] <br />
  [`@pure` php] <br />
  [`Math\maxva(T $first, T $second, T ...$rest): T` php]

  Returns the largest number of all the given numbers.

  * [`$first` php]: The first value to compare.
  * [`$second` php]: The second value to compare.
  * [`$rest` php]: The rest of the values to compare.

  ```php
  use Psl\Math;

  $a = Math\maxva(1, 2, 3);     // 3
  $b = Math\maxva(1, 2, 3, 4);  // 4
  $c = Math\maxva(1, 2);        // 2
  ```

* [`@template T of int|float` php] <br />
  [`@pure` php] <br />
  [`Math\minva(T $first, T $second, T ...$rest): T` php]

  Returns the smallest number of all the given numbers.

  * [`$first` php]: The first value to compare.
  * [`$second` php]: The second value to compare.
  * [`$rest` php]: The rest of the values to compare.

  ```php
  use Psl\Math;

  $a = Math\minva(1, 2, 3);     // 1
  $b = Math\minva(1, 2, 3, 4);  // 1
  $c = Math\minva(1, 2);        // 1
  ```

* [`@pure` php] <br />
  [`Math\mean(list<int|float> $numbers): ?float` php]

  Returns the arithmetic mean of the given numbers in the list. <br />
  Returns null if the given iterable is empty.

  * [`$numbers` php]: The values to calculate the mean from.

  ```php
  use Psl\Math;

  $a = Math\mean([1, 2, 3]);     // 2
  $b = Math\mean([20, 15, 31]);  // 22
  $c = Math\mean([]);            // null
  ```

* [`@pure` php] <br />
  [`Math\median(list<int|float> $numbers): ?float` php]

  Returns the median of the given numbers in the list. <br />
  Returns null if the given iterable is empty.

  * [`$numbers` php]: The values to calculate the median from.

  ```php
  use Psl\Math;

  $a = Math\median([1, 2, 3]);     // 2
  $b = Math\median([20, 15, 31]);  // 20
  $c = Math\median([]);            // null
  ```

</div>

### Exceptions

<div class="api-exceptions">

* [`final class Math\Exception\DivisionByZeroException implements Math\Exception\ExceptionInterface extends Exception` php]

  Thrown when division by zero is attempted.

  ```php
  use Psl\Math;

  try {
      Math\div(1, 0);
  } catch (Math\Exception\DivisionByZeroException $e) {
      // ... handle exception
  }
  ```

* [`final class Math\Exception\ArithmeticException implements Math\Exception\ExceptionInterface extends Exception` php]

  Thrown when an arithmetic error occurs.

  ```php
  use Psl\Math;

  try {
      Math\div(Math\INT64_MIN, -1);
  } catch (Math\Exception\ArithmeticException $e) {
      // ... handle exception
  }
  ```
</div>
