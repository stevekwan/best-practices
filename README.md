# Coding Standards and Best Practices

Standards and best practices guides for various front-end technologies.

Author:  
Steve Kwan, Project Lead, EASPORTS.com  
Electronic Arts  
<mail@stevekwan.com>  
<http://www.stevekwan.com/>

Originally from my GitHub:  
<https://github.com/stevekwan/best-practices/>

## JavaScript:

### [Common JavaScript "Gotchas"][javascript-gotchas]

### [JavaScript Coding Standards and Best Practices Guide][javascript-best-practices]

<!--
### JS optimizations?
### JavaScript Style Guide (recommended but not required)
-->

## CSS:

### Nothing here yet.

<!--
### CSS Coding Standards and Best Practices Guide
* prefer ems to pixels
	* even for media queries
	* URL
* use a pre processor, preferably LESS.
 * layering of styles...app, site, page, component
* Long selectors vs short
	* > 2 levels of selection is a bad code smell...inefficient, but more importantly, means you're getting into the land of weird selector specificity.  Causes bugs later.  Use .scope .specific pattern
* DOM structural dependency
* Classes that describe what they do instead of what they are for
* Adding tons classes to markup for reuse
* global, unscoped styles
* Only make styles global/shared if you are ABSOLUTELY SURE they need to be
	* "it might come in handy some day" doesn't cut it
	* easy to make things global later if needed...very hard to refactor global stuff into local if you aren't sure where it's used
* components should not be attached to styles that specify page-specific rules
* Id vs class styling
* Monolithic CSS files
* browser-specific hacks.
* avoid crazy percentage chaining
	* percentages not bad - required for RWD - but percentage chaining is
	* comes up with things like fonts
	* use LESS/Sass variables instead
* no @import (unless with a preprocessing that does this the right way)
	* latency
* understand how CSS affects progressive rendering
### Separation of concerns in CSS?
### CSS gotchas?
* 31 style sheet IE rule
* internal network ie rule...document standards mode
* quirks mode

### CSS optimizations?
 ### CSS Style Guide (recommended but not required)
-->

[javascript-gotchas]: https://github.com/stevekwan/best-practices/blob/master/javascript/gotchas.md
[javascript-best-practices]: https://github.com/stevekwan/best-practices/blob/master/javascript/best-practices.md
[good-parts]: http://shop.oreilly.com/product/9780596517748.do
[constructors-confusing]: http://joost.zeekat.nl/constructors-considered-mildly-confusing.html
[jquery-api]: http://api.jquery.com/
[javascript-type]: http://vijayan.ca/blog/2012/02/21/javascript-type-model/
[partial-application]: http://benalman.com/news/2012/09/partial-application-in-javascript/
[js-adolescence]: http://james.padolsey.com/javascript/js-adolescence/
[object-function-experiment]: https://github.com/stevekwan/experiments/blob/master/javascript/object-vs-function.html
[constructor-prototype-experiment]: https://github.com/stevekwan/experiments/blob/master/javascript/constructor-vs-prototype.html
