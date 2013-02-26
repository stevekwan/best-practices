# CSS Coding Standards and Best Practices

## Introduction:

This is the CSS best practices and standards guide that we use at PULSE, the web agency for EA SPORTS.

This is a community-driven document, so please feel free to contribute!

Author:  
Steve Kwan, Project Lead, EASPORTS.com  
Electronic Arts  
<mail@stevekwan.com>  
<http://www.stevekwan.com/>

Originally from my GitHub:  
<https://github.com/stevekwan/best-practices/>

<!--
Is this section really necessary for CSS?

## If You're A CSS Noob, Read These:

## If You're A CSS Ninja, Read These:

-->

## Best Practices We Follow:
This section is a work in progress.  I'll eventually add details, explanations and examples to each of the best practices.

### Prefer ems to pixels.
<!--
* even for media queries
* URL
* Understand pixels are not as absolute as you may think they are.
-->

### Use a CSS preprocessor, preferably LESS.
<!--
* Sass is great too
-->

### Apply your stylesheets in "layers," so you can peel them back as needed.
<!--
* app, site, page, component
-->

### Keep your CSS selectors short.
<!--
* \> 2 levels of selection is a bad code smell...inefficient, but more importantly, means you're getting into the land of weird selector specificity.  Causes bugs later.  Use .scope .specific pattern
-->

### Your CSS classes should describe the content, not the look.
<!--
* Avoid classes with names like `blue` or `float-left`.
-->

### Decouple your styles from the code behind.
<!--
* components should not be attached to styles that specify page-specific rules
-->

### Select using id vs class where appropriate.

### Optimize your CSS for fast progressive rendering.
<!--
* Specifying width for images, etc.
-->

## Bad Practices We Avoid:
This section is a work in progress.  I'll eventually add details, explanations and examples to each of the best practices.

### Selectors that are dependent on the DOM's structure.
<!--
    #mySection .myComponent
    {
        ...
    }

    .section:first-child > div > .myComponent
    {
        /* Slower and also likely to break if the DOM structure is ever changed...eg a new <div> is added */
    }
* Ties into the previous point about minimizing the complexity of your selectors.  If selectors are overly long, that often means there is a dependency on DOM structure
-->

### Adding tons of classes to your markup for "reuse."
<!--
    <div class="contact-info padded float-left bold">
        ...
    </div>
-->

### Unscoped styles at the global level.
<!--
* Only make styles global/shared if you are ABSOLUTELY SURE they need to be
* "it might come in handy some day" doesn't cut it
* easy to make things global later if needed...very hard to refactor global stuff into local if you aren't sure where it's used
-->

### Monolithic CSS files.

### Browser-specific hacks.
<!--
* Causes confusion
* Usually not as necessary as you think (think progressive enhancement)
* Causes problems down the road as there is no guarantee future versions of the browser will behave the same, or other browsers will parse the style properly
* Always safer to stick to properly structured CSS
-->

### Crazy percentage chaining.
<!--
    body
    {
        font-size: 13px;
    }
    
    .main-content
    {
        font-size: 120%;
    }
    
    .main-content header
    {
        font-size: 75%;
    }
* comes up with things like fonts
* percentages not bad - required for RWD - but percentage chaining is
* Rarely in the real world are size dependencies like this actually useful.  Just confusing
* use LESS/Sass variables instead
-->

### @import (unless with a preprocessor that fixes this under the hood)
<!--
* latency
-->

### !important

<!--
Is this section really necessary for CSS?
## "Best" Practices We Disagree With:
This section is a work in progress.  I'll eventually add details, explanations and examples to each of the best practices.
-->

<!-- My CSS documentation -->
[css-gotchas]: https://github.com/stevekwan/best-practices/blob/master/css/gotchas.md
[css-best-practices]:https://github.com/stevekwan/best-practices/blob/master/css/best-practices.md
[css-style-guide]:https://github.com/stevekwan/best-practices/blob/master/css/style-guide.md

<!-- My JavaScript documentation -->
[javascript-gotchas]: https://github.com/stevekwan/best-practices/blob/master/javascript/gotchas.md
[javascript-best-practices]: https://github.com/stevekwan/best-practices/blob/master/javascript/best-practices.md
[javascript-style-guide]:https://github.com/stevekwan/best-practices/blob/master/javascript/style-guide.md
[object-function-experiment]: https://github.com/stevekwan/experiments/blob/master/javascript/object-vs-function.html
[constructor-prototype-experiment]: https://github.com/stevekwan/experiments/blob/master/javascript/constructor-vs-prototype.html
[module-pattern-experiment]: https://github.com/stevekwan/experiments/blob/master/javascript/module-pattern.html

<!-- External documentation -->
[good-parts]: http://shop.oreilly.com/product/9780596517748.do
[constructors-confusing]: http://joost.zeekat.nl/constructors-considered-mildly-confusing.html
[jquery-api]: http://api.jquery.com/
[javascript-type]: http://vijayan.ca/blog/2012/02/21/javascript-type-model/
[partial-application]: http://benalman.com/news/2012/09/partial-application-in-javascript/
[js-adolescence]: http://james.padolsey.com/javascript/js-adolescence/
[yahoo-speed]: http://developer.yahoo.com/performance/rules.html
[javascript-hoisting]: http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting