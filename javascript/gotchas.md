# Common JavaScript "Gotchas"

## Introduction:

JavaScript has a lot of weird behaviours that trip up noobs to the language - especially those acquainted with more traditional OOP languages.  Hopefully this guide will provide a quickly scannable, easily understood list to save a lot of pain to those getting acquainted with the language.

The intended audience for this article are engineers from other programming languages who are working with JavaScript for the first time.  This article will not focus on detailed explanations of why the language works the way it does.  It's merely intended to get you past the early hurdles quickly.

JavaScript is a flexible language with many ways to achieve the same result.  What I document below is not the single "best" way.  Rather, it's a series of strategies I use to write code in such a way that it is less confusing to people new to the language.

If you take nothing else away from this article, I __highly__ recommend you read [JavaScript: The Good Parts, by Douglas Crockford][good-parts].  It's the single best resource I'm aware of for getting new JavaScript developers up to speed.

Author:  
Steve Kwan, Project Lead, EASPORTS.com  
Electronic Arts  
<mail@stevekwan.com>  
<http://www.stevekwan.com/>

Originally from my GitHub:  
<https://github.com/stevekwan/best-practices/>

## Common Javascript "Gotchas":

Let's talk about some of the JavaScript 101 problems that noobs to the language encounter.  If you're relatively unfamiliar with JavaScript, I highly recommend you read this section - it'll save you a lot of pain down the road.

### The `var` keyword: what exactly does it do?
`var` declares a variable.  But...you don't need it.  Both these will work:

```js
var myFunction = function()
{  
    var foo = 'Hello'; // Declares foo, scoped to myFunction
    bar = 'Hello';     // Declares bar...in global scope?
};
```

But you should never, EVER use the latter.  See the comment for the reason why.

If you ever forget the var keyword, your variable will be declared...but it will be scoped __globally__ (to the `window` object), rather than to the function it was declared in.

This is extremely bizarre behaviour, and really an unfortunate design decision in the JavaScript language.  There are no pragmatic situations where you would want local variables to be declared globally.  So remember to __always__ use `var` in your declarations.

### Why are there so many different ways to declare a function?  What are the differences?
There are three common ways to define a function in JavaScript:

```js
myFunction1 = function(arg1, arg2) {};     // NEVER do this!
function myFunction2(arg1, arg2) {};       // This is OK, but...
var myFunction3 = function(arg1, arg2) {}; // This is best!
```

The first option is bad because it declares a function like a variable, but without the `var` keyword.  Read the above point to understand why that's bad...it gets created as a global function!

The second option is better, and is properly scoped, but it leads to a slightly more complicated syntax once you get into closures.  It can also cause your code to behave in ways you may not expect due to [JavaScript variable hoisting][javascript-hoisting].

The third option is best, and is syntactically consistent with the rest of your variables.

As an alternative to the third option, you can use:

```js
var myFunction4 = function myFunction4(arg1, arg2) {};
```

Although more verbose, this option can be useful because it provides a little more context when debugging.

### The `this` keyword: how does it behave?
`this` in JavaScript does __not__ behave the way you would expect.  Its behaviour is very, very different from other languages.

If you are merely looking for a way to encapsulate and scope your code, I suggest avoiding the `this` keyword for now.  But if you truly need to use `this` to write object-oriented JavaScript, here's an explanation...

In more sane languages, `this` gets you a pointer to the current object.  But in JavaScript, it means something quite different: it gets you a pointer to the __calling context__ of the given function.

This will make much more sense with an example:

```js
var myFunction = function()
{
    console.log(this);
};
var someObject = {};                // Create an empty object.  Same as: new Object();
someObject.myFunction = myFunction; // Give someObject a property
someObject.myFunction();            // Logs Object
myFunction();                       // Logs...Window?
```

That's bizarre, isn't it?  Depending on how you call a function, its `this` pointer changes.

In the first example, because `myFunction` was a property of `someObject`, the `this` pointer referred to `someObject`.

But in the second example, because `myFunction` wasn't a property of anything, the `this` pointer defaults to the "global" object, which is `window`.

The important take-away here is that __`this` points to whatever is "left of the dot."__

