---
layout: post
title: "3rd Party Javascript Checklist"
date: 2014-02-01 16:51
comments: true
categories: 
---

I’ve been enjoying Ben Vinegar & Anton Kovalyov’s [Third-Party Javascript](http://thirdpartyjs.com/)
and would recommend it to anyone who is looking to build their front-end
expertise, not just to those working on third-party scripts.

One thing the book doesn’t provide, but that I found myself wanting, was a short
checklist to run through while evaluating vendors or scripts. So here’s my stab
at that, with some of my own concerns mixed in with ideas from the book. The
book has thorough, detailed info on many of these topics, particularly security,
async loading, and deployment, so if any of these leave you with questions,
check it out.


## Third Party Javascript Checklist

1. Will it be served minified & compressed? Will there also be an expanded,
readable source for developers to reference?

2. Is an https version available? Will all related resources be served over
https?

3. Is it deployed in a performant way?
    * Is it gzipped?
    * Does it have HTTP cache headers, set as long as sensible?
    * Is it served from a CDN?

4. Does it work async? Can it be loaded both with a standard script tag and with
a script loader? Many tag-management solutions force async loading of
third-party scripts. Relying on document.write or expecting dependencies to load
in a particular order may make a script unusable (or unpalatable) for some site
owners.

5. Does it add only one variable to the global scope?
    * Bonus: if the whole script can run inside an anonymous function, it may
    add zero variables to the global scope.
    * For widgets that have a visual component: does it namespace all styles
    under a widget-specific ID?

6. If it requires a library or framework, does it load the library namespaced
inside itself, and with no-conflict mode enabled, if applicable? Or, if it
depends on the host site to provide dependencies, is there clear documentation
of what they are and which versions are supported?

7. Is it as secure as it can be? Third-party scripts can be implicated in most
of the same client-side security issues as first-party scripts. There’s a
thousand different dimensions to this one, but it’s worth taking one last chance
to look out for common problems, like eval-ing user or publisher input, adding
sensitive data to cookies or local storage, and sending user data in the clear.

8. How is the script's performance monitored? Is there monitoring for errors?

9. Is there enough documentation? Adequate documentation should show how to
include the script and clearly describe what it will do. Ideally, the
documentation will show how to add the script both as a one-liner (script tag)
and with an async loader. The documentation should also state what browsers and
versions are supported. Does it work on a phone? Does it work in IE? Does it
work in IE *6*?

Listed out, these requirements seem fairly basic, but it’s the rare third party
that provides them all. While developers are often roped into working with
third-party scripts that are less than ideal, it’s a good idea to at least raise
the issues with the provider and your own company. Many of these items don’t
take much to accommodate — customers and users just don’t ask for them enough.
