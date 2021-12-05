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

* `Math\INFINITY`

  The value of `INFINITY` is `1 / 0` (positive infinity).

* `Math\NAN`

  The value of `NAN` is `0 / 0` (not a number).

* `Math\E`

  The base of the natural system of logarithms, or approximately 2.7182818284590452353602875.

* `Math\PI`

  The ratio of the circumference of a circle to its diameter, approximately 3.141592653589793238462643.

* `Math\INT64_MAX`

  The maximum integer value representable in a 64-bit binary-coded decimal.

* `Math\INT64_MIN`

  The minimum integer value representable in a 64-bit binary-coded decimal.

* `Math\INT32_MAX`

  The maximum integer value representable in a 32-bit binary-coded decimal.

* `Math\INT32_MIN`

  The minimum integer value representable in a 32-bit binary-coded decimal.

* `Math\INT16_MAX`

  The maximum integer value representable in a 16-bit binary-coded decimal.

* `Math\INT16_MIN`

  The minimum integer value representable in a 16-bit binary-coded decimal.

* `Math\UINT32_MAX`

  The maximum unsigned integer value representable in a 32-bit binary-coded decimal.

* `Math\UINT16_MAX`

  The maximum unsigned integer value representable in a 16-bit binary-coded decimal.

### Functions

* `Math\sin(float $number): float`

  Returns the sine of the given number.

  * `$number`: The number to calculate the sine of.

  > This function is pure.

  ```php
  use Psl\Math;

  $a = Math\sin(5.0); // -0.9589242746631385
  ```

* `Math\cos(float $number): float`

  Returns the cosine of the given number.

  * `$number`: The number to calculate the cosine of.

  > This function is pure.

  ```php
  use Psl\Math;

  $a = Math\cos(1.0); // 0.5403023058681398
  ```

* `Math\tan(float $number): float`

  Returns the tangent of the given number.

  * `$number`: The number to calculate the tangent of.

  > This function is pure.

  ```php
  use Psl\Math;

  $a = Math\tan(4.8); // -11.384870654242922
  ```

* `Math\asin(float $number): float`

  Returns the arc sine of the given number.

  * `$number`: The number to calculate the arc sine of.

  > This function is pure.

  ```php
  use Psl\Math;

  $a = Math\asin(0.5); // 0.5235987755982989
  ```

* `Math\acos(float $number): float`

  Returns the arc cosine of the given number.

  * `$number`: The number to calculate the arc cosine of.

  > This function is pure.

  ```php
  use Psl\Math;

  $a = Math\acos(0.5); // 1.0471975511965979
  ```

* `Math\atan(float $number): float`

  Returns the arc tangent of the given number.

  * `$number`: The number to calculate the arc tangent of.

  > This function is pure.

  ```php
  use Psl\Math;

  $a = Math\atan(-0.5); // -0.4636476090008061
  ```

* `Math\atan2(float $y, float $x): float`

  Returns the arc tangent of the given coordinates.

  * `$y`: The y-coordinate of the point.

  * `$x`: The x-coordinate of the point.

  > This function is pure.

  ```php
  use Psl\Math;

  $a = Math\atan2(1.0, 0.8); // 0.8960553845713439
  ```

* `Math\abs<T of int|float>(T $number): T`

  Returns the absolute value of the given number.

  * `$number`: The number to calculate the absolute value of.

  > This function is pure.

  ```php
  use Psl\Math;

  $a = Math\abs(2);     // 2
  $b = Math\abs(-2);    // 2
  $c = Math\abs(2.5);   // 2.5
  $d = Math\abs(-2.5);  // 2.5
  ```

* `Math\ceil(float $number): float`

  Returns the smallest integer greater than or equal to the given number.

  * `$number`: The number to calculate the ceiling of.

  > This function is pure.

  ```php
  use Psl\Math;

  $a = Math\ceil(2);      // 2.0
  $b = Math\ceil(-2);     // -2.0
  $c = Math\ceil(2.5);    // 3.0
  $d = Math\ceil(-2.5);   // -2.0
  ```

