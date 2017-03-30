# selector-max-specificity

Limit the specificity of selectors.

```css
    .foo, #bar.baz span, #hoo { color: pink; }
/** ↑     ↑              ↑
 * Each of these selectors */
```

Visit the [Specificity Calculator](https://specificity.keegan.st) for visual representation of selector specificity.

This rule ignores selectors with variable interpolation (`#{$var}`, `@{var}`, `$(var)`).

This rule ignores selectors containing the `:not()` or `:matches()` pseudo-classes.

This rule resolves nested selectors before calculating the specificity of a selector.

## Options

`string`: Maximum specificity allowed.

Format is `"id,class,type"`, as laid out in the [W3C selector spec](https://drafts.csswg.org/selectors/#specificity-rules).

For example, with `"0,2,0"`:

The following patterns are considered warnings:

```css
#foo {}
```

```css
.foo .baz .bar {}
```

```css
.foo .baz {
  & .bar {}
}
```

```css
.foo {
  color: red;
  @nest .baz .bar & {
    color: blue;
  }
}
```

The following patterns are *not* considered warnings:

```css
div {}
```

```css
.foo div {}
```

```css
.foo div {
  & div a {}
}
```

```css
.foo {
  & .baz {}
}
```

```css
.foo {
  color: red;
  @nest .baz & {
    color: blue;
  }
}
```

## Optional secondary options

### `granular: true | false` (default: `false`)

Limit each component of specificity (`id`, `class`, and `type`) individually, rather than limiting cumulative specificity.

For example, with `"1,2,1"`:

The following patterns are always considered warnings:

```css
a span {}
```

```css
.foo .bar .baz {}
```

```css
.foo .bar a:hover {}
```

```css
#hello #world .foo {}
```

The following patterns are *not* considered warnings:

```css
span {}
```

```css
.foo .bar::after {}
```

```css
.bar a:hover {}
```

```css
#hello .foo {}
```
