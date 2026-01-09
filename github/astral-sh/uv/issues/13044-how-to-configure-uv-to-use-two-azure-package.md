---
number: 13044
title: How to configure uv to use two azure package registry?
type: issue
state: open
author: krasnikau-andrei
labels:
  - question
assignees: []
created_at: 2025-04-22T07:29:31Z
updated_at: 2025-04-26T12:35:13Z
url: https://github.com/astral-sh/uv/issues/13044
synced_at: 2026-01-07T13:12:18-06:00
---

# How to configure uv to use two azure package registry?

---

_Issue opened by @krasnikau-andrei on 2025-04-22 07:29_

### Question

My current .netrc
```netrc
machine pkgs.dev.azure.com
    login repo1
    password <PAT1>
machine pkgs.dev.azure.com
    login repo2
    password <PAT2>
```

Part of pyproject.toml
```toml
...

[[tool.uv.index]]
name = "repository1"
url = "https://pkgs.dev.azure.com/repo1/Project1/_packaging/some-other-repo/pypi/simple/"
explicit = true

[[tool.uv.index]]
name = "repository2"
url = "https://pkgs.dev.azure.com/repo2/Project2/_packaging/some-repo/pypi/simple/"
explicit = true
...
```

`repo2` works, but `repo1` shows an authorization error. Suppose I remove the `repo2` credential from .netrc, the `repo1` starts to work. I assume the current .netrc just overrides the previous record. How to make it work together?

Also, I tried a config like this, but no result
```toml
...
[[tool.uv.index]]
name = "repository2"
url = "https://repo2@pkgs.dev.azure.com/repo2/Project2/_packaging/some-repo/pypi/simple/"
explicit = true
...
```


### Platform

Windows 11

### Version

uv 0.6.14 (a4cec56dc 2025-04-09)

---

_Label `question` added by @krasnikau-andrei on 2025-04-22 07:29_

---

_Comment by @krasnikau-andrei on 2025-04-22 17:31_

I can't use .netrc
I can't use .env
Keyring somehow also doesn't work :(

---

_Assigned to @jtfmumm by @konstin on 2025-04-23 11:30_

---

_Comment by @jtfmumm on 2025-04-23 15:26_

This is probably the same issue as seen in #13059, where I [describe a workaround](https://github.com/astral-sh/uv/issues/13059#issuecomment-2824667055) for the time being. Let me know if that works for you.

---

_Comment by @krasnikau-andrei on 2025-04-25 13:08_

Thanks for the help. We decided not to move to UV right now. 

---

_Comment by @jtfmumm on 2025-04-25 14:03_

> Thanks for the help. We decided not to move to UV right now.

Do you mind sharing the reasons? It can be helpful for us if you're comfortable with it.

---

_Comment by @krasnikau-andrei on 2025-04-26 12:35_

We use Azure private registries, and there are several ways to configure authorization for them:
1. .netrc - doesn't work for 2 registries
2. env - #13059 also didn't work. And it is not very useful to write envs for 3 repos. .env file couldn't be loaded automatically and could be specified only for the `uv run` command
3. keyring - also didn't work. Probably because same reasons as in #13059 
4. pat in toml - we can't do that for security reasons

We will wait until the tool is more stable for such tasks. Currently, I can't create a frictionless way to use it. It works great for public projects, but it's an unnecessary risk for commercial development. 

---
