---
layout: post
title: "Auto Layout: center a view in its container"
description: "How to center a view in its container?"
tag:
  - swift
  - auto layout
---

There are two ways to center one UIView inside another, depending on whether you use Auto Layout or not.

If you don’t use Auto Layout, it’s only one line of code:

```swift
childView.center = parentView.center
```

Note, that sets the position once, so you need to update when for example user rotates their device.

If you’re using Auto Layout, you can center your child view inside its parent like this:

```swift
childView.centerXAnchor.constraint(equalTo: parentView.centerXAnchor).isActive = true
childView.centerYAnchor.constraint(equalTo: parentView.centerYAnchor).isActive = true
```

Here, our constraints will automatically update as the available space changes ☺️.
