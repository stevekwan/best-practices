# Pragmatic Standards: JavaScript Coding Standards and Best Practices

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

## Bad Practices We Avoid:
Please submit a pull request if you have any suggestions to the bad practices below!

### Treating JavaScript like a classical OOP language.
Developers often come into JavaScript expecting it to behave like classical OOP languages.  And this is totally understandable, because JavaScript's syntax would put you under that impression.

But JavaScript is absolutely _not_ a classically object-oriented language.  It's very different from Java, C++, C#, or PHP.  It's function-driven, and the way you program in JavaScript requires a bit of a mental paradigm shift.

For starters, read [The Good Parts, by Douglas Crockford][good-parts].  Yes, this is not the first time you've read that recommendation in this document, but it's worth the read.  Crockford does an excellent job of explaining how JavaScript may not work the way you expect.

<!--
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
Performant JavaScript is important.  But it's also important to identify if your improvements are providing noticeable benefit.  In some cases, your "improvements" may actually make performance worse!

There are some optimizations that are _always_ worth doing, such as those listed in the best practices section above.  But some just aren't worth it.  If you are obsessing over optimizations to the point where you are talking about shaving off a millisecond or two, and the consequence to your optimizations is hard-to-maintain code, you need to rethink whether you are improving things or making them worse.

### Causing excessive document reflows.
DOM modification is slow, because in addition to rebuilding the DOM tree, the browser must recalculate all element dimensions and see how they fit together.  This is _especially_ a problem when your elements are positioned via `static` (the default) or `relative` - and this is almost always the case.

This is will make the user's browser stutter:

```js
var table = $('<table></table>');

$('body').append(table);

for (var i=0; i<10000; i++)
{
    $('table').append
    (
        '<tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>'
    );
}

window.alert("Done!");
```

Keep your DOM modifications to a bare minimum.

If you must make significant DOM modifications, try to batch up your DOM modifications and do them all at once.  

If you cannot do them all at once, do your modifications in a document fragment and only append to your main document once.  At the very least that'll prevent the browser from having to do complex rendering while the elements are being added.

Compare the performance of the snippet below to the one above.  The one below is much faster!

```js
var table = $('<table></table>');

for (var i=0; i<10000; i++)
{
    $(table).append
    (
        '<tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>'
    );
}

// Append the table at the end this time.  The document has not been modified
// until we do this.
$('body').append(table);

window.alert("Done!");
```

### Going overboard with file concatenation.
If you aren't automatically concatenating your JavaScript, you should be.  Combining all your JavaScript into fewer files results in fewer HTTP requests.  This is good for performance, not just for JavaScript, but for CSS as well.

But concatenating everything into one big, compressed file is not always a good idea.  It's possible to go too far into this direction to the point where you're actually _hurting_ performance in the long term.

Let's say, for example, that you crunch all your site's JavaScript into a single file.  That way it only needs to be downloaded once.  In the long term, this could be beneficial...but in the short term, your visitors will suffer a _dramatically increased load time_ on their first visit, because instead of loading one page's resources, they're loading a whole _site's_.

So let's take it in the other direction: rather than having one big JavaScript file, we'll make a separate (but single) JavaScript file for each page.  That sounds great on paper, but there's a problem: it _doesn't make use of the browser's built-in caching abilities_.

If a lot of your JavaScript is common across your entire site, you want the visitor's browser to cache that JavaScript rather than request it again.  However, if you've crunched all that "common" stuff into individual page bundles, the browser can no longer do this.

The best solution is usually a compromise.  A good way to serve up cached JavaScript is to break things into two files:

1. A "common" file containing JavaScript that is used everywhere
2. A page-specific file containing JavaScript only used on the page requested.

### Really long function chains.
Function chaining like this can be really handy:

```js
$('.some-element').click(handleClick).parents('.container')[0].slideDown();
```

This terse, abbreviated form of jQuery can be very clear and good for performance.  But you can also get yourself into trouble.

In the above example, if the `.container` selector returns no results, you'll encounter a JavaScript error.

Function chaining is great, but be wary of null or undefined values coming back mid-chain.

### Causing a flash of unstyled content (FOUC) due to late-loading JavaScript.
We've all seen it: you load a website, it appears unstyled for a brief period, then it flickers into the right design.  You may not be able to reproduce the issue if you refresh the page.  What happened?

