# Runtime

## Introduction

The Runtime component provides a set of functions that can be used to retrieve information about the current PHP runtime.

## Usage

```php
use Psl\Runtime;

if (Runtime\has_extension('parallel')) {
  // ... do something
}
```

## API

### Functions

* `Runtime\get_version(): non-empty-string`

  Return the current PHP version as a string in "major.minor.release[extra]" notation.

  > This function is pure.

  ```php
  use Psl\Runtime;

  $version = Runtime\get_version();
  ```

* `Runtime\get_version_id(): positive-int`

  Return the current PHP version as an integer, useful for version comparisons (e.g., int(80100) from version "8.1.0-RC5").

  > This function is pure.

  ```php
  use Psl\Runtime;

  $version_id = Runtime\get_version_id();
  if ($version_id < 80100) {
    // ... do something
  }

  if ($version_id >= 80100) {
    // ... do something
  }
  ```

* `Runtime\get_version_details(): array{major: non-empty-string, minor: non-empty-string, release: non-empty-string, extra: non-empty-string|null}`

  Returns an array with the version details of the PHP runtime.

  > This function is pure.

  ```php
  use Psl\Runtime;

  $version_details = Runtime\get_version_details();

  $major = $version_details['major'];
  $minor = $version_details['minor'];
  $release = $version_details['release'];
  $extra = $version_details['extra'];
  ```

* `Runtime\get_zend_version(): non-empty-string`

  Return the version of the current Zend engine.

  > This function is pure.

  ```php
  use Psl\Runtime;

  $zend_version = Runtime\get_zend_version();
  ```

* `Runtime\has_extension(string $extension): bool`

  Find out whether an $extension is loaded.

  * `$extension`: The name of the extension to check.

  > This function does not cause any mutations.

  ```php
  use Psl\Runtime;

  if (Runtime\has_extension('uv')) {
    $driver = new UvDriver();
  } else {
    $driver = new StreamSelectDriver();
  }
  ```

* `Runtime\get_extensions(): non-empty-list<non-empty-string>`

  Returns an list with the names of all extensions compiled and loaded.

  > This function does not cause any mutations.

  ```php
  use Psl\Runtime;

  $extensions = Runtime\get_extensions();
  ```

* `Runtime\get_zend_extensions(): list<non-empty-string>`

  Returns an list with the names of all Zend extensions compiled and loaded.

  > This function does not cause any mutations.

  ```php
  use Psl\Runtime;

  $zend_extensions = Runtime\get_zend_extensions();
  ```

* `Runtime\get_sapi(): non-empty-string`

  Returns the name of the SAPI that PHP is running under.

  > This function is pure.

  ```php
  use Psl\Runtime;

  $sapi = Runtime\get_sapi();
  ```

* `Runtime\is_debug(): bool`

  Return true if PHP was built with debugging enabled.

  > This function is pure.

  ```php
  use Psl\Runtime;

  if (Runtime\is_debug()) {
    // ... do something
  }
  ```

* `Runtime\is_thread_safe(): bool`

  Return true if PHP was built with ZTS enabled.

  > This function is pure.

  ```php
  use Psl\Runtime;

  if (Runtime\is_thread_safe()) {
    // ... do something
  }
  ```
