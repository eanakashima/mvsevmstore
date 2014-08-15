---
layout: post
title: "What to Say About JavaScript"
date: 2014-07-25 17:18
comments: true
categories: 
---

When I decide whether or not to use the latest javascript tool/library/framework,
I first look at what it *does* — what the features are. But after that, there are
a number of other factors that influence my decision. I never like hearing people
boil these things down to "Ember sucks" or "Underscore is useless," so I've tried
to come up with a more precise way to describe what I like and don't. I talk
about what the thing provides on three different axes: convention, abstraction,
and sugar.

- **Convention**: does it provide and enforce conventions for writing
and organizing my code? Are these conventions clear, easy to pick up, and easy
to work with?

- **Abstraction**: does it isolate me from details and platform
quirks I don't care about? Is working with the abstraction layer usually more
expressive and convenient than the alternative? Does it allow you to work at a
more consistent level of abstraction?

- **Sugar**:  does it make it fast and pretty for me to write the code
I need to write? Does it offer helpers that make common problems less irritating?

## So, Coffeescript.

Coffeescript is probably the best example of a tool I wish I'd applied these
axes to. I had a bad introduction to coffeescript, watching rubyists try to make
it work like ruby on an older application where only a suite of flaky Jasmine
tests was in coffeescript and trying to find the source line-number of a
failing test was a perennial nuisance. But I think the bigger problem was that I
had the wrong expectations for how coffeescript was going to help me. If I'd
just asked about these three different aspects, I would have figured it out a
lot sooner.

### Convention

While coffeescript keeps most of the flexibility of javascript and doesn't
provide a large number of new conventions, the ones it does encourage are
essentials that go a long way toward keeping sanity in a js-heavy application.

#### Discourage globals

First, coffeescript discourages the use of globals by wrapping every file in an
immediately-executing function. While nothing prevents you from adding variables
to the global scope, the small barrier of needing to explicitly make a variable
global is enough to encourage me to find better ways to share state between features.

#### Encourage a single way to do "classes"

It's always been possible to simulate classes and class-based inheritance in
javascript, but the sheer number of different ways to do it makes it daunting,
especially on large teams or in big applications. I've seen a lot of projects
skip "classes" entirely rather than incur the cost of choosing and evangelizing
one right way to do them. Coffeescript gives you one simple way to do classes,
and they work in approximately the way people expect them to.

### Abstraction

Abstraction is the axis where most of my coffeescript confusion originated.
Visually, coffeescript looks a bit like python or ruby, and I think this lead me
(and many developers I've worked with) to assume that coffeescript would
abstract away a lot of the quirks of javascript and let you write in some sort
of dynamically-typed esperanto. But coffeescript isn't heavy-handed enough to pull
this off (and now I agree that it's probably a good thing).

I still think in javascript regularly when I write coffeescript. One
example: this common case where you decide whether to use the `?` operator based
on whether you want to ask if a value is falsy or whether it's null-like.

```coffeescript
  # won't execute if length is 0
  doStuff() if myElement.length

  # will execute if length is 0
  doStuff() if myElement.length?
```

I'm glad to have the flexibility to do both, but coffeescript doesn't change the
fact that I need to understand the difference.

### Sugar

This is the other place where coffeescript shines, of course. I think of
javascript as relatively simple, so I didn't realize how much I'd wanted a sugar
layer on top of it, but now that I have it, I'm pleased about all those minutes
I save not typing `var` and `function`.

The loop comprehension feature, while easy to abuse, is a particularly pleasing
example:

```js
// javascript
var ids = ["top", "nav", "sidebar", "main"]

for (var i = 0, l = foods.length; i < l; i++) {
  id = ids[i];
  if (id !== "main") {
    print(id);
  }
}
```

becomes

```coffeescript
# coffeescript
ids = ["top", "nav", "sidebar", "main"]

print id for id in ids when id isnt "main"
```

## Other javascript things

With these axes, I find that it's a lot easier to say what I like and don't like
about other javascript tools & libraries. Underscore is great because it
provides sugary utility functions and nothing else. jQuery is an amazing
abstraction layer on top of different DOM implementations, but it doesn't provide
much in the way of conventions. And people get frustrated because they work
with jQuery in projects where *nothing* is providing a convention layer. Which,
incidentally, is an easier problem to talk about than the problem where jQuery
just "sucks."