* `Math\floor(float $number): float`

  Returns the largest integer less than or equal to the given number.

  * `$number`: The number to calculate the floor of.

  > This function is pure.

  ```php
  use Psl\Math;

  $a = Math\floor(2);     // 2.0
  $b = Math\floor(-2);    // -2.0
  $c = Math\floor(2.5);   // 2.0
  $d = Math\floor(-2.5);  // -3.0
  ```

* `Math\round(float $number, int $precision = 0): float`

  Returns the given number rounded to the specified precision.

  A positive precision rounds to the nearest decimal place whereas a negative precision rounds to the nearest power of ten.

  For example, a precision of 1 rounds to the nearest tenth whereas a precision of -1 rounds to the nearst nearest.

  * `$number`: The number to round.

  > This function is pure.

  ```php
  use Psl\Math;

  $a = Math\round(2.5);       // 3.0
  $b = Math\round(-2.5);      // -2.0

  $c = Math\round(2.5, 1);    // 2.5
  $d = Math\round(-2.5, 1);   // -2.5

  $e = Math\round(2.5, -1);   // 3.0
  $f = Math\round(-2.5, -1);  // -3.0
  ```

* `Math\sqrt(float $number): float`

  Returns the square root of the given number.

  * `$number`: The number to calculate the square root of.

  If the given number is negative, `Psl\Exception\InvariantViolationException` is thrown.

  > This function is pure.

  ```php
  use Psl\Math;

  $a = Math\sqrt(2); // 1.4142135623730951
  ```

* `Math\log(float $number, ?float $base = null): float`

  Returns the logarithm of the given number.

  * `$number`: The number to calculate the logarithm of.

  If the given number is equal to, or less than, zero, `Psl\Exception\InvariantViolationException` is thrown. <br />
  If the given base is equal to, or less than, zero, `Psl\Exception\InvariantViolationException` is thrown. <br />
  If the given base is equal to one, `Psl\Exception\InvariantViolationException` is thrown.

  > This function is pure.

  ```php
  use Psl\Math;

  $a = Math\log(2); // 0.6931471805599453
  $b = Math\log(2, 2); // 1.0
  ```

* `Math\exp(float $number): float`

  Returns the exponential of the given number.

  * `$number`: The number to calculate the exponential of.

  > This function is pure.

  ```php
  use Psl\Math;

  $a = Math\exp(2); // 7.38905609893065
  ```

* `Math\div(int $numerator, int $denominator): int`

  Returns the integer division of the given numerator by the given denominator.

  * `$numerator`: The numerator.
  * `$denominator`: The denominator.

  If the given denominator is zero, `Exception\DivisionByZeroException` is thrown. <br />
  If the given numerator is equal to `Math\INT64_MIN` and the given denominator is equal to `-1`, `Exception\ArithmeticException` is thrown.

  > This function is pure.

  ```php
  use Psl\Math;

  $a = Math\div(2, 2);    // 1
  $b = Math\div(2, 3);    // 0
  $d = Math\div(3, 3);    // 1
  $d = Math\div(30, 3);   // 10
  ```

* `Math\sum(list<int> $numbers): int`

  Returns the sum of all the given numbers.

  * `$numbers`: The numbers to sum.

  > This function is pure.

  ```php
  use Psl\Math;

  $a = Math\sum([1, 2, 3]);     // 6
  $b = Math\sum([1, 2, 3, 4]);  // 10
  ```

* `Math\sum_floats(list<float|int> $numbers): float`

  Returns the sum of all the given numbers.

  * `$numbers`: The numbers to sum.

  > This function is pure.

  ```php
  use Psl\Math;

  $a = Math\sum_floats([1.1, 2.2, 3.3]);      // 6.6
  $b = Math\sum_floats([1.1, 2.2, 3.3, 4.4]); // 10.4
  ```

