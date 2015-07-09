.Wildebeest CSS Style Guide {
==================

*A mostly reasonable approach to CSS*

## General

### Minimise the use of element selectors
.
Selectors that contain elements tightly coupled the CSS to specific markup. It is not a safe assumption that the semantics of the content will never change so authors should prefer classes which exist independent of markup and create more flexible CSS.

Source: [SMACSS on modules](http://smacss.com/book/type-module)

### Do not use location based selectors

A location based selector is a selector that changes a modules appearance based on its location (content area, side bar, header etc).  Where a module has different appearances it is better to use a module subclass.  If the appearance and/or content is very different it would be better to use a different module

Source: [OOCSS, slide 20](http://www.slideshare.net/stubbornella/object-oriented-css)

### Do not use ID's in CSS Selectors

It is never safe to assume there will only ever be one of something on a page so do not use ID's for CSS.  Id's are much better used as javascript hooks so use them for this instead.

Source: [SMACSS on layout](http://smacss.com/book/type-layout)

### Animate an interface using classes not inline styles

Inline styles added by javascript are harder to update and maintain, prefer to add classes using javascript.  CSS3 transitions can then handle any animations and if CSS3 transitions are not supported the state will still be updated.

Source: [SMACSS on state](http://smacss.com/book/type-state)

### Selectors should be 4 or less levels deep

Long complex selector chains spawn specificity wars and are slower for the browser to evaluate.  Its unlikely you _need_ to use a selector more than 4 levels deep.  If it is, consider adding additional classes to the markup to enable a shorter  selector length.

### Don't use a Preprocessor to produce bloated CSS

Utilise a preprocessors features such as mixins, variables, nested rules and functions to make code easier to maintain but always double check compiled stylesheets to avoid code bloat.  Preprocessors, when used inefficiently, can cause code bloat.

Source: [SMACSS on preprocessors (members only)](https://smacss.com/book/preprocessors)

### Minimise HTTP requests

HTTP Requests are expensive and slow the rendering of a page, so minimise them. Ideally a HTML page should contain just one CSS file with extra CSS being loaded as the page changes.

### Icons: use SVG or Icon Fonts istead of sprite images

There are big advantages to vector icons (SVG or Icon Fonts): resizable up and down without losing quality, extra sharp on retina displays, and small file size among them.

### Do not use units for zero values

The value of `0` works without specifying units in all situations where numbers with units or percentages are allowed. There is no difference between `0px`, `0em`, `0%`, or any other zero-value. The units aren't important because the value is still zero. CSS allows you to omit the units for zero values and still remain valid CSS.

Source: [CSSLint](https://github.com/CSSLint/csslint/wiki/Disallow-units-for-zero-values)

### Do not use overqualified elements

Writing selectors such as li.active are unnecessary unless the element name causes the class to behave differently. In most cases, it's safe to remove the element name from the selector, both reducing the size of the CSS as well as improving the selector performance (doesn't have to match the element anymore). Removing the element name also loosens the coupling between your CSS and your HTML, allowing you to change the element on which the class is used without also needing to update the CSS.

Source: [CSSLint](https://github.com/CSSLint/csslint/wiki/Disallow-overqualified-elements)

### Do not use !important

The `!important` annotation is used to artificially increase the specificity of a given property value in a rule. This is usually an indication that the specificity of the entire CSS has gotten a bit out of control and needs to be refactored. The more frequently `!important` is found in CSS, the more likely it is that developers are having trouble styling parts of a page effectively.

Source: [CSSLint](https://github.com/CSSLint/csslint/wiki/Disallow-!important)

### Always Minimise CSS

Minifying code can save up to 60% on a files size.  Use a tool like [YUI Compressor](http://yui.github.io/yuicompressor/) or [Grunt CSS Min](https://github.com/gruntjs/grunt-contrib-cssmin) together with build tools such as [Grunt](gruntjs.com) or [Maven](maven.apache.org/) to manage this and it's easy-peasy.

Source: [Google Page Speed on Minifying CSS](https://developers.google.com/speed/docs/best-practices/payload#MinifyCSS)

### Always use a CSS Linter

Using a CSS Linter will make obvious whenever a rule is not matching the coding standard and best practices. It will make developers know something is wrong even before looking at it in the browser.
When using Sublime Text a great solution is to use [SublimeLinter](https://github.com/SublimeLinter/SublimeLinter), there are other options for other IDEs and it can also be used online via http://csslint.net/

### Never Use Single line formatting

CSS Architecture is more difficult to do well than it is difficult to understand CSS Properties. Reading long lines of CSS code makes it difficult to understand the properties of the rule. This, together with the correct ordering of the CSS properties make it really easy to understand a rule as first sight.

Source: [SMACSS on formatting](https://smacss.com/book/formatting)

### Use dashes to separate words

CSS properties do not use underscores or camel case, they use dashes.  Do the same with classes


## Responsive Design

### Use "Mobile-First" technique

*Mobile First* technique increase the performance of the site. Usually desktop and large screen devices have more hardware powerful and better internet connections than mobile ones. So instead of declaring large screen rules first only to override them for smaller screens, is better define the mobile rules (which are usually less) and then add more content for larger screens.

```css
.element {
    display: none;
}

@media screen and (min-width: 480px) {
    .element {
        background: url(small-image.png);
        display: inline;
        width: 100%;
    }
}

@media screen and (min-width: 768px) {
    .element {
        background: url(large-image.png);
        width: 50%;
    }
}
```

### Use *font-sizes* in *rem*

Define a global `font-size` in pixels:
```css
body {
    font-size: 16px;
}
```
... and then use `rem` for the rest of declarations:
```css
h1 {
    font-size: 1.5rem; /* 24px / 16px = 1.5rem */
}

h2 {
    font-size: 1.25rem; /* 20px / 16px = 1.25rem */
}
```
... eventually you'll want to use `em` for dynamically changing text size, with a definition in `rem` for the container:
```css
.list-container {
    font-size: 1.25rem;
    ...
}

@media screen and (min-width: 480px) {
    .list-container {
        font-size: 1.5rem;
    }
}

@media screen and (min-width: 768px) {
    .list-container {
        font-size: 1.75rem;
    }
}

.list-container .list-title {
    font-size: 2em;
    ...
}

.list-container .list-subtitle {
    font-size: 1.5em;
    ...
}
```

### Declare media queries by component
Declare media queries in the same component definition instead of a general media query. This practice improves the maintainability

**(Sass code) Bad example:**
```scss
// modules/_button.scss

.list-title {
    ...
}

.list-image {
    ...
}

@media screen and (min-width: 468px) {
    .list-title {
        ...
    }
    
    .list-image {
        ...
    }
}
    
@media screen and (min-width: 768px) {
    .list-title {
        ...
    }
    
    .list-image {
        ...
    }
}

```

**(Sass code) Good example:**
```scss
// modules/_button.scss

.list-title {
    ...
    
    @media screen and (min-width: 468px) {
        ...
    }
    
    @media screen and (min-width: 768px) {
        ...
    }
}

.list-image {
    ...
    
    @media screen and (min-width: 468px) {
        ...
    }
    
    @media screen and (min-width: 768px) {
        ...
    }
}
```

## Sass / Less

### Use variables
Use variables instead of hardcoded values whenever possible. You might also like to use a configuration file for this:
```scss
//  base/_config.scss

// Fonts
$font-family--primary       : "Lato", sans-serif, Helvetica, Arial;
$font-family--secondary     : "Open Sans", sans-serif, Helvetica, Arial;
$font-family--icon          : "icomoon";

$font-size--primary         : 16px;

// Color Pallete
$aquamarine                 : #70CFD9;
$coral-red                  : #FF3643;
$oxford-blue                : #2B3444;
$sahara-sand                : #F5EE9B;

// Transitions
$transition--primary        : all 0.3s ease;
$transition--secondary      : all 0.4s cubic-bezier(0.2, 1, 0.3, 1);

// Responsive
$phone-small                : 320px;
$phone-medium               : 480px;
$phone-large                : 512px;
$phone-xlarge               : 640px;
$tablet                     : 768px;
$desktop-small              : 1024px;
$desktop-medium             : 1280px;
$desktop-large              : 1440px;
```

### Use mixin and variables for media queries
Use a media queries mixin within the pre-setted variables of breakpoints to improves the maintainability

```scss
.list-item {
    ...
    
    @include breakpoint($phone-medium) {
        ...
    }
    
    @include breakpoint($tablet) {
        ...
    }
    
    @include breakpoint($desktop-medium) {
        ...
    }
}
```

### Avoid nesting more than 4 levels

Avoid nesting more than 4 levels to prevent large file sizes (in generated css files)

```scss

// Bad example

.wrapper {
    ...
    
    .main-content {
        ...
      
        .feature-list {
            ...
            
            .feature-list-item {
                ...
                
                .default-button {
                    ...
                }
            }
        }
    }
}


// Good example

.wrapper {
    ...
}

.main-content {
    ...
}

.feature-list {
    ...
}

.feature-list-item {
      ...
      
      .default-button {
          ...
      }
  }
```


## Coding Style

### General coding guidelines

* Use indentation of 4 spaces to improve readability.
* Put spaces after `:` in property declarations.
* Put spaces before `{` in rule declarations.
* Use hex color codes #000 unless using rgba.

Syntax example:

```css
.styleguide-format {
    background: rgba(0, 0, 0, 0.5);
    border: 1px solid #0F0;
    color: #000;
}
```

### Multiple selectors should each be on a single line

```css
/* Bad */
.my-class, .our-class, .your-class {
    border: 1px solid #0F0;
}

/* Good */
.my-class,
.our-class,
.your-class {
    border: 1px solid #0F0;
}
```

### Each property should be on a new line

```css
/* Bad */
.my-class { border: 1px solid #0F0; color: #F00; margin: 20px; }

/* Good */
.my-class {
  border: 1px solid #0F0;
  color: #F00;
  margin: 20px;
}
```

### Use hyphens to name classes

```css
/* Bad */
.demoImage,
.demo_image { 
    ...
}

/* Good */
.demo-image { 
    ...
}
```

### Alphabetize properties to improve the maintainability

```css
/* Bad */
.container { 
    width: 100%;
    color: #000;
    background: #FFF;
    margin: 20px;
}

/* Good */
.container { 
    background: #FFF;
    color: #000;
    margin: 20px;
    width: 100%;
}
```

### Use CSS shorthands whenever possible

```css
/* Bad */
.featured-text {
    background-color: #CCC;
    background-image: url("img/bg.jpg");
    background-position: top right;
    background-repeat: no-repeat;
    margin-bottom: 10px;
    margin-left: 5px;
    margin-right: 15px;
    margin-top: 20px;
}

/* Good */
.featured-text {
    background: #CCC url("img/bg.jpg") no-repeat top right;
    margin: 20px 15px 10px 5px;
}
```

### Use display: inline-block insead of float:left whenever possible

```css
/* Not recommended */
.list-item { 
    float: left;
    ...
}

/* Better */
.list-item { 
    display: inline-block;
    ...
}
```

### Do not duplicate background images

```css
/* Bad */
.heart-icon {
    background: url(sprite.png) -16px 0 no-repeat;
}

.task-icon {
    background: url(sprite.png) -32px 0 no-repeat;
}

/* Good */
.icons {
    background: url(sprite.png) no-repeat;
}

.heart-icon {
    background-position: -16px 0;
}

.task-icon {
    background-position: -32px 0;
}
```

### Use a semicolon after each declaration
```css
/* Bad */
.my-class {
  border: 1px solid #0F0;
  color: #F00
}

/* Good */
.my-class {
  border: 1px solid #0F0;
  color: #F00;
}
```

## Architecture

### Split CSS into Layout, Module & Pages files

Splitting CSS into multiple files in a constant way makes it easier to find the CSS you are looking for.

### Keep CSS files under 500 lines of code

It sucks to have to search through 1000+ lines of code, so avoid it.  This rule may mean having to split the CSS module file into multiple files. When doing this make sure to only serve one file to the user.

}
=
