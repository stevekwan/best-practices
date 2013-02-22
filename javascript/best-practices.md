# JavaScript Coding Standards and Best Practices

## Introduction:

This is the JavaScript best practices and standards guide that I espouse at EA SPORTS.

Although this guide is primarily centered around front-end JavaScript, most of the practices here are equally applicable to back-end JavaScript in node.js.

Author:  
Steve Kwan, Project Lead, EASPORTS.com  
Electronic Arts  
<mail@stevekwan.com>  
<http://www.stevekwan.com/>

Originally from my GitHub:  
<https://github.com/stevekwan/best-practices/>

## If You're A Javascript Noob, Read These:

### [JavaScript: The Good Parts, by Douglas Crockford][good-parts]
The good news is, this book is very short.  The bad news is, it's very dense and will likely require a lot of thinking and re-reading.  But The Good Parts is, in my mind, the definitive guide on how an engineer should approach JavaScript.  If you take nothing else aware from this article, please take away this: read The Good Parts.

### [Constructors considered mildly confusing, by Joost Diepenmaat][constructors-confusing]
If you've ever been confused by how constructors and the prototype property work in JavaScript, this article does a very detailed job of explaining how they work.

### [jQuery API][jquery-api]
jQuery is essential for the modern JavaScript developer.  I recommend you consult their API docs to learn all the library has to offer.

## If You're A Javascript Ninja, Read These:

### [The Surprisingly Elegant JavaScript Type Model, by Kannan Vijayan][javascript-type]
JavaScript's type model is deceptively complicated, but this article does the best job I've ever seen of explaining it.

### [JavaScript Object vs Function experiment, by Steve Kwan][object-function-experiment]
If you liked Kannan's article, you might like this.  It's a really trippy demonstration of the relationship between the Object and Function objects.

### [Partial Application in JavaScript, by Ben Alman][partial-application]
Currying and partial function application are important, but advanced, concepts within functional languages like JavaScript.  When you're ready to truly master these concepts, this article is the best I've seen.  I learned a ton reading it.

### [JavaScript constructor vs prototype experiment, by Steve Kwan][constructor-prototype-experiment]
If you _really_ want to understand constructor vs prototype, here's an in-depth example I came up with.

## Best Practices We Follow:
This section is a work in progress.  I'll eventually add details, explanations and examples to each of the best practices.

### Ensure your site still works without JavaScript.
<!--
* In particular, links...progressive enhancement URL?
-->

### Use the Module Pattern to encapsulate.
<!--
* Provides scope
* Provides public/private support
* By default, works best for singletons (but can be used for true OOP)
-->

### When using the Module Pattern, keep your JavaScript public object as clean as possible.
<!--
* Avoid polluting the global namespace.
-->

### Namespace your JavaScript if you need to refer to it elsewhere.

### Anonymously scope JavaScript if you’re never going to call it elsewhere.

### Put your JavaScript right before the `<body>` tag.
<!--
There are exceptions, such as preventing FOUC.  Generally OK as the JS doesn't
do any waiting for Ajax calls, major DOM restructuring, or number crunching.
-->

### When optimizating, focus on the big things.
<!--
* Document reflows
* Events that get fired ALL THE TIME (eg on resizing)
* Minimizing HTTP requests (and even this is becoming less important than in the past)
* Lazy loading big assets
-->

### `unbind()` all event handlers before binding.
<!--
* Not strictly required, but a good defensive coding practices to prevent events from stacking up.
* jQuery event functions stack, they don't replace.
-->

## Bad Practices We Avoid:
This section is a work in progress.  I'll eventually add details, explanations and examples to each of the best practices.

### Treating JavaScript like a classical OOP language.
<!--
* Prototypal, not classical...read The Good Parts for more info
* Classical OOP results in ugly and complicated JavaScript
* Don't create crazy object structures in JS
    * Rely on the DOM as your object model, or
    * Rely on JSON REST results as your object model.
-->

### Inlining the crap out of functions and object literals.
<!--
* Bad because it leads to major readability issues
* Also bad because it prevents you from accessing inline functions in the future.  And you usually need to if you want to safely unbind() before you bind()
-->

### Excessive optimization.
<!--
* Caching selectors, especially long-term (at most cache for only a function's lifetime)
* Going nuts with minification
-->

### Causing excessive document reflows.

### Going overboard with minification.
<!--
Discuss how some pieces need to be cached across all pages on the site.
-->

### Really long function chains.
<!--
* Avoid chains where you can't detect `null`/`undefined` mid-chain.
-->

### Causing a flash of unstyled content (FOUC) due to late-loading JavaScript.
<!--
* Wait until all content on the page has been loaded (can be detrimental to the UX)
* Put some of the "styling" scripts in the <head> (be wary that this can be a VERY bad practice...)
* CSS3 or JS fade-ins
-->

## "Best" Practices We Disagree With:
This section is a work in progress.  I'll eventually add details, explanations and examples to each of the best practices.
<!--
Many of these come from: [JS adolescence][js-adolescence]
-->

### Putting all `var` declarations at the top.
<!--
Provide example of how this can cause bugs
-->

### Obsessing over indentation, tabs vs spaces, and squiggly bracket placement.

### Obsessing over strict equality.

### Optimizations that try to "outsmart the browser."
<!--
* Inevitably go out of vogue quickly, eg. domain sharding, minimizing HTTP requests (although this is still important)
-->

### Caching selectors for long periods of time.
<!--
Provide example of how this can cause problems
-->

### Combined `var` declarations.

### Excessive chaining at the expense of readibility.

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
