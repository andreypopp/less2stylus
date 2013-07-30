# LESS to Stylus converter

Convert LESS to Stylus by parsing LESS sources and traversing resulting AST.

## Installation

    % npm install -g less2stylus

## Usage

    % less2stylus styles.less > styles.styl

## Bootstrap translation

Currently there's a single [pull request][1] for Bootstrap to be accepted for
`less2stylus` to be capable of translating it as-is. This pull request fixes out
of order variable and mixin declarations in a few places.

[1]: https://github.com/twbs/bootstrap/pull/8726

## Notes

  * every mixin with no params or all params having default values will have
    corresponding class generated, so

        .some-mixin() {
          ...
        }

    results in

        some-mixin()
          ...

        .some-mixin
          some-mixin()

  * call to a mixin with no params will result in an `@extend` of corresponding
    class, so

        body {
          .some-mixin()
        }

    results in

        body
          @extend .some-mixin

  * `@media` directives which use variables to specify a condition on which
    rules apply will translate into an additional variable declaration which
    holds `@media` condition. This is because of Stylus limitation not to allow
    variables inside `@media` conditions.

        @media screen and (min-width: tablet) {
          ...
        }

    results in

        var213123edf25df1a = "screen and (min-width: " + tablet + ")"
        @media var213123edf25df1a
          ...

  * if there are mixins which named `translate`, `translate3d`, `scale`, `skew`,
    `rotate` then they will be prefixed with `mixin-` in resulted Stylus
    sources. This is to prevent recursive mixin invokations.