Note that you can actually override what `this` points to by using the built-in `call()` and `apply()` functions.

As you can imagine, this causes a ton of confusion - particularly for new JavaScript developers.  My recommendation is to avoid writing code in such a way that relies on the intricacies of `this`.

### WTF are `constructor` and `prototype`?

If you're asking this question, it means you're getting knee-deep into JavaScript OOP.  Good for you!

The first thing you need to know is that JavaScript does NOT use classical OOP.  It uses something called prototypal OOP.  This is very, very different.  If you really want to know how JavaScript OOP works, you need to read [Constructors considered mildly confusing, by Joost Diepenmaat][constructors-confusing].  Joost does a better job of explaining it than I ever will.

But for the lazy, I'll summarize: JavaScript does not have any classes.  You don't create a class and spawn new objects off of it like in other languages.  Instead, you create a new object, and set its "prototype" as the old one.

When you refer to an object's property, if that property doesn't exist JavaScript will look at its prototype...and its prototype, and its prototype, all the way up until it hits the `Object` object.

So unlike the class/object model you see in other languages, JavaScript relies on a series of "parent pointers."

`constructor` is a pointer to the function that gets called when you use the `new` keyword.  

This is best explained with an in-depth code example, so read this [constructor vs prototype experiment][constructor-prototype-experiment] if you want to learn specifically how `constructor` and `prototype` play together.

### WTF is `__proto__`, and how is it different from `prototype`?
If you've tried debugging your JavaScript in Chrome Inspector, you may have noticed a `__proto__` property on all your objects.  Try copying and pasting this code Chrome Inspector's console, and dig into the `__proto__` property to see what I mean.

```js
var ParentObject = function()
{
    // If ParentObject had constructor code, it would go here
};

ParentObject.prototype.parentProperty = 'foo';

// Again, note we're not using a classical OOP class/object relationship here.
// In JavaScript, objects just "point" to their parent via prototypes.
var ChildObject = new ParentObject();

ChildObject.childProperty = 'bar';

console.log("ChildObject:");
console.dir(ChildObject);  // Outputs a viewable object

console.log("ParentObject:");
console.dir(ParentObject);
```

For me, that outputs a `ChildObject` that looks like this:

```js
ChildObject:
ParentObject
    childProperty: "bar"
    __proto__: Object
        constructor: function ()
        parentProperty: "foo"
        __proto__: Object
```

And a `ParentObject` that looks like this:
```js
ParentObject:
function () { // If ParentObject had constructor code, it would go here }
arguments: null
caller: null
length: 0
name: ""
prototype: Object
    constructor: function ()
    parentProperty: "foo"
    __proto__: Object
__proto__: function Empty() {}
```

I highly recommend checking this out yourself in Chrome Inspector, as it's a great debugging tool.

Let's look at `ChildObject` first.  You'll notice it contains `childProperty`, which is what we would expect.  It also has a property called `__proto__`, which itself is an object.  Among its contents, it contains `parentProperty`.

Knowing this, you could reasonably assume that `__proto__` is the same as `prototype`.  Unfortunately, that's incorrect.  The difference between `prototype` and `__proto__` is subtle, but it's very important.

If you look at the output for `ParentObject`, you'll see that it contains both `__proto__` AND `prototype`, making the situation even more confusing.  So what's going on?

Well, it turns out that every function gets an automatically-created property called `prototype`.  That's right, __every__ function gets this property, because any function can be used as a constructor.  As we discussed earlier, JavaScript uses prototype properties like this to figure out your object inheritance chain.

When you create a new object using the syntax:

```js
var ChildObject = new ParentObject();
```

JavaScript will look in `ParentObject`, find its `prototype` property, and copy it into `ChildObject`.  But it will __not__ copy it into `ChildObject` as `prototype`...it will copy it in as `__proto__`.

So why not copy it in as `prototype`?  For a variety of reasons, the main one I am aware of being that it is totally possible for an object to have both a `prototype` __and__ a `__proto__`.  In fact, this is quite common, because all functions have a `prototype` and all objects have a `__proto__`.  Heck, take a look at our `ParentObject` up above...it definitely does.