* `Math\clamp<T of int|float>(T $number, T $min, T $max): T`

  Returns the given number clamped to the given range.

  * `$number`: The number to clamp.
  * `$min`: The minimum value.
  * `$max`: The maximum value.

  If the given number is less than the given minimum, the minimum is returned. <br />
  If the given number is greater than the given maximum, the maximum is returned. <br />

  If `$min` is greater than `$max`, `Psl\Exception\InvariantViolationException` is thrown.

  > This function is pure.

  ```php
  use Psl\Math;

  $a = Math\clamp(2, 1, 3); // 2
  $b = Math\clamp(1, 1, 3); // 1
  $c = Math\clamp(3, 1, 3); // 3
  ```

* `Math\base_convert(non-empty-string $value, int $from_base, int $to_base): string`

  Converts the given string in base `$from_base` to base `$to_base`, assuming letters a-z are used for digits for bases greater than 10. <br />
  The conversion is done to arbitrary precision. <br />

  * `$value`: The string to convert.
  * `$from_base`: The base of the given string.
  * `$to_base`: The base to convert to.

  If `$value` is not a valid base `$from_base` number, `Psl\Exception\InvariantViolationException` is thrown. <br />
  If `$from_base` and `$to_base` are out of the [2, 36] range, `Psl\Exception\InvariantViolationException` is thrown. <br />

  > This function is pure.

  ```php
  use Psl\Math;

  $a = Math\base_convert('101', 2, 10); // '5'
  $b = Math\base_convert('5', 10, 2); // '101'

  $c = Math\base_convert('2014587925987', 10, 36); // 'pphlmw9v'
  $d = Math\base_convert('pphlmw9v', 36, 10); // '2014587925987'
  ```

* `Math\from_base(non-empty-string $number, int $from_base): int`

    Converts the given string in base `$from_base` to an integer,  assuming letters a-z are used for digits when `$from_base` > 10. <br />

    * `$number`: The string to convert.
    * `$from_base`: The base of the given string.

    If `$number` is not a valid base `$from_base` number, `Psl\Exception\InvariantViolationException` is thrown. <br />
    If `$from_base` is out of the [2, 36] range, `Psl\Exception\InvariantViolationException` is thrown. <br />

    > This function is pure.

    ```php
    use Psl\Math;

    $a = Math\from_base('101', 2); // 5
    $b = Math\from_base('5', 10); // 5
    $c = Math\from_base('pphlmw9v', 36); // 2014587925987
    ```

* `Math\to_base(0|positive-int $number, positive-int $base): non-empty-string`

    Converts the given non-negative integer to a string in base `$base`, using letters a-z for digits when `$base` > 10. <br />

    * `$number`: The integer to convert.
    * `$base`: The base to convert to.

    If `$base` is out of the [2, 36] range, `Psl\Exception\InvariantViolationException` is thrown. <br />
    If `$number` is negative, `Psl\Exception\InvariantViolationException` is thrown. <br />

    > This function is pure.

    ```php
    use Psl\Math;

    $a = Math\to_base(5, 2); // '101'
    $b = Math\to_base(5, 10); // '5'
    $c = Math\to_base(2014587925987, 36); // 'pphlmw9v'
    ```

* `Math\max<T of int|float>(list<T> $numbers): ?T`

  Returns the largest element of the given list, or null if the list is empty.

  * `$numbers`: The values to compare.

  > This function is pure.

  ```php
  use Psl\Math;

  $a = Math\max([1, 2, 3]);     // 3
  $b = Math\max([1, 2, 3, 4]);  // 4
  $c = Math\max([]);            // null
  ```

* `Math\min<T of int|float>(list<T> $numbers): ?T`

  Returns the smallest element of the given list, or null if the list is empty.

  * `$numbers`: The values to compare.

  > This function is pure.

  ```php
  use Psl\Math;

  $a = Math\min([1, 2, 3]);     // 1
  $b = Math\min([1, 2, 3, 4]);  // 1
  $c = Math\min([]);            // null
  ```

