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

<div class="api-functions">

* [`@pure` php] <br />
  [`Runtime\get_version(): non-empty-string` php]

  Return the current PHP version as a string in "major.minor.release[extra]" notation.

  ```php
  use Psl\Runtime;

  $version = Runtime\get_version();
  ```

* [`@pure` php] <br />
  [`Runtime\get_version_id(): positive-int` php]

  Return the current PHP version as an integer, useful for version comparisons (e.g., int(80100) from version "8.1.0-RC5").

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

* [`@pure` php] <br />
  [`Runtime\get_version_details(): array{major: non-empty-string, minor: non-empty-string, release: non-empty-string, extra: non-empty-string|null}` php]

  Returns an array with the version details of the PHP runtime.

  ```php
  use Psl\Runtime;

  $version_details = Runtime\get_version_details();

  $major = $version_details['major'];
  $minor = $version_details['minor'];
  $release = $version_details['release'];
  $extra = $version_details['extra'];
  ```

* [`@pure` php] <br />
  [`Runtime\get_zend_version(): non-empty-string` php]

  Return the version of the current Zend engine.

  ```php
  use Psl\Runtime;

  $zend_version = Runtime\get_zend_version();
  ```

* [`@mutation-free` php] <br/>
  [`Runtime\has_extension(string $extension): bool` php]

  Find out whether an $extension is loaded.

  * `$extension`: The name of the extension to check.

  ```php
  use Psl\Runtime;

  if (Runtime\has_extension('uv')) {
    $driver = new UvDriver();
  } else {
    $driver = new StreamSelectDriver();
  }
  ```

* [`@mutation-free` php] <br/>
  [`Runtime\get_extensions(): non-empty-list<non-empty-string>` php]

  Returns an list with the names of all extensions compiled and loaded.

  ```php
  use Psl\Runtime;

  $extensions = Runtime\get_extensions();
  ```

* [`@mutation-free` php] <br/>
  [`Runtime\get_zend_extensions(): list<non-empty-string>` php]

  Returns an list with the names of all Zend extensions compiled and loaded.

  ```php
  use Psl\Runtime;

  $zend_extensions = Runtime\get_zend_extensions();
  ```

* [`@pure` php] <br />
  [`Runtime\get_sapi(): non-empty-string` php]

  Returns the name of the SAPI that PHP is running under.

  ```php
  use Psl\Runtime;

  $sapi = Runtime\get_sapi();
  ```

* [`@pure` php] <br />
  [`Runtime\is_debug(): bool` php]

  Return true if PHP was built with debugging enabled.

  ```php
  use Psl\Runtime;

  if (Runtime\is_debug()) {
    // ... do something
  }
  ```

* [`@pure` php] <br />
  [`Runtime\is_thread_safe(): bool` php]

  Return true if PHP was built with ZTS enabled.

  ```php
  use Psl\Runtime;

  if (Runtime\is_thread_safe()) {
    // ... do something
  }
  ```

</div>