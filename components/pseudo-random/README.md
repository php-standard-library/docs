# PseudoRandom

## Introduction

The PseudoRandom component is a component that generates pseudo-random numbers.

?> The PseudoRandom component is meant as a direct replacement for PHP [`mt_rand()`](https://www.php.net/manual/en/function.mt-rand.php), [`rand()`](https://www.php.net/manual/en/function.rand.php) functions.

## Usage

```php
use Psl\PseudoRandom;

$random_number = PseudoRandom\int();
$random_float = PseudoRandom\float();
```

## API

### Functions

<div class="api-functions">

* [`@external-mutation-free` php] <br/>
  [`PseudoRandom\int(int $min = Math\INT64_MIN, int $max = Math\INT64_MAX): int` php]

  Returns a pseudo-random integer in the given range.

  * [`$min` php]: The minimum value.
  * [`$max` php]: The maximum value.

  If [`$min` php] is greater than [`$max` php], [`Psl\Exception\InvariantViolationException` php] is thrown.

  ```php
  use Psl\PseudoRandom;

  $random_number = PseudoRandom\int(0, 10);

  // 0 <= $random_number <= 10
  ```

* [`@external-mutation-free` php] <br/>
  [`PseudoRandom\float(): float` php]

  Returns a pseudo-random float in the range of [0.0, 1.0].

  ```php
  use Psl\PseudoRandom;

  $random_float = PseudoRandom\float();

  // 0.0 <= $random_float <= 1.0
  ```

</div>
