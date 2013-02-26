# JavaScript Coding Standards and Best Practices

## Introduction:

This is a coding standards and best practices guide for JavaScript, and to a lesser extent, jQuery.

This document focuses on __pragmatism, not perfection__.  It does not focus on writing perfect code, but rather, code that will strike the best balance of __value for your time__.

In particular, we are focused on writing JavaScript that:

* does what it's supposed to do
* is easily understood
* is easily maintained.

Anything above and beyond this is considered a nice-to-have, and is not the focus of this document.

This attitude may not be appropriate for open-source projects, but it is definitely appropriate for projects where cost and schedule are key factors.

This is a community-driven document, so please feel free to contribute!

Although this guide is primarily centered around front-end JavaScript, most of the practices here are equally applicable to back-end JavaScript in node.js.

Author:  
Steve Kwan, Project Lead, EASPORTS.com  
Electronic Arts  
<mail@stevekwan.com>  
<http://www.stevekwan.com/>

Originally from my GitHub:  
<https://github.com/stevekwan/best-practices/>

## If You're A JavaScript Noob, Read These:

### [JavaScript: The Good Parts, by Douglas Crockford][good-parts]
The good news is, this book is very short.  The bad news is, it's very dense and will likely require a lot of thinking and re-reading.  But The Good Parts is, in my mind, the definitive guide on how an engineer should approach JavaScript.  If you take nothing else aware from this article, please take away this: read The Good Parts.

### [Best Practices for Speeding Up Your Site, by the Yahoo! Developer Network][yahoo-speed]
In particular, check out the JavaScript section.  This is the best guideline for optimization I'm aware of.

### [Constructors considered mildly confusing, by Joost Diepenmaat][constructors-confusing]
If you've ever been confused by how constructors and the prototype property work in JavaScript, this article does a very detailed job of explaining how they work.

### [jQuery API][jquery-api]
jQuery is essential for the modern JavaScript developer.  Consult their API docs to learn all the library has to offer.

## If You're A JavaScript Ninja, Read These:

### [The Surprisingly Elegant JavaScript Type Model, by Kannan Vijayan][javascript-type]
JavaScript's type model is deceptively complicated, but this article does the best job I've ever seen of explaining it.

### [JavaScript Object vs Function experiment, by Steve Kwan][object-function-experiment]
If you liked Kannan's article, you might like this.  It's a really trippy demonstration of the relationship between the Object and Function objects.

### [Partial Application in JavaScript, by Ben Alman][partial-application]
Currying and partial function application are important, but advanced, concepts within functional languages like JavaScript.  When you're ready to truly master these concepts, this article is one of the best

### [JavaScript constructor vs prototype experiment, by Steve Kwan][constructor-prototype-experiment]
If you _really_ want to understand constructor vs prototype, here's an in-depth example.

## Best Practices We Follow:
Please submit a pull request if you have any suggestions to the best practices below!

### Ensure your site still works without JavaScript.
This is progressive enhancement, a cornerstone of modern web development.  JavaScript should be used to decorate your site with additional functionality, and should not be required for your site to be operational.

If JavaScript is required to get your site to function, that's bad for accessibility and SEO.

The most obvious place where this problem manifests is `<a>` links that require JavaScript.  For example, don't do the following:

```html
<a href="#">There's a JavaScript click handler somewhere else...</a>
<a href="javascript:someJavaScriptFunctionInThisLink()">...</a>
```

If you have a link that absolutely _must_ require JavaScript to function, don't include that link in your DOM.  Instead, add that link dynamically through JavaScript.  That way, users without JavaScript will not see it at all.

### Use the Module Pattern to encapsulate.
The Module Pattern uses functions and closures to provide encapsulation in functional languages like JavaScript.

The Module pattern is the right solution if you want to:

