# `stylint-config-groupon`

A `stylint` config for Groupon's way of writing stylesheets
using [`stylus`][stylus].

[stylus]: http://learnboost.github.io/stylus/

## Philosophy

Whenever possible favor a simple solution over a smart one.
The harder it is to work with the framework,
the more likely people will be to work around it instead.
The grosser the hack,
the more difficult it will be to roll back or standardize.
Consider specificity a smell.

### Browser Support

IE 9 is the limbo stick to clear but [be pragmatic][pragmatic].
Rounded corners are not worth a 40kb polyfill.

[pragmatic]: http://dowebsitesneedtolookexactlythesameineverybrowser.com/

## Styleguide

### Beyond Formatting

* Write your styles to have as low a specificity as possible/reasonable.
  Nobody should have to struggle to come up with a chain of selectors that will override yours.
* Don't write your styles to be dependent on markup structures.
  Use descriptive class names liberally to apply styles to elements directly
  rather than generic class names whose styles changes based on their parent markup.
* If a variable exists for something, use it!
  Hardcoding hex values or rgb/rgba/hsl/hsla declarations into your files
  when there's already a variable for them only causes headaches down the road.
* Prefer [`nib`'s][nib] mixins & browser prefixing to homegrown solutions.

[nib]: https://github.com/tj/nib

### Formatting

#### Spacing & Blocks

Use 2 spaces for indentation, 1 space after operators, and empty lines between rules.
Omit the curly braces around blocks and semicolons.
Keep the colons after properties.
Use `//` for comments.

```styl
// Bad
.container color: $body-background-color
a
  color $color-link
  //A comment
  text-decoration: none
  &:hover {
    text-decoration: underline
  }

// Good
.container
  color: $body-background-color

a
  color: $color-link
  // A comment
  text-decoration: none

  &:hover
    text-decoration: underline
```

#### Naming Conventions

Use hyphens & lowercase to identify elements.
No underscores or camelcase.

#### `prefixVarsWithDollar`

To make it easier to distinguish variables from values,
prefix them with a dollar sign.

```styl
// Bad
hide = transparent

.thing
  background-color: hide

// Good
$hide = transparent

.thing
  background-color: $hide
```

#### Units

Omit units for zero-values.
Avoid confusing shorthand properties when order is significant.

```styl
// Bad
.container
  margin: 10px 0px
  // Quick, which dimension is repeated?
  padding: 10px 5px 15px

// Good
.container
  margin: 10px 0
  border-radius: 10px 5px 15px 5px
```

#### `quotePref`

Because it's what we do for JavaScript,
we prefer single quotes for stylus as well.

```styl
// Bad
.container
  content: "x"

// Good
.container
  content: 'x'
```

#### `duplicates`/`globalDupe`

It's a good practice to only define a selector/property combination once per project
and to use `@import` if it's needed in different files.

This rule may be broken to provide fallbacks:

```styl
// Bad
.some-class
  width: 920px
  width: 100%

// Good
.some-class
  width: 95% // old browser that dont know the calc property (I know this is a guess)
  width: calc(100% - 10px) // modern browsers
```

#### `noImportant`

Avoid `!important` at all costs.
See: [How style precendence works][specificity].

[specificity]: https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity

#### `@css`

Some CSS features like certain media queries aren't handled well by stylus.
In those cases CSS can be inlined via `@css` blocks or put into separate `.css` files.
You should use `@import` and external CSS files.

The reason for this rule isn't great - stylint just can't handle inline `@css` blocks properly.
The good news is that you should rarely need to drop down to raw CSS.
