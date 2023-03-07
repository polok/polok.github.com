---
layout: post
title: "StoreKit: `mocking` SKProduct, SKProductDiscount, SKProductSubscriptionPeriod"
description: "How I can mock an SKProduct, SKProductDiscount or/and SKProductSubscriptionPeriod classes in Swift?"
tag:
  - swift
  - testing
  - storekit
---

How I can mock an `SKProduct`, `SKProductDiscount`, `SKProductSubscriptionPeriod` and other classes realted to [StoreKit](https://developer.apple.com/documentation/storekit) in Swift?

The solution is to extend the [SKProduct](https://developer.apple.com/documentation/storekit/skproduct) class and set the fields which you need to:

```swift
import StoreKit

public extension SKProduct {

    convenience init(
        identifier: String,
        price: NSDecimalNumber,
        priceLocale: Locale) {
        self.init()
        self.setValue(identifier, forKey: "productIdentifier")
        self.setValue(price, forKey: "price")
        self.setValue(priceLocale, forKey: "priceLocale")
    }
}
```

The same with [SKProductDiscount](https://developer.apple.com/documentation/storekit/skproductdiscount) class:

```swift
import StoreKit

public extension SKProductDiscount {

    convenience init(
        price: String,
        priceLocale: Locale,
        subscriptionPeriod: SKProductSubscriptionPeriod,
        numberOfPeriods: Int) {
        self.init()
        self.setValue(NSDecimalNumber(string: price), forKey: "price")
        self.setValue(priceLocale, forKey: "priceLocale")
        self.setValue(subscriptionPeriod, forKey: "subscriptionPeriod")
        self.setValue(numberOfPeriods, forKey: "numberOfPeriods")
    }
}
```

And [SKProductSubscriptionPeriod](https://developer.apple.com/documentation/storekit/skproductsubscriptionperiod) extension:

```swift
public extension SKProductSubscriptionPeriod {

    convenience init(
        numberOfUnits: Int,
        unit: SKProduct.PeriodUnit) {
        self.init()
        self.setValue(numberOfUnits, forKey: "numberOfUnits")
        self.setValue(unit.rawValue, forKey: "unit")
    }
}
```

In the above extension, there is a tricky part for the key “unit”. We don’t set the value as unit but we are passing a raw value of it. I was struggling with [this](https://forums.swift.org/t/crash-when-setting-enum-from-swift-to-obj-c-via-setvalue/50665) and this post helped me.
