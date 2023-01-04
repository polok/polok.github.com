---
layout: post
title: "iTerm2 + Oh My Zsh"
description: "No doubt, there is not a more productive tool for us developers to use!"
tag:
  - dev-tools
---
No doubt, there is not a more productive tool for us developers (actually, even not only for us  ) to use – the terminal is the swiss-army knife of the developers' tool kit and customizing it is a very personal thing.

In this post I will go through the key oh-my-zsh plugins I use to boost my productivity and transform the command line into a productive environment.

For me, the attributes which improve the productivity
* a prompt displaying key information (e.g. git information)
* syntax highlighting
* efficient directory and file system navigation
* auto-completion

## Prerequisites

I am using MacOS Ventura with [iTerm2](https://iterm2.com/) and [Homebrew](https://brew.sh/).

## Setup the Prompt

### Oh My ZSH

_Oh My Zsh is a delightful, open source, community-driven framework for managing your Zsh configuration. It comes bundled with thousands of helpful functions, helpers, plugins, themes, and a few things that make you shout..._

_"Oh My ZSH!"_

The instructions how to install are [here](https://ohmyz.sh/#install)

Installing `oh-my-zsh` will create a `~/.zshrc` file under your home directory. I will be editing the ~/.zshrc file throughout this post to customize our terminal.

### Customise the prompt

`robbyrussell` is a default theme for oh-my-zsh - named after the creator of oh-my-zsh. On the day of writing this post I use `eastwood` one. I really like this theme’s minimalistic aesthetic.

 [Here](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes) is the list of other themes which come with oh-my-zsh. Note, that there are external themes that you can download and use. For example, `dracula` which I was using for some time. You can find it [here](https://draculatheme.com/zsh).

In order to change the theme to one of the inbuilt themes, you just need to update the `ZSH_THEME` value in your `~/.zshrc` and restart your terminal for the changes to take effect. 

`ZSH_THEME="eastwood"`

Easy, right?

### Choose a color scheme and font

If the default color scheme and font are fine with you, just skip this part. This is completely down to personal preference. When it comes to font, my choice is `Monaco`.

![iterm2-font-screenshot](/images/2023-01-04-iTerm2+Oh-My-Zsh/iterm2_font.png)

And `Solarized Dark` for the color scheme.

![iterm2-color-scheme-screenshot](/images/2023-01-04-iTerm2+Oh-My-Zsh/iterm2_colour_scheme.png)

The final prompt uses `eastwood` and `Solarized Dark` with `Monaco` font.

![iterm2-eastwood-final-screenshot](/images/2023-01-04-iTerm2+Oh-My-Zsh/iterm2_eastwood_final_effect.png)

## oh-my-zsh plugins

* [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)

_This package provides syntax highlighting for the shell zsh. It enables highlighting of commands whilst they are typed at a zsh prompt into an interactive terminal. This helps in reviewing commands before running them, particularly in catching syntax errors._

Instructions on how to install with oh-my-zsh are [here](https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md#oh-my-zsh). Run `source ~/.zshrc` or restart your terminal for the changes to take effect.

The effect:

![plugins-highlighting-screenshot](/images/2023-01-04-iTerm2+Oh-My-Zsh/zsh-syntax-highlighting.gif)

Note, that there are some tweaks which can be applied, check [this](https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/docs/highlighters.md) out.

* [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)

_It suggests commands as you type based on history and completions._

Instructions how to install with oh-my-zsh are [here](https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md#oh-my-zsh).

## Useful plugins and command line tools

* [exa](https://the.exa.website/) – ls on steroids

_**exa** is an improved file lister with more features and better defaults. It uses colours to distinguish file types and metadata. It knows about symlinks, extended attributes, and Git. And it’s small, fast, and just one single binary._

![iterm2-tools-exa-screenshot](/images/2023-01-04-iTerm2+Oh-My-Zsh/exa.gif)

## Aliases

You can specify your own aliases in `.zshrc` file. They can be a big time saver for long commands that you use frequently.

## Resources

A full list of `oh-my-zsh` plugins is available [here](https://github.com/ohmyzsh/ohmyzsh/wiki/Plugins)

Full `.zshrc` file created above

[Gist](https://gist.github.com/polok/b02dd260f58a05eaef5ce1f3ab06194a)

![zshrc-file-final-screenshot](/images/2023-01-04-iTerm2+Oh-My-Zsh/zshrc_default.png)
