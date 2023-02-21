---
layout: post
title: "macOS + MongoDB"
description: "How to install MongoDB on your Mac"
tag:
  - database
  - mongodb
---

# Installing MongoDB on your Mac

There are a few steps:

- First, you install Homebrew. It's not a mandatory thing but it just simplifies the process. If you are not familiar with Homebrew, check [this](https://brew.sh) out.

- Next, find the MongoDB tap.

`brew tap mongodb/brew`

- Now, install MongoDB.

`brew install mongodb-community`

Now, you are ready to go to install MongoDB locally. Btw. When writing this post I was using macOS Catalina but had to update to macOS Big Sur as some of the dependencies couldn't be installed on a lower version of the macOS.

# Preparations for macOS Catalina onwards

Apple created a new [Volume](https://support.apple.com/en-us/HT210650) in Catalina for security purposes. If you’re on Catalina, you need to create the `/data/db` folder in `System/Volumes/Data`.

Go and use this command:

`sudo mkdir -p /System/Volumes/Data/data/db`

Then, you have to add the permissions to it so use below command for that:

`sudo chown -R `id -un` /System/Volumes/Data/data/db`

# Using MongoDB

In the past, you could run the mongod command to start MongoDB. This no longer works out for the box from MongoDB v4.2.3 onwards.

## Starting MongoDB

Use this command:

`brew services run mongodb-community`

You can use `start` instead of `run`. However `start` will start MongoDB automatically when you login into your machine.

## Checking if MongoDB is running

Use this command:

`brew services list | grep mongo`

If MongoDB is running, `mongodb-community` will have a `status` set to `started`.
Eg.

```bash
mongodb-community started              /usr/local/opt/mongodb-community/homebrew.mxcl.mongodb-community.plist
```

## MongoDB Shell

Use this command:

`mongo`

## Stopping MongoDB

Use this command:

`brew services stop mongodb-community`

# Aliases to make these easier

You can specify your own aliases in `.zshrc` file if you don't want to type rew services run mongodb-community every time I want to start MongoDB. More about the aliasses you can find [here](https://haker.dev/2023/01/04/iTerm2+Oh-My-Zsh.html)

I created some aliases to make things easier for me. Here are my aliases:

```bash
alias mongod-run='brew services run mongodb-community'
alias mongod-status='brew services list | grep mongo'
alias mongod-stop='brew services stop mongodb-community'
```
