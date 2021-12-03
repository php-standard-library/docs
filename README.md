# PHP Standard Library

PHP Standard Library ( a.k.a. Psl ), is a standard library for PHP, inspired by [hhvm/hsl](https://github.com/hhvm/hsl).

The goal of Psl is to provide a consistent, centralized, well-typed set of APIs for PHP programmers.

## Installation

Supported installation method is via [`composer`](https://getcomposer.org/):

```bash
composer require azjezz/psl:^2.0
```

### Psalm Integration ( recommended )

Psl provides an official Psalm plugin that provides further type enhancements:

```bash
composer require php-standard-library/psalm-plugin --dev && \
    php vendor/bin/psalm-plugin enable php-standard-library/psalm-plugin
```
