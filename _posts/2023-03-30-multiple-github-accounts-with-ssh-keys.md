---
layout: post
title: "Using multiple Github accounts with ssh keys"
description: "Having more then one Github account on the same computer"
tag:
  - ssh
  - github
---

Recently, I had a need to have two Github accounts on the same computer and I wanted to use both of them without typing password everytime, when doing git push or pull.
Let's say that I have two accounts "superman" and "batman". A solution is to use ssh keys and define host aliases in ssh config file (each alias for an account).

## Solution

1.Edit or create ssh config file (~/.ssh/config):

```bash
# Default github account: superman
Host github.com
   HostName github.com
   IdentityFile ~/.ssh/superman_private_key
   IdentitiesOnly yes

# Other github account: batman
Host github-batman
   HostName github.com
   IdentityFile ~/.ssh/batman_private_key
   IdentitiesOnly yes
```

2.Add ssh private keys to your agent [link](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

```bash
$ ssh-add ~/.ssh/superman_private_key
$ ssh-add ~/.ssh/batman_private_key
```

3.Test our connection

```bash
$ ssh -T git@github.com
```

```bash
$ ssh -T git@github.com
Warning: Permanently added the ECDSA host key for IP address '140.82.121.3' to the list of known hosts.
Hi superman! You've successfully authenticated, but GitHub does not provide shell access.
```

```bash
$ ssh -T git@github-batman
```

```bash
Warning: Permanently added the ECDSA host key for IP address '140.82.121.3' to the list of known hosts.
Hi batman! You've successfully authenticated, but GitHub does not provide shell access.
```

4.Cloning repository

Using superman account:

```bash
$ git clone git@github.com:superman-user/project.git /path/to/project
```

Using batman account:

```bash
$ git clone git@github-batman:batman-user/project-other.git /path/to/project-other
```

5.Using proper user and email

Assumption that for default account we are using global git configuration, so for not default Github account go to cloned repository and do the following:

```bash
$ cd /path/to/project-other
$ git config user.email "batman-user@mail.com"
$ git config user.name  "Batman"
```
