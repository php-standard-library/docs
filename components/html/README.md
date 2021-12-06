# Html

## Introduction

The html component provides a set functions to encode and decode html strings.

## Usage

```php
use Psl\Html;
use Psl\IO;

$name = 'Héllo </>';

IO\write_line(Html\encode($name)); // H&eacute;llo &lt;/&gt;
IO\write_line(Html\encode_special_characters($name)); // Hello &lt;/&gt;
```

## API

### Functions

<div class="api-functions">

* [`Html\encode(string $html, bool $double_encoding = true, Html\Encoding $encoding = Html\Encoding::UTF_8): string` php]

  Convert all applicable characters to HTML entities.

  * [`$double_encoding` php]: If set to false, this function will not encode existing html entities.
  * [`$encoding` php]: defines character set used in conversion.

  ```php
  use Psl;
  use Psl\Html;

  Html\encode('Héllo </>'); // H&eacute;llo &lt;/&gt;
  Html\encode('Héllo &lt;/&gt;', true); // H&eacute;llo &amp;lt;/&amp;gt;
  Html\encode('Héllo &lt;/&gt;', false); // H&eacute;llo &lt;/&gt;
  ```

* [`Html\decode(string $html, Html\Encoding $encoding = Html\Encoding::UTF_8): string` php]

  Convert all HTML entities to their applicable characters.

  * [`$encoding` php]: defines character set used in conversion.

  ```php
  use Psl;
  use Psl\Html;

  Html\decode('H&eacute;llo &lt;/&gt;'); // Héllo </>
  Html\decode('H&eacute;llo &amp;lt;/&amp;gt;'); // Héllo &lt;/&gt;
  ```

* [`Html\encode_special_characters(string $html, bool $double_encoding = true, Html\Encoding $encoding = Html\Encoding::UTF_8): string` php]

  Convert special characters to HTML entities.

  * [`$double_encoding` php]: If set to false, this function will not encode existing html entities.
  * [`$encoding` php]: defines character set used in conversion.

  ```php
  use Psl;
  use Psl\Html;

  Html\encode_special_characters('Héllo </>'); // Héllo &lt;/&gt;
  Html\encode_special_characters('Héllo &lt;/&gt;', true); // Héllo &amp;lt;/&amp;gt;
  Html\encode_special_characters('Héllo &lt;/&gt;', false); // Héllo &lt;/&gt;
  ```

* [`Html\decode_special_characters(string $html): string` php]

  Convert special characters to HTML entities.

  * [`$html` php]: The string to decode.

  ```php
  use Psl;
  use Psl\Html;

  Html\decode_special_characters('Héllo &lt;/&gt;'); // Héllo </>
  Html\decode_special_characters('Héllo &amp;lt;/&amp;gt;'); // Héllo &lt;/&gt;
  Html\decode_special_characters('H&eacute;llo &lt;/&gt;'); // H&eacute;llo </>
  ```

* [`Html\strip_tags(string $html, list<string> $allowed_tags = []): string` php]

  Strip HTML tags from a string.

  * [`$html` php]: The string to strip.
  * [`$allowed_tags` php]: A list of tags to allow.

  ```php
  use Psl;
  use Psl\Html;

  Html\strip_tags('<p>Hello</p>'); // Hello
  Html\strip_tags('<p>Hello</p>', ['p']); // <p>Hello</p>
  Html\strip_tags('<p>Hello</p>', ['p', 'div']); // <p>Hello</p>
  Html\strip_tags('<p>Hello</p>', ['div']); // Hello
  ```

</div>

### Enums

<div class="api-enums">

  * [`enum Html\Encoding` php]

    defines character set used in conversion.

</div>