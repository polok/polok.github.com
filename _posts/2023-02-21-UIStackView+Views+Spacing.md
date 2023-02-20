---
layout: post
title: "UIStackView: arranged subviews spacing"
description: "How to add a custom spacing between UIStackView views?"
tag:
  - swift,
  - UIKit
  - UIStackView
---

# UIStackView: custom spacing between arranged subviews

[UIStackView](https://developer.apple.com/documentation/uikit/uistackview) has an overall `spacing` property that affects spacing between all of its arranged subviews which can be set like this:

```swift
let stackView = UIStackView()
stackView.spacing = 10.0
```

However, what in case when we would like to have a different spacing between our subviews? To make that happen, we use the `setCustomSpacing()` method of our stack view.

```swift
func setCustomSpacing(_ spacing: CGFloat, after arrangedSubview: UIView)
```

It takes the number of points of spacing we want and the view that should precede the spacing.

Below example, creates a stack view with a button and then requests 5 points of spacing after it:

````
let stackView = UIStackView()
let button = UIButton()

stackView.addArrangedSubview(button)
stackView.setCustomSpacing(5.0, after: button)

#### Standard and Default Spacing:
There are two new type properties on the UIStackView class in iOS 11 that define default and system spacing. We can use these twp to set or reset the custom spacing after a view.

```swift
class let spacingUseDefault: CGFloat
class let spacingUseSystem: CGFloat
````

### Notes:

- You always specify the spacing after the arranged subview. There is no method to specify the spacing before a view
- There is no way to specify this custom spacing in Interface Builder. We need to do it in the code.
- When we remove the subview from the stack view the system removes the custom spacing as well.
