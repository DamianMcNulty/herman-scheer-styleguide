# Sass Styleguide

*A mostly reasonable approach to CSS and Sass*

## Table of Contents
  0. [Commandments](#commandments)
  1. [Terminology](#terminology)
    - [Rule Declaration](#rule-declaration)
    - [Selectors](#selectors)
    - [Properties](#properties)
  1. [CSS](#css)
    - [Formatting](#formatting)
    - [Comments](#comments)
    - [OOCSS and BEM](#oocss-and-bem)
    - [ID Selectors](#id-selectors)
    - [JavaScript hooks](#javascript-hooks)
  1. [Sass](#sass)
    - [Syntax](#syntax)
    - [Ordering](#ordering-of-property-declarations)
    - [Mixins](#mixins)
    - [Placeholders](#placeholders)
    - [Nested selectors](#nested-selectors)

## Commandments
1. Let’s use REMS
  - Using pixels is not responsive and leads to different sizing issues across devices.
  - if you still want to use pixels you can use rem-calc() function

2. The use of mixins is for ease of development, overwriting a mixin can lead to styling inconsistencies.
  - if you’re using grid-column(11) and override the width to be 100%, then you could just use grid-column(12) and save yourself the extra line of code.

3. Grid-row and Grid-column are for grid consistency and layout.
  - The use of grid-row and grid-column are for elements that need to be in line with the grid.
  - Overuse of grid-row and grid-column is just extra code

4. Use @extend or .class-name when re-using code.
  - When extending a component either use .p-one hello world on your jade or use @extend .p-one on you sass.

5. Let’s start our elements with the element name and then add  **-wrap for the rest of you elements.
  - Read and follow the BEM methodology (2 minute read)
  - ```.header > &__wrap > &__content```
  - We only use one level when using bem


6. Overcomplicated @mixins can confuse other developers same with @extends and @functions.
  - It might be tempting to build extremely powerful mixins with massive amounts of logic. It’s called over-engineering and most developers suffer from it. 
  - Don’t over think your code, and above all keep it simple. If a mixin ends up being longer than 20 lines or so, then it should be either split into smaller chunks or completely revised.
  - Put all these mixins in the mixins file in the utils folder.
 
7. Stick with the pre-defined media queries unless there’s a special case.
  - Foundation provides us with 4 media queries (small, medium, large, extra-large) use the +breakpoint mixin to use them. Use as many media queries as possible, but don’t overdo it.
  - You can resize your browser after large-up, which means if you have widows on your copy after large then either put a max-width or let the user resize their own browser.


8. Let’s keep the folder structure simple and consistent.
  - Creating more folder than the ones already in the assigned folder structure is not needed, if you must create another folder, there has to be a good reason for it.

9. Overcomplicated names are hard to understand.
  - A good naming convention should tell us
  - what type of thing a class does
  - where a class can be used;
  - what (else) a class might be related to.
  - The naming convention I follow is very simple: hyphen (-) delimited strings, with BEM-like naming for more complex pieces of code with the use the &__ instead of nesting.

10. Keep Sass Simple.

## Terminology
### Rule declaration

A “rule declaration” is the name given to a selector (or a group of selectors) with an accompanying group of properties. Here's an example:

```css
.listing {
  font-size: 18rem;
  line-height: 1.2;
}
```

### Selectors

In a rule declaration, “selectors” are the bits that determine which elements in the DOM tree will be styled by the defined properties. Selectors can match HTML elements, as well as an element's class, ID, or any of its attributes. Here are some examples of selectors:

```css
.my-element-class {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
```

### Properties

Finally, properties are what give the selected elements of a rule declaration their style. Properties are key-value pairs, and a rule declaration can contain one or more property declarations. Property declarations look like this:

```css
/* some selector */ {
  background: #f1f1f1;
  color: #333;
}
```

## CSS

### Formatting

* Use hard tabs for indentation
* Prefer dashes over camelCasing in class names. Underscores are OK if you're using BEM (see [OOCSS and BEM](#oocss-and-bem) below).
* Do not use ID selectors
* When using multiple selectors in a rule declaration, give each selector its own line.
* Put a space before the opening brace `{` in rule declarations
* In properties, put a space after, but not before, the `:` character.
* Put closing braces `}` of rule declarations on a new line
* Put blank lines between rule declarations

**Bad**

```css
.avatar{
    border-radius:50%;
    border:2rem solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

**Good**

```css
.avatar {
  border-radius: 50%;
  border: 2rem solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

### Comments

* Prefer line comments (`//` in Sass-land) to block comments.
* Prefer comments on their own line. Avoid end-of-line comments.
* Write detailed comments for code that isn't self-documenting:
  - Uses of z-index
  - Compatibility or browser-specific hacks

### BEM

We encourage BEM for these reasons:

  * It helps create clear, strict relationships between CSS and HTML
  * It helps us create reusable, composable components
  * It allows for less nesting and lower specificity
  * It helps in building scalable stylesheets

**BEM**, or “Block-Element-Modifier”, is a _naming convention_ for classes in HTML and CSS. It was originally developed by Yandex with large codebases and scalability in mind, and can serve as a solid set of guidelines for implementing OOCSS.

  * CSS Trick's [BEM 101](https://css-tricks.com/bem-101/)
  * Harry Roberts' [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

**Example**

```html
<article class="listing-card listing-card--featured">

  <h1 class="listing-card__title">Adorable 2BR in the sunny Mission</h1>

  <div class="listing-card__content">
    <p>Vestibulum id ligula porta felis euismod semper.</p>
  </div>

</article>
```

```sass
.listing-card
  &--featured
  &__title
  &__content
```

Compiles to: 

```css
.listing-card { }
.listing-card--featured { }
.listing-card__title { }
.listing-card__content { }
```

  * `.listing-card` is the “block” and represents the higher-level component
  * `.listing-card__title` is an “element” and represents a descendant of `.listing-card` that helps compose the block as a whole.
  * `.listing-card--featured` is a “modifier” and represents a different state or variation on the `.listing-card` block.

### ID selectors

While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable.

For more on this subject, read [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) on dealing with specificity.

### JavaScript hooks

Avoid binding to the same class in both your CSS and JavaScript. Conflating the two often leads to, at a minimum, time wasted during refactoring when a developer must cross-reference each class they are changing, and at its worst, developers being afraid to make changes for fear of breaking functionality.

We recommend creating JavaScript-specific classes to bind to, prefixed with `.js-`:

```html
<button class="btn btn-primary js-request-to-book">Request to Book</button>
```

Now most of the time we would be using angular directives for any dom manipulation but in case it's a small project

## Sass

### Syntax

* Use the `.sass` syntax
* Order your `@extend`, regular CSS and `@include` declarations logically (see below)

### Ordering of property declarations

1. `@extend` declarations

    Just as in other OOP languages, it's helpful to know right away that this “class” inherits from another.

    ```sass
    .btn-green
      @extend %btn
      // ...
    ```

2. Property declarations

    Now list all standard property declarations, anything that isn't an `@extend`, `@include`, or a nested selector.

    ```sass
    .btn-green
      @extend %btn
      background: green
      font-weight: bold
      // ...
    ```

3. `@include` declarations

    Grouping `@include`s at the end makes it easier to read the entire selector, and it also visually separates them from `@extend`s.

    ```sass
    .btn-green
      @extend %btn
      background: green
      font-weight: bold
      @include transition(background 0.5s ease)
      // ...
    
    ```

4. Nested selectors

    Nested selectors, _if necessary_, go last, and nothing goes after them. Add whitespace between your rule declarations and nested selectors, as well as between adjacent nested selectors. Apply the same guidelines as above to your nested selectors.

    ```sass
    .btn
      @extend %btn
      background: green
      font-weight: bold
      @include transition(background 0.5s ease)

      .icon
        margin-right: 10rem

    ```

### Mixins

Mixins, defined via `@mixin` and called with `@include`, should be used sparingly and only when function arguments are necessary. A mixin without function arguments (i.e. `@mixin hide display: none`) is better accomplished using a placeholder selector (see below) in order to prevent code duplication.

### Placeholders

Placeholders in Sass, defined via `%selector` and used with `@extend`, are a way of defining rule declarations that aren't automatically output in your compiled stylesheet. Instead, other selectors “inherit” from the placeholder, and the relevant selectors are copied to the point in the stylesheet where the placeholder is defined. This is best illustrated with the example below.

Placeholders are powerful but easy to abuse, especially when combined with nested selectors. **As a rule of thumb, avoid creating placeholders with nested rule declarations, or calling `@extend` inside nested selectors.** Placeholders are great for simple inheritance, but can easily result in the accidental creation of additional selectors without paying close attention to how and where they are used.

**Sass**

```sass
// Unless we call `@extend %icon` these properties won't be compiled!
%icon
  font-family: "Airglyphs"


.icon-error
  @extend %icon
  color: red


.icon-success
  @extend %icon
  color: green

```

**CSS**

```css
.icon-error,
.icon-success {
  font-family: "Airglyphs";
}


.icon-error {
  color: red;
}


.icon-success {
  color: green;
}

```

### Nested selectors

**Do not nest selectors more than three levels deep!**

```sass
.page-container
  .content 
    .profile 
      // STOP!
```


When selectors become this long, you're likely writing CSS that is:

* Strongly coupled to the HTML (fragile) *—OR—*
* Overly specific (powerful) *—OR—*
* Not reusable

**Instead lets use the &__ symbol**
```sass
.nav
  &__logo
  &__links
```

This compiles to

```css
.nav__logo {}
.nav__links {}
```

Again: **never nest ID selectors!**

If you must use an ID selector in the first place (and you should really try not to), they should never be nested. If you find yourself doing this, you need to revisit your markup, or figure out why such strong specificity is needed. If you are writing well formed HTML and CSS, you should **never** need to do this.