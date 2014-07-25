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
to come up with a more precise way to describe what I like and don't, by
talking about what a thing provides on three different axes: convention, abstraction,
and sugar.

- **Convention**: to what degree does it provide and enforce conventions for writing
and organizing my code? Are these conventions clear, easy to pick up, and easy
to work with?

- **Abstraction**: to what degree does it isolate me from details and platform
quirks I don't care about? Is working with the abstraction layer usually more
expressive and convenient than the alternative? Does it allow you to work at a
more consistent level of abstraction?

- **Sugar**: to what degree does it make it fast and pretty for me to write the code
I need to write? Does it offer helpers that make common problems less irritating?

## Case: Coffeescript

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

Coffeescript discourages the use of globals by wrapping every file in an
immediately-executing function. While nothing prevents you from adding variables
to the global scope in coffeescript, the small barrier of needing to explicity
make a variable global is enough to encourage me to find better ways to share
state and communicate between features.

#### Encourages a single way to do "classes"

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
of dynamicly-typed esperanto. But coffeescript isn't heavy-handed enough to pull
this off (and now I agree that it's probably a good thing).

I find that I still have to think in javascript regularly in coffeescript. One
example: this common case where you decide whether to use the `?` operator based
on whether you want to ask if a value is falsy or whether it's null-like.

```js
  # won't execute if length is 0
  doStuff() if myElement.length

  # will execute if length is 0
  doStuff() if myElement.length?
```

I'm glad to have the flexibility to do both, but

Sugar

Better loops

Case: jQuery
