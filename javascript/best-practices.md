# JavaScript Coding Standards and Best Practices

## Introduction:

This is the JavaScript best practices and standards guide for PULSE, the internal agency responsible for the EA SPORTS family of websites.

Although this guide is primarily centered around front-end JavaScript, most of the practices here are equally applicable to back-end JavaScript in node.js.

Author:  
Steve Kwan, Project Lead, EASPORTS.com  
Electronic Arts  
[mail@stevekwan.com](mailto:mail@stevekwan.com)  
[http://www.stevekwan.com/](http://www.stevekwan.com/)

Originally from my GitHub:  
https://github.com/stevekwan/experiments/

## If You're A Javascript Noob, Read These:

### [JavaScript: The Good Parts, by Douglas Crockford](http://www.amazon.ca/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742)
The good news is, this book is very short.  The bad news is, it's very dense and will likely require a lot of thinking and re-reading.  But The Good Parts is, in my mind, the definitive guide on how an engineer should approach JavaScript.  If you take nothing else aware from this article, please take away this: read The Good Parts.

### [Constructors considered mildly confusing, by Joost Diepenmaat](http://joost.zeekat.nl/constructors-considered-mildly-confusing.html)
If you've ever been confused by how constructors and the prototype property work in JavaScript, this article does a very detailed job of explaining how they work.

### [jQuery API](http://api.jquery.com/)
jQuery is essential for the modern JavaScript developer.  I recommend you consult their API docs to learn all the library has to offer.

## If You're A Javascript Ninja, Read These:

### [The Surprisingly Elegant JavaScript Type Model, by Kannan Vijayan](http://vijayan.ca/blog/2012/02/21/javascript-type-model/)
JavaScript's type model is deceptively complicated, but this article does the best job I've ever seen of explaining it.

### [JavaScript Object vs Function experiment, by Steve Kwan](https://github.com/stevekwan/experiments/blob/master/javascript/object-vs-function.html)
If you liked Kannan's article, you might like this.  It's a really trippy demonstration of the relationship between the Object and Function objects.

### [Partial Application in JavaScript, by Ben Alman](http://benalman.com/news/2012/09/partial-application-in-javascript/)
Currying and partial function application are important, but advanced, concepts within functional languages like JavaScript.  When you're ready to truly master these concepts, this article is the best I've seen.  I learned a ton reading it.

### [JavaScript constructor vs prototype experiment, by Steve Kwan](https://github.com/stevekwan/experiments/blob/master/javascript/constructor-vs-prototype.html)
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

### Avoid causing document reflows.

### Optimize the big things.
<!--
* Document reflows
* Events that get fired ALL THE TIME (eg on resizing)
-->

### Be sure to `unbind()` before binding.
<!--
* Not strictly required, but a good defensive coding practices to prevent events from stacking up.
* jQuery event functions stack, they don't replace.
-->

### Prevent flash of unstyled content (FOUC).
<!--
* Wait until all content on the page has been loaded (can be detrimental to the UX)
* Put some of the "styling" scripts in the <head> (be wary that this can be a VERY bad practice...)
* CSS3 or JS fade-ins
-->

### Avoid chains where you can't detect `null`/`undefined` mid-chain.

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

### Excessive optimization.
<!--
* Caching selectors, especially long-term (at most cache for only a function's lifetime)
* Going nuts with minification
-->

### Optimizations that try to "outsmart the browser."
<!--
* Inevitably go out of vogue quickly, eg. domain sharding
-->

## "Best" Practices We Disagree With:
This section is a work in progress.  I'll eventually add details, explanations and examples to each of the best practices.
<!--
Many of these come from: [JS adolescence](http://james.padolsey.com/javascript/js-adolescence/)
-->

### Putting all `var` declarations at the top.

### Obsessing over indentation, tabs vs spaces, and squiggly bracket placement.

### Obsessing over strict equality.

### Caching selectors for long periods of time.

### Combined `var` declarations.

### Excessive chaining at the expense of readibility.
