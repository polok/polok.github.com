---
layout: post
title: "Remove a git's submodule from our repository"
description: "Remove git's submodule in a clean way"
tag:
  - git
---

Recently I had to remove a submodule from our project which was achieved in easy way:

```bash
# Remove the submodule entry from .git/config
git submodule deinit -f SUBMODULE_DIRECTORY
```

```bash
# Remove the submodule directory from the superproject's .git/modules directory
rm -rf .git/modules/SUBMODULE_DIRECTORY
```

```bash
# Remove the entry in .gitmodules and remove the submodule directory located at SUBMODULE_DIRECTORY
git rm -f SUBMODULE_DIRECTORY
```
