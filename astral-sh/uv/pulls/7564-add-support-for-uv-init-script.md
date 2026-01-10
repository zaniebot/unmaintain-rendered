```yaml
number: 7564
title: Add support for uv init --script
type: pull_request
state: closed
author: tfsingh
labels: []
assignees: []
draft: true
base: main
head: tfsingh/init-script
created_at: 2024-09-20T00:14:16Z
updated_at: 2024-09-20T00:22:27Z
url: https://github.com/astral-sh/uv/pull/7564
synced_at: 2026-01-10T12:53:50Z
```

# Add support for uv init --script

---

_Pull request opened by @tfsingh on 2024-09-20 00:14_

This PR adds support for ```uv init --script```, as defined in issue #7402 (started working on this before I saw jbvsmo's PR). Wanted to highlight a few decisions I made that differ from the existing PR:

1. Script just takes a path, instead of a path/name. This potentially leads to a little ambiguity (I can certainly elaborate on any inline docs, lmk!), but strictly allowing ```uv init --script path/to/script.py``` felt a little more natural than allowing for ```uv init --script path/to --name script.py``` (which I also thought would prompt more questions for users, such as should the name include the .py extension?) 
2. The request is processed immediately in ```init```, sharing logic in resolving which python version to use with ```uv add --script```. This made more sense to me — since scripts are meant to operate in isolation, they shouldn't consider the context of an encompassing package should one exist (I also think the decision makes the relative codepaths easier to follow).
3. No readme — this felt a little excessive for a script, but I can of course add them in!

@jbvsmo  – hope it's okay I'm using your documentation! 

---

_Closed by @tfsingh on 2024-09-20 00:22_

---
