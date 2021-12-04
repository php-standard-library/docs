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

* `Json\encode(mixed $value, bool $pretty = false, int $flags = 0): string`

  Returns a string containing the JSON representation of the supplied value.

  * `$value`: The value to be encoded.
  * `$pretty`: If true, the result will be formatted with whitespace.
  * `$flags`: Bitmask of `JSON_*` constants.

  If the value cannot be encoded, `Json\Exception\EncodeException` is thrown.

  > This function is pure.

  !> `JSON_UNESCAPED_UNICODE`, `JSON_UNESCAPED_SLASHES`, `JSON_PRESERVE_ZERO_FRACTION` flags are always set.

  ```php
  use Psl\Json;

  $json = Json\encode(['foo' => 'bar']);

  // {"foo":"bar"}

  $json = Json\encode(['foo' => 'bar'], true);

  // {
  //   "foo": "bar"
  // }
  ```

* `Json\decode(string $json, bool $assoc = true): mixed`

  Decode a json encoded string into a dynamic variable.

  * `$json`: The json encoded string.
  * `$assoc`: If true ( default ), the result will be an associative array.

  If the json string cannot be decoded, `Json\Exception\DecodeException` is thrown.

  > This function is pure.

  !> The maximum nesting depth of the structure being decoded is `512`. <br />
  This function always decodes large integers as their original string value.

  ```php
  use Psl\Json;

  $json = '{"foo":"bar"}';

  $data = Json\decode($json);
  // ['foo' => 'bar']

  $data = Json\decode($json, false);
  // stdClass { foo: 'bar' }
  ```

* `Json\typed<T>(string $json, Type\TypeInterface<T> $type): T`

  Decode a json encoded string into a typed variable.

  * `$json`: The json encoded string.
  * `$type`: The type to decode the json into.

  If the json string cannot be decoded, `Json\Exception\DecodeException` is thrown.

  > This function is pure.

  !> The maximum nesting depth of the structure being decoded is `512`. <br />
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

### Exception

* `Json\Exception\EncodeException`

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

* `Json\Exception\DecodeException`

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
    Json\typed('{'foo': 'bar'}', Type\string());
  } catch (Json\Exception\DecodeException $e) {
    // ... do something.
  }
  ```
