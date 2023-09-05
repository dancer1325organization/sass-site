---
title: Special Functions
table_of_contents: true
introduction: >
  CSS defines many functions, and most of them work just fine with Sass’s normal
  function syntax. They’re parsed as function calls, resolved to [plain CSS
  functions](/documentation/at-rules/function/#plain-css-functions), and compiled
  as-is to CSS. There are a few exceptions, though, which have special syntax
  that can’t just be parsed as a [SassScript
  expression](/documentation/syntax/structure#expressions). All special function
  calls return [unquoted strings](/documentation/values/strings#unquoted).
---

## `url()`

The [`url()` function][] is commonly used in CSS, but its syntax is different
than other functions: it can take either a quoted *or* unquoted URL. Because an
unquoted URL isn't a valid SassScript expression, Sass needs special logic to
parse it.

[`url()` function]: https://developer.mozilla.org/en-US/docs/Web/CSS/url

If the `url()`'s argument is a valid unquoted URL, Sass parses it as-is,
although [interpolation][] may also be used to inject SassScript values. If it's
not a valid unquoted URL—for example, if it contains [variables][] or [function
calls][]—it's parsed as a normal [plain CSS function call][].

[interpolation]: /documentation/interpolation
[variables]: /documentation/variables
[function calls]: /documentation/at-rules/function
[plain CSS function call]: /documentation/at-rules/function/#plain-css-functions

{% codeExample 'url' %}
  $roboto-font-path: "../fonts/roboto";

  @font-face {
      // This is parsed as a normal function call that takes a quoted string.
      src: url("#{$roboto-font-path}/Roboto-Thin.woff2") format("woff2");

      font-family: "Roboto";
      font-weight: 100;
  }

  @font-face {
      // This is parsed as a normal function call that takes an arithmetic
      // expression.
      src: url($roboto-font-path + "/Roboto-Light.woff2") format("woff2");

      font-family: "Roboto";
      font-weight: 300;
  }

  @font-face {
      // This is parsed as an interpolated special function.
      src: url(#{$roboto-font-path}/Roboto-Regular.woff2) format("woff2");

      font-family: "Roboto";
      font-weight: 400;
  }
  ===
  $roboto-font-path: "../fonts/roboto"

  @font-face
      // This is parsed as a normal function call that takes a quoted string.
      src: url("#{$roboto-font-path}/Roboto-Thin.woff2") format("woff2")

      font-family: "Roboto"
      font-weight: 100


  @font-face
      // This is parsed as a normal function call that takes an arithmetic
      // expression.
      src: url($roboto-font-path + "/Roboto-Light.woff2") format("woff2")

      font-family: "Roboto"
      font-weight: 300


  @font-face
      // This is parsed as an interpolated special function.
      src: url(#{$roboto-font-path}/Roboto-Regular.woff2) format("woff2")

      font-family: "Roboto"
      font-weight: 400
{% endcodeExample %}

## `element()`, `progid:...()`, and `expression()`

{% compatibility 'dart: "<1.40.0"', 'libsass: false', 'ruby: false', 'feature: "calc()"' %}
  LibSass, Ruby Sass, and versions of Dart Sass prior to 1.40.0 parse `calc()`
  as special syntactic function like `element()`.

  Dart Sass versions 1.40.0 and later parse `calc()` as a [calculation].

  [calculation]: /documentation/values/calculations
{% endcompatibility %}

{% compatibility 'dart: ">=1.31.0 <1.40.0"', 'libsass: false', 'ruby: false', 'feature: "clamp()"' %}
  LibSass, Ruby Sass, and versions of Dart Sass prior to 1.31.0 parse `clamp()`
  as a [plain CSS function] rather than supporting special syntax within it.

  [plain CSS function]: /documentation/at-rules/function/#plain-css-functions

  Dart Sass versions between 1.31.0 and 1.40.0 parse `clamp()` as special
  syntactic function like `element()`.

  Dart Sass versions 1.40.0 and later parse `clamp()` as a [calculation].

  [calculation]: /documentation/values/calculations
{% endcompatibility %}

The [`element()`] function is defined in the CSS spec, and because its IDs could
be parsed as colors, they need special parsing.

[`element()`]: https://developer.mozilla.org/en-US/docs/Web/CSS/element

[`expression()`][] and functions beginning with [`progid:`][] are legacy
Internet Explorer features that use non-standard syntax. Although they're no
longer supported by recent browsers, Sass continues to parse them for backwards
compatibility.

[`expression()`]:
    https://blogs.msdn.microsoft.com/ie/2008/10/16/ending-expressions/
[`progid:`]:
    https://blogs.msdn.microsoft.com/ie/2009/02/19/the-css-corner-using-filters-in-ie8/

Sass allows *any text* in these function calls, including nested parentheses.
Nothing is interpreted as a SassScript expression, with the exception that
[interpolation][] can be used to inject dynamic values.

[interpolation]: /documentation/interpolation

{% codeExample 'element' %}
  $logo-element: logo-bg;

  .logo {
    background: element(##{$logo-element});
  }
  ===
  $logo-element: logo-bg

  .logo
    background: element(#{$logo-element})
{% endcodeExample %}
