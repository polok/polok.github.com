---
layout: post
title: "Decodable: decode JSON with dynamic keys"
description: "JSON structure with dynamic keys mapping using Decodable"
tag:
  - swift
  - networking
---

What do I mean by JSON with dynamic keys? Take a look at the following JSON example that shows a dictionary of currencies where currency code is a dynamic key and currency name is a value of it. 

![](/images/2023-01-10-Decodable-decode-JSON-with-dynamic-keys/json_dynamic_keys.png)

JSON comes from free open API which can be found [here](https://github.com/fawazahmed0/currency-api).

In this post, I will walk through the decoding approach using the `Decodable` protocol. The effect which I want to achieve is a dictionary of the currencies like `currencies: [String: String]`.

First, let’s define a `CurrenciesResponse` struct that conforms to the `Decodable` protocol and will hold our dictionary of currencies.

![](/images/2023-01-10-Decodable-decode-JSON-with-dynamic-keys/dynamic_keys_decodable_model_1.png)

Important note which is a different thing which we suppose to do when we don’t have dynamic keys. In order to access the JSON’s dynamic keys, we must define a custom `CodingKey` struct. This custom `CodingKey` struct is needed as we want to create a decoding container using the `JSONDecoder`.

![](/images/2023-01-10-Decodable-decode-JSON-with-dynamic-keys/dynamic_keys_decodable_model_2.png)

Note that we are only interested in the string value initialiser because our keys (currency codes) are of type string, therefore we can just return nil in the integer value initialiser. 

Now, we can start decoding our data.

![](/images/2023-01-10-Decodable-decode-JSON-with-dynamic-keys/dynamic_keys_decodable_model_3.png)

Let’s break down in detail what’s happening inside the initializer.

1. Create a decoding container using the `DynamicCodingKey` struct. This container will contain all the dynamic keys (currency codes).

2. Loop through each key (currency code) to decode its respective String object (currency name).

3. Store all the decoded objects into the currencies dictionary.


Done, the final file looks like this

![](/images/2023-01-10-Decodable-decode-JSON-with-dynamic-keys/dynamic_keys_decodable_model_4.png)