* `Math\max_by<T>(iterable<T> $numbers, (callable(T $value): numeric) $numeric_function): ?T`

  Returns the largest element of the given iterable, or null if the iterable is empty. <br />
  The value for comparison is determined by the given function. <br />
  In the case of duplicate values, later values overwrite previous ones. <br />

  * `$numbers`: The values to compare.
  * `$numeric_function`: A function that returns the numeric value used for comparison.

  ```php
  use Psl\Math;

  $a = Math\max_by([1, 2, 3], static fn (int $value): int => $value * 2);     // 6
  $b = Math\max_by([1, 2, 3, 4], static fn (int $value): int => $value * 2);  // 8
  $c = Math\max_by([]);                                                       // null
  ```

* `Math\min_by<T>(iterable<T> $numbers, (callable(T $value): numeric) $numeric_function): ?T`

  Returns the smallest element of the given iterable, or null if the iterable is empty. <br />
  The value for comparison is determined by the given function. <br />
  In the case of duplicate values, later values overwrite previous ones. <br />

  * `$numbers`: The values to compare.
  * `$numeric_function`: A function that returns the numeric value used for comparison.

  ```php
  use Psl\Math;

  $a = Math\min_by([1, 2, 3], static fn (int $value): int => $value * 2);     // 2
  $b = Math\min_by([1, 2, 3, 4], static fn (int $value): int => $value * 2);  // 2
  $c = Math\min_by([]);                                                       // null
  ```

* `Math\maxva<T of int|float>(T $first, T $second, T ...$rest): T`

  Returns the largest number of all the given numbers.

  * `$first`: The first value to compare.
  * `$second`: The second value to compare.
  * `$rest`: The rest of the values to compare.

  > This function is pure.

  ```php
  use Psl\Math;

  $a = Math\maxva(1, 2, 3);     // 3
  $b = Math\maxva(1, 2, 3, 4);  // 4
  $c = Math\maxva(1, 2);        // 2
  ```

* `Math\minva<T of int|float>(T $first, T $second, T ...$rest): T`

  Returns the smallest number of all the given numbers.

  * `$first`: The first value to compare.
  * `$second`: The second value to compare.
  * `$rest`: The rest of the values to compare.

  > This function is pure.

  ```php
  use Psl\Math;

  $a = Math\minva(1, 2, 3);     // 1
  $b = Math\minva(1, 2, 3, 4);  // 1
  $c = Math\minva(1, 2);        // 1
  ```

* `Math\mean(list<int|float> $numbers): ?float`

  Returns the arithmetic mean of the given numbers in the list. <br />
  Returns null if the given iterable is empty.

  * `$numbers`: The values to calculate the mean from.

  > This function is pure.

  ```php
  use Psl\Math;

  $a = Math\mean([1, 2, 3]);     // 2
  $b = Math\mean([20, 15, 31]);  // 22
  $c = Math\mean([]);            // null
  ```

* `Math\median(list<int|float> $numbers): ?float`

  Returns the median of the given numbers in the list. <br />
  Returns null if the given iterable is empty.

  * `$numbers`: The values to calculate the median from.

  > This function is pure.

  ```php
  use Psl\Math;

  $a = Math\median([1, 2, 3]);     // 2
  $b = Math\median([20, 15, 31]);  // 20
  $c = Math\median([]);            // null
  ```

### Exceptions

* `Math\Exception\DivisionByZeroException`

  Thrown when division by zero is attempted.

  ```php
  use Psl\Math;

  try {
      Math\div(1, 0);
  } catch (Math\Exception\DivisionByZeroException $e) {
      // ... handle exception
  }
  ```

* `Math\Exception\ArithmeticException`

  Thrown when an arithmetic error occurs.

  ```php
  use Psl\Math;

  try {
      Math\div(Math\INT64_MIN, -1);
  } catch (Math\Exception\ArithmeticException $e) {
      // ... handle exception
  }
  ```
