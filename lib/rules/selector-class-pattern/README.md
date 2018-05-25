# selector-class-pattern

Specify a pattern for class selectors.

```css
    .foo, #bar.baz span, #hoo[disabled] { color: pink; }
/** ↑         ↑
 * These class selectors */
```

This rule ignores non-outputting Less mixin definitions and called Less mixins.

Escaped selectors (e.g. `.u-size-11\/12\@sm`) are parsed as escaped twice (e.g. `.u-size-11\\/12\\@sm`). Your RegExp should account for that.

## Options

`regex|string`

A string will be translated into a RegExp — `new RegExp(yourString)` — so *be sure to escape properly*.

The selector value *after `.`* will be checked. No need to include `.` in your pattern.

Given the string:

```js
"foo-[a-z]+"
```

The following patterns are considered violations:

```css
.foop {}
```

```css
.foo-BAR {}
```

```css
div > #zing + .foo-BAR {}
```

The following patterns are *not* considered violations:

```css
.foo-bar {}
```

```css
div > #zing + .foo-bar {}
```

```css
#foop {}
```

```css
[foo='bar'] {}
```

```less
.foop() {}
```

```less
.foo-bar {
  .foop;
}
```

## Optional secondary options

### `messageTemplate: String` (default: null)

This option will change report message.

For example, with `Expected class selector ".${value}" to match BEM naming. http://getbem.com/naming/`.

Given the regex:

```js
/^(?:(?:(?:^|(?!^)-)[a-z]+\d*|-[a-z]*\d+)(?:__[a-z]+\d*|__[a-z]*\d+){0,1}(?:--[a-z]+\d*|--[a-z]*\d+){0,1})*$/
```

The following patterns are considered violations:

```css
.not_legal_bem {}
```

And report will be `Expected class selector ".not_legal_bem" to match BEM naming. http://getbem.com/naming/`.

### `resolveNestedSelectors: true | false` (default: `false`)

This option will resolve nested selectors with `&` interpolation.

For example, with `true`.

Given the string:

```js
"^[A-Z]+$"
```

The following patterns are considered violations:

```css
.A {
  &__B {} /* resolved to ".A__B" */
}
```

The following patterns are *not* considered violations:

```css
.A {
  &B {} /* resolved to ".AB" */
}
```