This is called a flash of unstyled content, or FOUC.  It's an old problem originally stemming from incorrect placement of stylesheets in your markup.  However, FOUC is making a resurgence due to late-loading JavaScript that redecorates the page.

It's common knowledge that putting your JavaScript at the bottom of your document leads to better performance.  However, if your JavaScript contains heavy DOM manipulation this may have the unpleasant side effect of your page being rendered twice: once without JavaScript, then a second time once your JavaScript is loaded.

So what's the solution?  Well, you could move all your JavaScript back into the `<head>`, which will result in your JavaScript being loaded before the document itself.  Unfortunately, this causes a completely different (and worse) performance problem: now your page will not render _at all_ until all JavaScript is loaded.  So don't do that.

Unfortunately, there is no good way to prevent this issue that I am aware of.  But there are some things you can do to alleviate the problem:

* **Minimize the amount of DOM manipulation done in JavaScript.**  
    As a rule, minimize or batch up DOM manipulations.  Document reflow is one of the biggest client-side performance hits you can incur.  There's a really in-depth explanation of reflow earlier in this document.
* **Specify the `width`/`height` for the page elements that will be changed by JavaScript.**  
    You can minimize the impact of FOUC by specifying dimensions on all the page regions that will be affected by late-loading JavaScript.

    For example, if your JavaScript renders a component that will always be 200em tall, specify that height in CSS.  That way the browser knows how much space to allocate for your component before it has been loaded, and this will minimize document reflow and jitters when the content comes in.
* **Mask the problem with CSS3 or JavaScript intro animations.**  
    You can slap a quick band-aid over the problem by applying a fade-in or slide-in animation to the element being affected by FOUC.  This hides than problem rather than solving it, but in a lot of cases this may be good enough.

    Note that if the offending late-loading JavaScript comes in _really_ late, masking the problem with an animation may not actually help because the animation will complete before the JavaScript gets executed.
* **Block rendering until all content on the page has been loaded.**  
    In most cases, this is a bad solution.  However, it's still worth discussing.

    You can possible block progressive rendering by hiding the FOUC-affected regions, and only showing them once you know for certain all content is loaded (usually by trapping the `load` event).

    However, blocking progressive rendering almost always makes the UX worse rather than better.  FOUC is bad, but it's often not as bad as refusing to show any content at all.  That said, there may be a few select cases where the FOUC is so offensive and performance-intensive that you need to investigate this option.
* **Inline the offending JavaScript and put it beneath the affected DOM elements.**  
    This is another of those "use sparingly" techniques, because it flies in the face of best practices.  It requires you to litter your DOM with embedded code, which is bad for myriad reasons.

    However, in truly offensive cases of FOUC, this may be the "best" way to solve the problem.

    When the browser parses your document, it executes JavaScript at the exact moment it encounters it.  So to minimize FOUC, you can bring your JavaScript and the elements it affects closer together.  Here's an example:

    ```html
    <div class="fouc-affected-element">
        <script>
            // Do something with .fouc-affected-element to resolve the FOUC
            // issue
        </script>
        ...
    </div>
    ```

    Again, I need to reiterate that this is a somewhat occult technique and should be used _very sparingly_.  It results in ugly, unmaintainable code.  But it also can fix FOUC quite cleanly in some situations.

    If you _do_ choose to use this technique, be aware that `<script>` tags _block progressive rendering_.  So ensure that these inline `<script>` tags execute fast and have no external dependencies.  Don't do any Ajax calls in there.

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
In this document I've used [Allman style][allman-style] indentation.  I find it far more elegant and readable than other styles.  I like its Gestalt.  But I fully understand I am in the minority on this, and I would never enforce this style on anyone else.  :)

I would rather engineers be focused on writing quality semantic code that makes sense.  I don't want talent wasting hours of their time on squigglies and tabs, when we could do that job in minutes with a linter.

It just isn't worth the effort obsessing over rigid indentation/squiggly rules.  Do what you think makes sense in the given context.  If your code is readable and clear, it should speak for itself regardless of the indentation style you choose.

Just be prepared to have your formatting overwritten by my linter.  :)

### Obsessing over strict equality.

The difference between `==` and `===` is important in JavaScript, _especially_ when checking against falsiness/truthiness.

But with that exception, it's not something I lose a lot of sleep over.  Yes, you should endeavour to use `===`, but this isn't the sort of thing that I would turn into a religious war.

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
[allman-style]: http://en.wikipedia.org/wiki/Indent_style#Allman_style