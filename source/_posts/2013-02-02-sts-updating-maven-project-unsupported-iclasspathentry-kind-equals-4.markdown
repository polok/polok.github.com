---
layout: post
title: "STS: Updating Maven Project. Unsupported IClasspathEntry kind=4"
date: 2013-02-02 18:02
comments: false
categories: [Maven, STS]
---

It is my first post and I treated it as warm-up. It is not very long but I wanted to finally start blogging ;).

During updating maven projects (on which I or someone has invoked command mvn eclipse:eclipse) in my STS I usually get error from the post title.

For me, belows steps help:

<li>In STS/Eclipse disable the maven nature for your project (via the right-click menu)</li>

<li>Run mvn eclipse:clean (while your project is open in STS/eclipse) from command line</li>

<li>Re-enable the maven nature</li>

Done.