This can lead to an interesting scenario where you create a `ChildObject`, and then change the `prototype` property of the `ParentObject` later.  If you do this, `ChildObject` will _continue to use the old prototype_.

Let's see an example:

```js
var ParentObject = function()
{
    // If ParentObject had constructor code, it would go here
};

ParentObject.prototype.parentProperty = 'foo';

// Again, note we're not using a classical OOP class/object relationship here.
// In JavaScript, objects just "point" to their parent via the prototype
// property.
var ChildObject = new ParentObject();

ChildObject.childProperty = 'bar';

delete ParentObject.prototype;

console.log("ChildObject:");
console.dir(ChildObject);
```

Even though we have removed the `prototype` property from `ParentObject`, outputting `ChildObject` will continue to look like this:

```js
ChildObject:
ParentObject
    childProperty: "bar"
    __proto__: Object
        constructor: function ()
        parentProperty: "foo"
        __proto__: Object
```

That's right, it will _still have its parent pointer_.

So to recap: **`prototype` goes on constructors that create objects, and `__proto__` goes on objects being created.**

One more thing: for the love of beer and pork tacos, please don't ever try to manipulate the `__proto__` pointer.  JavaScript is not supposed to support editing of `__proto__` as it is an internal property.  Some browsers will let you do it, but it's a bad idea to rely on this functionality.  If you need to manipulate the prototype chain you're better off using `hasOwnProperty()` instead.

### WTF is a closure?
Closures are a concept that appear in functional languages like JavaScript, but they have started to trickle their way into other languages like PHP and C#.  For those unfamiliar, "closure" is a language feature that ensures variables never get destroyed if they are still required.

This explanation is not particularly meaningful in and of itself, so here's an example:

```js
// When someone clicks a button, show a message.
var setup = function()
{
    var clickMessage = "Hi there!";
    $('button').click
    (
        function()
        {
            window.alert(clickMessage);
        }
    );
};
setup();
```

In a language without closures, you'd likely get an error when that click handler fires because `clickMessage` will be undefined.  It'll have fallen out of scope long ago.

But in a language with closures (like JavaScript), the engine _knows_ that `clickMessage` is still required, so it keeps it around.  When a user clicks a button, they'll see, "Hi there!"

As you can imagine, closures are particularly useful when dealing with event handling, because an event handler often gets executed long after the calling function falls out of scope.

Because of closures, we can go one step further and do something cool like this!

```js
(function()
{
    var clickMessage = "Hi there!";
    $('button').click
    (
        function()
        {
            window.alert(clickMessage);
        }
    );
})();
```

In the above example, we don't even need to give the function a name!  Instead, we execute it once with the () at the end, and forget about it.  Nobody can ever reference the function again, but _it still exists_.  And if someone clicks that button, it will still work!

### Why is parseInt() mucking up when I get into big numbers?
Despite looking like it, JavaScript doesn't actually have an integer data type - it only has a floating point type.  This isn't an issue when you do:

```js
parseInt("1000000000000000", 10) < parseInt("1000000000000001", 10); //true
```

but add one more zero:

```js
parseInt("10000000000000000", 10) < parseInt("10000000000000001", 10); //false
```

And you'll see where the difference between integers and floating points manifests.

### Why does JavaScript have so many different ways to do the same thing?
So you can choose which way is best for you. Some parts of JavaScript are not designed that well. Your best guide to muddle through it is to read [JavaScript: The Good Parts, by Douglas Crockford][good-parts].  He clearly outlines which pieces of the language you should ignore.

<!--
### Truthiness/Falsiness
### What is the difference between null, undefined and 'undefined'?
### How do I create a class?  How do I create public/private members?
### How do I do inheritance? Overwriting automatically-created prototype object (should provide an experiement)
### What are the differences between ready(), load(), and inline JavaScript?
### What are the differences between bind(), delegate(), live(), and on()?
### My function is always returning null, even though I'm providing a return value.  Why?
### Pushing console.log() into production
### Why is my JavaScript not executing (because an error was thrown earlier)
### I'm trying to select a DOM element and it's coming back as undefined, even though I know it's there.  Why?
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