* encapsulate a chunk of code and a "class" doesn't make sense
* provide public/private support (which "classes" don't support).

The Module Pattern tends to work well for a chunk of code that could be described as a Singleton class.

For an excellent explanation of the Module Pattern, read [The Good Parts, by Douglas Crockford][good-parts].

For a thorough demonstration of the Module Pattern, see this [Module Pattern example][module-pattern-experiment].

### Namespace your JavaScript if you need to refer to it elsewhere.
Your JavaScript shouldn't be floating off in the global namespace, where it collides with other stuff you've included.

Although JavaScript doesn't have built-in notions of namespaces, its object model is flexible enough that you can emulate them.  Here's an example:

```js
var MyNamespace = MyNamespace || {};

MyNamespace.MyModule = function()
{
    // Your module is now in a namespace!
}
```

For a more in-depth example of namespacing, see this [Module Pattern example][module-pattern-experiment].

### Anonymously scope JavaScript if you’re never going to call it elsewhere.
If you're writing a one-off chunk of JavaScript that never needs to be referred to by other code, it's wise to anonymously scope it so it won't get accidentally referenced elsewhere.

To do this, just wrap your code in an anonymous function closure:

```js
// An anonymous function that can never be referenced by name...
(function(){
    var x = 123;
    console.log(x);
})(); // Call the anonymous function once, then throw it away!

console.log(x);
```

In the code above, the first `console.log()` will succeed, but the second will fail.  You won't be able to reference `x` outside of the anonymous function.

### When optimizating, focus on the big things.
Some things cause a big dip in performance, such as:

* Excessive DOM changes that force the page to re-render
* Events that get fired _all the time_ (for example, resizing/scrolling)
* Lots of HTTP requests (and even this is becoming less important).

These are problems you should address.  Fixing them could result in less page stuttering.

However, there are a lot of other "problems" that have very little impact on page performance.  Yes, you may be able to shave off a few milliseconds by optimizing your selectors or caching function results, but is it worth the effort?

If your optimizations are making your code uglier (and thus more difficult to maintain), ask yourself: are these "optimizations" worth it?  For most webpages, 1ms isn't going to make or break the user experience.  There are probably better uses of your time.

### Lazy load assets that aren't immediately required.
You can _greatly_ improve load times and reduce bandwidth considerations by lazy loading page assets.

If your page is media-heavy, we recommend investigating JavaScript techniques to load media assets only when they are required.

### `unbind()` all event handlers before binding.
Generally speaking, this is fine:

```js
var handleClick = function()
{
    // Do some stuff
};

var init = function()
{
    $("a.some-link").click(handleClick);
};
```

There's one problem, though: if `init()` ever gets called more than once, that link is going to wind up with `handleClick()` bound to it twice.  And that means it'll execute twice if the link is clicked.

It's important to note that when you use jQuery event handlers, _they stack_.  Two `click()` calls on the same element will result in two function calls when the link is clicked.

In some cases, you can flat-out guarantee that your link handler won't get bound twice.  In the above example, perhaps you know for certain that `init()` is only called a single time, and therefore two click handlers will never appear on your link.  However, sometimes you can't be so sure, and in those instances it's better safe than sorry.

A good defensive coding practice is to unbind an event handler before binding, like so:

```js
var handleClick = function()
{
    // Do some stuff
};

var init = function()
{
    $("a.some-link").unbind(handleClick).click(handleClick);
};
```

In the above example, if `handleClick()` has already been bound to your link, it will be removed before it is rebound, thus ensuring it is only ever bound once.

Note that _relying_ on the above can sometimes be a bad "code smell," because it is often an indication that you don't truly understand how your code is working.  After all, if you really understand what's going on, you should be able to prevent your event handlers from queuing up.

However, in the real world this can be a valuable defensive coding practice to prevent your users from encountering a broken site.

It's probably a good idea to create a helper function that does the `unbind()`/`bind()` automatically, and logs an error if the code tries to double-bind.

<!--
### Support progressive rendering?  Is this better handled in a general optimization article?
-->

## Bad Practices We Avoid:
Please submit a pull request if you have any suggestions to the bad practices below!

### Treating JavaScript like a classical OOP language.
Developers often come into JavaScript expecting it to behave like classical OOP languages.  And this is totally understandable, because JavaScript's syntax would put you under that impression.

But JavaScript is absolutely _not_ a classically object-oriented language.  It's very different from Java, C++, C#, or PHP.  It's function-driven, and the way you program in JavaScript requires a bit of a mental paradigm shift.

For starters, read [The Good Parts, by Douglas Crockford][good-parts].  Yes, this is not the first time you've read that recommendation in this document, but it's worth the read.  Crockford does an excellent job of explaining how JavaScript may not work the way you expect.

<!--
* Prototypal, not classical...read The Good Parts for more info
* Classical OOP results in ugly and complicated JavaScript
* Don't create crazy object structures in JS
    * Rely on the DOM as your object model, or
    * Rely on JSON REST results as your object model.
-->

### Inlining the crap out of functions and object literals.
I don't know about you, but I find nested code like this to be really hard to follow:

```js
var name = 'Steve Kwan';
var company = 'Electronic Arts';

var myFunction = function()
{
    $('form#my-form').submit
    (
        function(event)
        {
            event.preventDefault();
            $.ajax
            (
                '/some_service',
                {
                    type: "POST",
                    data:
                    {
                        name: name,
                        name: company
                    },
                    success: function(data)
                    {
                        callSomeCompletionFunction
                        (
                            {
                                response1: data.value1,
                                response2: data.value2
                            }
                        )
                    },
                    error: function(data)
                    {
                        callSomeErrorHandler
                        (
                            {
                                response1: data.value1,
                                response2: data.value2
                            }
                        )
                    }
                }
            );
        }
    );
};
```

This kind of code is problematic because:

* It causes major readability and maintainability issues
* It prevents you from reusing some of those nested functions
* It prevents you from using the `unbind()`/`bind()` trick mentioned above.

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

### Excessive function chaining at the expense of readibility.

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