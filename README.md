CSS coding style
=====================

This document defines formatting and style rules for CSS. It aims at improving collaboration, code quality, and enabling supporting infrastructure.

1. **[General principles](#1-general-principles)**
2. **[Whitespace](#2-whitespace)**
3. **[Comments](#3-comments)**
4. **[Format](#4-format)**
    * [Anatomy of rulesets](#anatomy-of-rulesets)
    * [Exceptions and slight deviations](#exceptions-and-slight-deviations)
5. **[Less / Sass notation](#5-less--sass-notation)**
6. **[Declaration order](#6-declaration-order)**
    * [Exceptions and slight deviations](#exceptions-and-slight-deviations-1)
7. **[Bad practice to avoid](#7-bad-practice-to-avoid)**
    * [Avoid undoing styles](#avoid-undoing-styles)
    * [Avoid magic numbers](#avoid-magic-numbers)
    * [Avoid hard-coded/absolute values](#avoid-hard-codedabsolute-values)
    * [Avoid brute forcing](#avoid-brute-forcing)
    * [Avoid !important. Hacking specificity](#avoid-important-hacking-specificity)
8. **[Writing selectors](#8-writing-selectors)**
    * [Do not use IDs for styling](#do-not-use-ids-for-styling)
    * [Do not use qualified selectors](#do-not-use-qualified-selectors)
    * [Do not use dangerous selectors](#do-not-use-dangerous-selectors)
    * [Do not use loose class names](#do-not-use-loose-class-names)
    * [Keep your selectors as short as possible](#keep-your-selectors-as-short-as-possible)
9. **[Naming conventions](#9-naming-conventions)**
    * [Root modules](#root-modules)
    * [States or Modifiers](#states-or-modifiers)
    * [Sub-modules](#sub-modules)
    * [js-* classes for javascript selectors](#js--classes-for-javascript-selectors)
    * [is-* prefix for state names](#is--prefix-for-state-names)
    * [Content-independent class names](#content-independent-class-names)
    * [The “multi-class” pattern](#the-multi-class-pattern)
10. **[General rules about writing CSS selectors](#10-general-rules-about-writing-css-selectors)**

## 1. General principles

* Don't try to prematurely optimize your code; keep it readable and understandable.
* All code in any code-base should look like a single person typed it, even when many people are contributing to it.
* Strictly enforce the agreed-upon style.
* If in doubt when deciding upon a style use existing common patterns.

## 2. Whitespace

* Use 4 spaces for indents.
* Never mix spaces and tabs for indentation.
* Remove end-of-line whitespace.
* Set encoding to UTF-8 without BOM.
* Add new line at end of files.

## 3. Comments

* Place comments on a new line above their subject.
* Keep line-length to a sensible maximum, e.g., 80 columns.
* Place only short comments after a declaration. If comment is too long use shortcuts and place the comment above ruleset

Reasons for limit width to 80 characters width:

* the ability to have multiple files open side by side;
* viewing CSS on sites like GitHub, or in terminal windows;
* providing a comfortable line length for comments.

```css

/* Allow only vertical resizing of textareas. */

textarea {
    resize: vertical;
}

/*
Clearfix: contain floats

For modern browsers

1. The space content is one way to avoid an Opera bug when the
   `contenteditable` attribute is included anywhere else in the document.
   Otherwise it causes space to appear at the top and bottom of elements
   that receive the `clearfix` class.

2. The use of `table` rather than `block` is only necessary if using
   `:before` to contain the top-margins of child elements.
*/

.clearfix:before,
.clearfix:after {
    content: " "; /* 1 */
    display: table; /* 2 */
}

.clearfix:after {
    clear: both;
}

```


## 4. Format

The chosen code format must ensure that code is: easy to read; easy to clearly comment; minimizes the chance of accidentally introducing errors; and results in useful diffs and blames.

### Anatomy of rulesets

```
[selector],
[selector] {
    [property]: [value];
    [<- Declaration -->]
}
```

* Use one discrete selector per line in multi-selector rulesets.
* Include a single space before the opening brace of a ruleset.
* Include one declaration per line in a declaration block.
* The opening brace (`{`) on the same line as our last selector.
* Our first declaration on a new line after our opening brace (`{`).
* Our closing brace (`}`) on its own new line.
* Use one level of indentation for each declaration.
* Indent vendor prefixed declarations so that their values are aligned for easy multi-line editing.
* Include a single space after the colon of a declaration.
* Use lowercase and shorthand hex values, e.g., `#aaa`.
* Use double quotes, e.g., `content: ""`.
* Quote attribute values in selectors, e.g., `[type="checkbox"]`.
* Where allowed, avoid specifying units for zero-values, e.g., `margin: 0`.
* Include a space after each comma in comma-separated property or function values, e.g. `font-family: helvetica, arial, sans-serif;`.
* Include a semi-colon at the end of the last declaration in a declaration block.
* Place the closing brace of a ruleset in the same column as the first character of the ruleset.
* Separate each ruleset by a blank line.

```css

.selector-1,
.selector-2,
.selector-3[type="text"] {
    -webkit-box-sizing: border-box;
       -moz-box-sizing: border-box;
            box-sizing: border-box;
    display: block;
    font-family: helvetica, arial, sans-serif;
    color: #333;
    background: #fff;
    background: linear-gradient(#fff, rgba(0, 0, 0, 0.8));
}

.selector-a,
.selector-b {
    padding: 10px;
}

```

### Exceptions and slight deviations

Large blocks of repetitive declarations can use a slightly different, single-line format to make it easier to compare similar rules. In this case, a space should be included after the opening brace and before the closing brace.

```css

.ico {
    display: inline-block;
    background: url("ico-accout-sprite.png") top left no-repeat;
}

.ico-hint    { width:20px; height:17px; background-position:0    0; }
.ico-notify  { width:39px; height:39px; background-position:0  -17px; }
.ico-warning { width:31px; height:29px; background-position:0  -56px; }
.ico-print   { width:13px; height:15px; background-position:0  -85px; }
.ico-logout  { width:14px; height:20px; background-position:0 -100px; }

```

These types of ruleset benefit from being single-lined because

* they still conform to the one-reason-to-change-per-line rule;
* they share enough similarities that they don’t need to be read as thoroughly as other rulesets—there is more benefit in being able to scan their selectors, which are of more interest to us in these cases.


Long, comma-separated property values — such as collections of gradients or shadows — can be arranged across multiple lines in an effort to improve readability and produce more useful diffs. There are various formats that could be used; one example is shown below.

```css

.selector {
    background-image:
        linear-gradient(#fff, #ccc),
        linear-gradient(#f3c, #4ec);
    box-shadow:
        1px 1px 1px #000,
        2px 2px 1px 1px #ccc inset;
}

```

This makes life a little easier for developers whose text editors support column editing, allowing them to change several identical and aligned lines in one go.

## 5. Less / Sass notation

When using nested selectors follow the rules:

* Indent each nested selector or @-rule with 4 spaces.
* Seperate each rule by a blank line.
* Don't use blank lines between multiple closing brace (`}`).


Example:

```less

.product--name {
    position: relative;
    margin: .5em 0 0;
    font-size: 1.2em;
    text-transform: uppercase;
    white-space: nowrap;

    sup {
        position: absolute;
        top: .3em;
        margin-left: .2em;
        font-size: 1em;
    }

    @media (min-width: 640px) {
        margin-top: -1.75em;

        span {
            padding: 0 0.5em;
            background-color: #fff;
        }
    }
}

```

Nesting in Less / Sass should be avoided wherever possible because of specifity problem.


## 6. Declaration order

Related property declarations should be grouped together following the order:

1. Positioning
2. Display & Box model
3. Typographic
4. Visual
5. ...

Positioning comes first because it can remove an element from the normal flow of the document and override box model related styles. The box model comes next as it dictates a component's dimensions and placement. Definitions in box model section ordered from outer to inner, e.g. `margin → border → padding → width/height`.

If you are using global `box-sizing: border-box` then definitions should stick to that order `margin → width/height → border → padding`.

Everything else takes place inside the component or without impacting the previous two sections, and thus they come last.

```css

.declaration-order {
    /* Positioning */
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 100;

    /* Box-model */
    display: block;
    float: right;
    margin: 10px;
    padding: 20px;
    width: 100px;
    height: 100px;

    /* Typography */
    font: normal 13px "Helvetica Neue", sans-serif;
    line-height: 1.5;
    text-align: center;

    /* Visual */
    color: #333;
    background-color: #f5f5f5;
    border: 1px solid #e5e5e5;
    border-radius: 3px;
    box-shadow: 0 0 3px rgba(0,0,0,0.15);

    /* Misc */
    opacity: 1;
    -webkit-transition: 0.5s;
            transition: 0.5s;
}

```

### Exceptions and slight deviations

Some defenitions may come in different sections depending on situation.

For example, sometimes `border` can affect elements dimentions, sometimes not.

```css

/* 1. Border-width affects dimmentions because width is not «auto» */

.sample-1 {
    /* Box-model */
    margin: 10px;
    border: 5px solid #e5e5e5; /* 1 */
    padding: 10px;
    width: 50%;
}

/* 
1. Border-width doesn't affects dimmentions,
   It's more visual rather than box-model

2. Padding is no more affects dimmentions,
   so it can be placed below width.
*/

.sample-2 {
    /* Box-model */
    -webkit-box-sizing: border-box;
       -moz-box-sizing: border-box;
            box-sizing: border-box;
    margin: 10px;
    width: 50%;
    padding: 10px; /* 2 */

    /* Visual */
    border: 5px solid #e5e5e5; /* 1 */
}

```

Another example, when typography definitions may apply for layouting:

```css

/* 1. Text-align used for layouting inner elements */
/* 2. Vertical-align affects layout, so it is box-model definition, not typography */

.sample-3-outer {
    display: block;
    text-align: justify; /* 1 */
}

.sample-3-inner {
    display: inline-block;
    vertical-align: middle; /* 2 */
}

```
## 7. Bad practice to avoid

### Avoid undoing styles

Any CSS declarations that unsets styles (apart from in a reset) are *typically* bad news. If you are having to remove borders, you probably applied them too early. Rulesets should only ever inherit and add to previous ones, never undo.

Example:

```css

h2 {
    font-size: 2em;
    margin-bottom: 0.5em;
    padding-bottom: 0.5em;
    border-bottom: 1px solid #ccc;
}

.blog-roll h2 {
    padding-bottom: 0;
    border-bottom: none;
}

```

That's means you added `border-bottom` to early. You should rewrite you code like this:

```css

h2 {
    font-size: 2em;
    margin-bottom: 0.5em;
}

.page-headline {
    padding-bottom: 0.5em;
    border-bottom: 1px solid #ccc;
}

```


### Avoid magic numbers

A magic number is a value that is used ‘because it just works’. Take the following example:

```css

.site-nav {
    /*styles*/
}

.site-nav > li:hover .dropdown {
    position: absolute;
    top: 37px;
    left: 0;
}

```

`top: 37px;` — is a magic number. It works only because `li` inside `.site-nav` happen to be 37px tall, and the `.dropdown` flyout menu needs to appear at the bottom of it.

If someone changes the `font-size` in `.site-nav` and now everything become 29px tall, this number is no longer valid and the next dev needs to know to update it. In that example should be `top: 100%` instead 37px.

If you had a more complex example which used a magic number —and that magic number became invalid— you are faced with one or more of the following problems:

* The next dev doesn’t know where the magic number came from, so they delete it and are back at square one.
* The next dev is a cautious dev who, because he doesn’t know where the magic number came from, decides to try and fix the problem without touching that magic number. This means that an old, outdated, hacky magic number stays in the code, and the next dev simply hacks away on top of it. You are now hacking on top of a hack.


### Avoid hard-coded/absolute values

Not unlike magic numbers, hard-coded values are also bad news. A hard-coded value might be something like this:

```css

h1 {
    font-size: 24px;
    line-height: 32px;
}

```

`line-height: 32px;` here is not cool, it should be `line-height: 1.333…`. If you ever change the `font-size` of a `h1`, you want to know that your `line-height` will track it.

### Avoid brute forcing

This one is in a similar vein to hard-coded numbers, but a little more specific. Brute forcing CSS is when you use hard-coded magic numbers and a variety of other techniques to force a layout to work. Take for example:

```css

.foo {
    margin-left: -3px;
    position: relative;
    z-index: 99999;
    height: 59px;
    float: left;
}

```

### Avoid `!important`. Hacking specificity

Sometimes you need to increase specificity of some rule. Do not use `!important`, boost specificity of selector instead.

You can chain a selector with itself to increase its specificity:

```css
.btn.btn { }
```

…will select based on only one class (.btn) but with double the specificity. We can take this as far as we need to:

```css
.btn.btn.btn.btn { }
```

We could also add another selector to the `.btn {}` ruleset, for example `.box .btn {}`, but this isn’t a very maintainable solution at all: it depends on a context. We can’t really keep on adding a new selector every time we need to bump up specificity.

Chaining selector with itself boosts specificity with very little maintenance overhead, and no reliance on a location or context. But this still a hack.

## 8. Writing selectors

In short: **never use a selector more specific than the one you need**.

### Do not use IDs for styling

They have no advantage over classes (anything you can do with an ID, you can do with a class), they cannot be reused, and their specificity is too high. Even an infinite number of chained classes will not trump the specificity of one ID.

Let’s imagine you have this third-party widget embedded on your page, and you want to style it:

```html
<div id="widget">
    ...
</div>
```

Naturally, given that we can’t edit this HTML to use a class instead of (or alongside) the ID, we’d opt for something like this:

```css
#widget {
    ...
}
```
Now we have an ID in our CSS with very hight specificity. Instead, we should do something like this:

```css
[id="widget"] {
    ...
}
```

This selector has the exact same specificity as a class, so we’re selecting a chunk of the DOM based on an ID, but never actually increasing our specificity beyond that of our classes.

### Do not use qualified selectors

Do not qualify selectors unless you have a compelling reason to do so. If `.nav {}` will work, do not use `ul.nav {}`; to do so would not only limit the places you can use the `.nav` class, but it also increases the specificity of the selector, again, with no real gain.

More extreme examples might be:

```css

ul.nav li.active a {}
div.header a.logo img {}
.content ul.features a.button {}

```

All of these selectors can be trimmed down massively, or totally rewritten, to:

```css

.nav .active a {}
.logo > img  {}
.features-button {}

```

Which will help us:

* Save actual amounts of code
* Increase performance
* Allow greater portability
* Reduce specificity

### Do not use dangerous selectors

A ‘dangerous selector’ is one with far too broad a reach.

```css

header {
    padding: 1em;
    background-color: #BADA55;
    color: #fff;
    margin-bottom: 20px;
}

```

The `header` element **does not** mean ‘your site’s main header’ and, as per the spec, the header element can be used multiple times in multiple contexts. This should be targeted via a selector more like `.site-header {}`, for example.

To give such specific styling to such a generic selector is dangerous. Your styles will leak out into areas they shouldn’t as soon as you start trying to use that element again, and you’ll need to start undoing styles (adding more code to take styles away) in order to combat this.

### Do not use loose class names

A ‘loose’ class name is one that isn’t specific enough for its intended purpose. Imagine a class of `.card`. What does this do?

This class name is very loose, and loose class names are very bad for two main reasons:

* You can’t necessarily glean its purpose from the class alone.
* It’s so vague that it could very easily be redefined accidentally by another dev.

Some examples:

```
.card   →  .credit-card-image

.header →  .page-header
           .article-header
           .section-header
           .widget-header
           
.content → .user-generated-content
           .article-content
           .comment-content
           .widget-content
           
.right  →  .right-aligned-img
           .text-right
           .float-right
           
```

### Keep your selectors as short as possible

Do not nest selectors unnecessarily. If `.header-nav {}` will work, never use `.header .header-nav {}`; to do so will literally double the specificity of the selector without any benefit.

## 9. Naming conventions

### Root modules

For root module prefer solid single world class-names without hyphens between words:  
  `.pageheader` better than `.page-header`;  
  `.topnav` better than `.top-nav`.

**Why?**

* Easier to select whole word on double-click,
* Visually not mixed with sub-modules or modifiers

**Exclusions**

Use hyphen when class name is too long or double-meaning appears (to improve readability).

### States or Modifiers

Use single hyphen «-» as a separator between module name and its state or modifier:  
`.btn` and `.btn-large`, `.btn-primary`.

Do not use state or modifier name without module prefix.

`<div class="btn large">...` — is bad.  
`<div class="btn btn-large">...` — is good.

### Sub-modules

Use single hyphen «-» as a separator between module name and sub-module name:

```css
.panel {...}
.panel-header {...}
.panel-body {...}
.panel-actions {...}

.menu {...}
.menu-link {...}
.menu-icon {...}
.menu-dropdown {...}
```

Sub-modules above have no strict dependence from root class. They can be used inside wrappers or other sub-modules.


Use double hyphen «--» for elements that can exists only inside more complex constructions (parent class required). For example:

```html
<ul class="grid grid-justified">
 <li class="grid--item"></li>
 <li class="grid--item"></li>
 <li class="grid--item"></li>
</ul>
```

```css
.grid-justified {
	display: table;
	table-layout: fixed;
}

.grid-justified > .grid--item {
	display: table-cell;
}
```

`.grid--item` is meaningless outside of a `.grid` tag.

### `js-*` classes for javascript selectors

As a rule, it is unwise to bind your CSS and your JS onto the same class in your HTML. This is because doing so means you can’t have (or remove) one without (removing) the other. It is much cleaner, much more transparent, and much more maintainable to bind your JS onto specific classes.

Typically, these are classes that are prepended with `js-`, for example:

```html
<input type="submit" class="btn  js-btn" value="Follow" />
```

This means that we can have an element elsewhere which can carry with style of `.btn {}`, but without the behaviour of `.js-btn`.

**`data-*` Attributes**

A common practice is to use `data-*` attributes as JS hooks, but this is incorrect. `data-*` attributes, as per the spec, are used to store custom data private to the page or application. `data-*` attributes are designed to store data, not be bound to.

### `is-*` prefix for state names

While `js-*` prefix is used to bind JS, usually we need to change state of an element dynamically. Use `is-*` prefix for that: `.is-active`, `.is-opened`, `.is-closed`, `.is-expanded`, `is-collapsed` ect.

But don't apply styles directly to that classes. Use multi-class pattern instead: chain state class with module class.

```css

.menu-item.is-active { ... }

.dropdown.is-opened { ... }

.aside-menu.is-expanded .aside-ads { ... }

```

### Content-independent class names

Aim for high reusability when naming things.

Tying your class name semantics tightly to the nature of the content has already reduced the ability of your architecture to scale or be easily put to use by other developers.

```
.site-nav → .primary-nav

.footer-links → .sub-links

.news-header → .section-header

.btn-login → .btn-primary
```

Name things for people; they’re the only things that actually _read_ your classes (everything else merely matches them). Once again, it is better to strive for reusable, recyclable classes rather than writing for specific use cases. Let’s take an example:

```css
/**
 * Runs the risk of becoming out of date; not very maintainable.
 */
.blue {
    color: blue;
}

/**
 * Depends on location in order to be rendered properly.
 */
.header span {
    color: blue;
}

/**
 * Too specific; limits our ability to reuse.
 */
.header-color {
    color: blue;
}

/**
 * Nicely abstracted, very portable, doesn’t risk becoming out of date.
 */
.highlight-color {
    color: blue;
}
```

It is important to strike a balance between names that do not literally describe the style that the class brings, but also ones that do not explicitly describe specific use cases.

### The “multi-class” pattern

Components often have variants with slightly different presentations from the base component, e.g., a different coloured background or border. There are two mains patterns used to create these component variants.

**The “single-class” pattern**

```css
.btn,
.btn-primary { /* button template styles */ }

.btn-primary { /* styles specific to primary button */ }
```

```html
<button class="btn">Default</button>
<button class="btn-primary">Login</button>
```

**The “multi-class” pattern**

```css
.btn { /* button template styles */ }
.btn-primary { /* styles specific to primary button */ }
```

```html
<button class="btn">Default</button>
<button class="btn btn-primary">Login</button>
```

“Multi-class” is more scalable pattern. For example, take the base `btn` component and add a further 5 types of button and 3 additional sizes. Using a “multi-class” pattern you end up with 9 classes that can be mixed-and-matched. Using a “single-class” pattern you end up with 24 classes.

It is also easier to make contextual tweaks to a component, if absolutely necessary. You might want to make small adjustments to any `btn` that appears within another component.

```css
/* "multi-class" adjustment */
.thing .btn { /* adjustments */ }

/* "single-class" adjustment */
.thing .btn,
.thing .btn-primary,
.thing .btn-danger,
.thing .btn-etc { /* adjustments */ }
```

A “multi-class” pattern means you only need a single intra-component selector to target any type of `btn`-styled element within the component. A “single-class” pattern would mean that you may have to account for any possible button type, and adjust the selector whenever a new button variant is created.

## 10. General rules about writing CSS selectors

* **Select what you want explicitly**, rather than relying on circumstance or coincidence. Good Selector Intent will rein in the reach and leak of your styles.
* **Write selectors for reusability**, so that you can work more efficiently and reduce waste and repetition.
* **Do not nest selectors unnecessarily**, because this will increase specificity and affect where else you can use your styles.
* **Do not qualify selectors unnecessarily**, as this will impact the number of different elements you can apply styles to.
* **Keep selectors as short as possible**, in order to keep specificity down and performance up.

Focussing on these points will keep your selectors a lot more sane and easy to work with on changing and long-running projects.
