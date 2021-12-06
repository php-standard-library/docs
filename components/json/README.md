# Json

## Introduction

The Json component provides a set of functions to encode, and decode JSON safely.

## Usage

```php
use Psl\Json;
use Psl\Type;
use Psl\IO;

$input = IO\input_handle()->read();

$user = Json\typed($input, Type\shape([
  'username' => Type\string(),
  'password' => Type\string(),
]));

$username = $user['username'];
$password = $user['password'];
```

## API

### Functions

<div class="api-functions">

* [`Json\encode(mixed $value, bool $pretty = false, int $flags = 0): string` php]

  Returns a string containing the JSON representation of the supplied value.

  * [`$value` php]: The value to be encoded.
  * [`$pretty` php]: If true, the result will be formatted with whitespace.
  * [`$flags` php]: Bitmask of [`JSON_*` php] constants.

  If the value cannot be encoded, [`Json\Exception\EncodeException` php] is thrown.

  > This function is pure.

  !> [`JSON_UNESCAPED_UNICODE` php], [`JSON_UNESCAPED_SLASHES` php], [`JSON_PRESERVE_ZERO_FRACTION` php] flags are always set.

  ```php
  use Psl\Json;

  $json = Json\encode(['foo' => 'bar']);

  // {"foo":"bar"}

  $json = Json\encode(['foo' => 'bar'], true);

  // {
  //   "foo": "bar"
  // }
  ```

* [`Json\decode(string $json, bool $assoc = true): mixed` php]

  Decode a json encoded string into a dynamic variable.

  * [`$json` php]: The json encoded string.
  * [`$assoc` php]: If true ( default ), the result will be an associative array.

  If the json string cannot be decoded, [`Json\Exception\DecodeException` php] is thrown.

  > This function is pure.

  !> The maximum nesting depth of the structure being decoded is [`512` php]. <br />
  This function always decodes large integers as their original string value.

  ```php
  use Psl\Json;

  $json = '{"foo":"bar"}';

  $data = Json\decode($json);
  // ['foo' => 'bar']

  $data = Json\decode($json, false);
  // stdClass { foo: 'bar' }
  ```

* [`@template T` php] <br/>
  [`Json\typed(string $json, Type\TypeInterface<T> $type): T` php]

  Decode a json encoded string into a typed variable.

  * [`$json` php]: The json encoded string.
  * [`$type` php]: The type to decode the json into.

  If the json string cannot be decoded, [`Json\Exception\DecodeException` php] is thrown.

  > This function is pure.

  !> The maximum nesting depth of the structure being decoded is [`512` php]. <br />
  This function always decodes large integers as their original string value.

  ```php
  use Psl\Json;
  use Psl\Type;

  $json = '{"foo":"bar"}';

  $data = Json\typed($json, Type\shape([
    'foo' => Type\string(),
  ]));

  // ['foo' => 'bar']
  ```

</div>

### Exception

<div class="api-exceptions">

* [`final class Json\Exception\EncodeException implements Json\Exception\ExceptionInterface extends Exception` php]

  Thrown when the value cannot be encoded.

  ```php
  use Psl\Json;
  use Psl\IO;

  $handle = IO\input_handle();

  try {
    Json\encode($handle);
  } catch (Json\Exception\EncodeException $e) {
    // ... do something.
  }
  ```

* [`final class Json\Exception\DecodeException implements Json\Exception\ExceptionInterface extends Exception` php]

  Thrown when the json string cannot be decoded.

  ```php
  use Psl\Json;

  try {
    Json\decode('{{{{{');
  } catch (Json\Exception\DecodeException $e) {
    // ... do something.
  }
  ```

  This exception is also thrown when using `Json\typed` and the resulted value is not of the expected type.

  ```php
  use Psl\Json;
  use Psl\Type;

  try {
    Json\typed("{'foo': 'bar'}", Type\string());
  } catch (Json\Exception\DecodeException $e) {
    // ... do something.
  }
  ```

</div>
