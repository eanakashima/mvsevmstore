---
layout: post
title: "Sunset Testing for Short-Lived Code"
date: 2014-07-25 16:40
comments: true
categories: 
---

*Rachel Myers and I talked about Sunset Testing in our talks at O’Reilly’s
Fluent Conf 2014 and Front-End Ops Conf, and a number of people asked us if we
had a blog post on the topic that they could share with their coworkers. We
didn’t, so here it is, somewhat belatedly.*

When I write some javascript to work around a browser compatibility bug or some
custom ruby to show different configurations for an A/B test, I always have to
get past a bad feeling before checking it in. It helps to write a comment,
something like “Workaround for an IE8 bug,” or “Delete this when we finish the
navigation A/B test,” so it’s obvious when we can remove the code, but it
always feels like a bit of a defeat just to have this code that we know is
short-lived alongside our business logic that’s core to the domain.

Of course, good code organization practices help — browser compatibility code
can be moved into a “polyfills” directory and a good A/B framework can abstract
most of the hurt out of your application — but there’s always a few files
hanging around that you just *know* you’ll have to go back to in a few months.
For those files, Sunset Tests might be the answer.

## Sunset Tests

Sunset Tests are tests that remind us to delete or at least revisit temporary
code at some point in the future. I first got the idea from my long-time coworker
[Eric Saxby](http://github.com/sax). We were pairing on a feature A/B test, and as the finishing
touch, he added a test that would fail in a month or so and basically remind us,
“Hey future us, remember that you hate all this A/B test code and want to delete
it now.”  Turns out, there are more places this is useful than just A/B tests.
My coworker/cospeaker [Rachel Myers](http://github.com/rachelmyers) coined the term “Sunset Test” and I like it
because it reminds me of all the time I’ll be able to spend on my beach vacation
while everyone without sunset tests is busy wading through old
weird `.js.erb` files trying to figure out what they do. Err, I mean, I like the
tests because they remind you of when you can sunset your short-lived code.

``` js

// A/B test testing

test( “homepage banner a/b test is active“, function() {
  var today     = new Date();
  var testEndDate = Date.parse(“2014-04-19”);

  if (today < testEndDate) {
    ok(activeABTests.homepage);
  } else {
    ok(!activeABTests.homepage);
  }
});

```

My favorite use case for Sunset Tests is as a reminder for when we can pull out
browser compatibility code we don’t need anymore. I’ve worked on a number of
*very* large Ruby on Rails apps, and run into a lot of random shims & browser
hacks that *seem* like they might be dead but don’t definitively express their
ability to be deleted. Here’s a good example: this `Array.prototype.indexOf`
polyfill file. Unless you remember a lot about the dawn of ECMA-262, you might
not know what this is for or whether or not you can take it out. Even if you
*do* know what it does, do you remember *which* version of IE added native
support for `[‘a’,’b’].indexOf(‘b’)`? I don’t. And I added this polyfill to the
codebase.

```js
if (!Array.prototype.indexOf) {
  Array.prototype.indexOf = function (searchElement, fromIndex) {

    var k;

    // 1. Let O be the result of calling ToObject passing
    //    the this value as the argument.
    if (this == null) {
      throw new TypeError('"this" is null or not defined');
    }
    //... continues for 150 lines
```
So we can test it like so:

```js
test( “indexOf polyfill expired?", function() {
  var browserSupport = window.polyfill.browserSupport;
  var today     = new Date();
  var iE8Sunset = Date.parse(browserSupport[‘ie-8’]);

  if (today < iE8Sunset) {
    ok(polyfilled.indexOf);
  } else {
    ok(!polyfilled.indexOf);
  }
});
```

This is best coupled with a config file that has all the important future dates
you care about. Even if you don’t know the exact date in the future when you’ll
drop support for, say, IE10, it’s worth setting up one of these. Add every
browser version (or every reason for your temporary code), and the date when
you’ll drop support, or the next date you want to think about the problem, if
you don’t know when you’ll drop support. Six months is usually a decently good
runway.

```javascript
// check our browser analytics on these dates

browserSupport = {
  “safari-5.0": "June 01 2014",
  “ie-8":       "June 01 2014",
  “ie-9":       "January 01 2015"
}
```

Now the cool part: six months from now, you’ll see this test failure, you’ll
have a reminder of what that code is, and you’ll be able to check with your
team and see if you’re ready to remove it.

Voila. One step closer to code that deletes itself.

