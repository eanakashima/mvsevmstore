---
layout: post
title: "Just Enough Web Perf"
date: 2013-11-03 15:52
comments: true
categories: 
---

# How to get started with web performance optimization in five steps

At ModCloth, I was lucky enough to have dedicated time to focus on web performance optimization. I spent a full month totally focused on web perf and a few months after that with a dedicated percentage of my queue set aside for perf work. With this amount of time, my team was able to clean up thousands of lines of legacy javascript & css, update backend settings, and shave more than 500 ms off median load times for top pages.

Since then, I’ve been spread a bit more thin, and I’ve been a bit intimidated by the idea of trying to work web perf into the queue one story at a time. Performance optimization can seem like a rabbit-hole you can fall down and never get back out of — like work that's never done. However, I know from my past experience that it’s possible to bite off a week of web perf work, make an impact, and get back to other business, so I thought I’d sketch out some notes on how to do that.

Here’s a rough outline for how to do a meaningful web performance optimization in five steps. The goal of this is "minimum viable web perf” — just enough optimization to create a user-perceived performance improvement on your site and get everyone on your team thinking about performance.

# Part 1: Collect metrics!

If you don't currently collect performance metrics, stop everything and start here. I (and most of the engineers I've worked with) always want to jump right in and start optimizing, which is a nice impulse but a horrible idea. For one, without a good way to track and quantify performance, you can’t really have goals or confirm that performance is improving. For two, while engineers always have a gut feeling for what could be faster (and qualitatively, they’re usually right), those gut instincts (mine included) rarely point to the highest-impact optimizations first.

Metrics have other jobs too, once work gets started — they’ll help keep everyone motivated and also help identify when it makes sense to *stop* optimizing. Most projects hit a tipping point after a few weeks or months where they've burned through all the highest-impact optimizations, and the same amount of work starts to yield fewer returns. At that point, particularly on a small team or in a young company, it may make sense to switch to other work.

## Choosing Tools

While you can nerd out over metrics forever, I’ve found that the only absolute must-haves are two kinds of data: high-level data collected from real users (RUM) and detailed data that will show *how* components load. (This second type of data is typically 'synthetic:' collected on your local pc or with tests you run on a specific server.) The real user data will help you verify that you're actually getting faster, and the detailed data will help you identify what points are slow so you know where to spend your time optimizing.

For RUM data, some top choices:

* **Soasta mPulse** (formerly LogNormal): awesome client-side real-user metrics with histograms, browser stats, and the ability to tag pages into groups.
* **New Relic:** great for Rails apps; provides both front-end and back-end metrics.
* **Google Analytics:** collects client-side real-user perf data by default. The charts aren’t the most beautiful, but unless your site has massive traffic, it’s FREEEEE.

For synthetic data, **Web Page Test** is a great free tool that will give you an incredibly detailed breakdown of your front-end loading and rendering. Visit webpagetest.org, run a few tests — from various locations and browsers — on your most popular pages and save the results so you have a record of where you started. You can always screen-shot or save the page, but WebPageTest will also let you export results as a HTTP Archive or .har file, a data format that will let you look at loading in detail later.

### First Intermission:

At this point, it’s good to take a moment and decide if a given site or app is truly slow enough to warrant an optimization. Web Page Test is great for this — you can run tests from a handful of geographic locations on major pages, *and* you can do this same testing on your competitors. Depending on the nature of your application (and possibly your competition), you may be able to skip optimizing for now.

## Part 2: Find high-impact optimizations

It’s easy for teams to fall into the habit of doing cargo cult performance optimizations. For this reason, it’s essential to do some audits and write up a prioritized list. This exercise was the only thing that prevented my team from spending two weeks to enable full-page caching for a popular page of the site — an optimization that sounded like a great idea, but wasn’t needed for scalability and would have shaved only a few milliseconds off response times.

Start by auditing the 2-3 most popular pages of the site to see how the pages perform:

1. Run YSlow and PageSpeed Insights. These tools audit pages for performance best practices and spit out a list of recommendations, grouped by priority.

2. Take a look at your time-to-first-byte metrics on top pages. For most sites, performance is 80% front-end and 20% back-end, but If it’s taking two seconds to get the first byte of content to your users, you may need to start with back-end optimizations first.

3. Find slow or blocking resources. Web Page Test will provide you with a waterfall diagram that will let you see what's taking a long time, what scripts consume lots of CPU & memory while they run, and what's blocking other resources from downloading. Take note of these and add them to your list for Part Four.


# Part 3: Flip all the switches

YSlow and PageSpeed will probably identify some optimizations that are essentially configuration changes. It usually makes sense to start with these, since they'll provide the most bang for the buck. Changes like gzipping CSS & JS files, adding or increasing file caching headers, or adding domain sharding for images can be done once & kept in place with minimal maintenance, and they can result in noticeable performance improvements.

## Second Intermission:

If you’ve identified a ton of configuration changes and slow points, and you are wondering how to fit them all in, it may be time to look into tools like mod_pagespeed, a web server module that will sit in front of your app and apply a lot of performance best practices for you (e.g. minifying and compressing text files and images). A team working full-time on web performance can probably do a better job than mod_pagespeed, but if you have a relatively simple app and don't have a lot of resources to dedicated to web perf, it's a great way to cover the bases.

If setting up mod_pagespeed seems intimidating, some CDNs and hosting providers (e.g. Cloudflare) can offer you a hosted solution that will provide similar features.

# Part 4: Untangle something.

Dive in and untangle at least one slow or blocking resource you found in step #2.

Chances are, you found at least one thing in Step #2 that was slowing down your page loads. Maybe all your javascripts load synchronously in the head of the page, and you want to move them down below your footer. Maybe you have a bunch of scripting that executes on DOM-ready, eating up memory and CPU. Maybe you have a zillion unused CSS selectors. Maybe you have some major application performance issues to untangle, like slow queries or n+1 queries. Grab whichever one of these is having the greatest impact on your users and untangle it.

If you can, deploy the fix by itself so you can try to measure the impact it has on performance. Lather, rinse, repeat.

# Part 5. Share!

It’s easy to get stuck in Part 4 forever, but there’s an equally important step that you can’t skip: telling your team about the work you did, why it matters, and what they can do to help. Share load time numbers, compare yourself to competitors, and tell everyone about the work you did and what you want to do next. (Extra credit: use Charles or a similar tool to show everyone what your site looks like loading in Internet Explorer 8 over a low-end DSL connection, *then* share all your numbers.)

Ultimately, everyone working on the site or app has an impact on performance, whether it's content owners adding images, marketers adding third-party web tools, or product managers planning new features. If you don't share your work with all of these people, they'll start to feel like adversaries who are slowing down the site with all of their tracking tags and photo carousels and social sharing widgets. But ultimately, everyone in your company can understand the benefits of a fast site — sharing web performance work with them goes a long way toward helping to keep them as partners.
