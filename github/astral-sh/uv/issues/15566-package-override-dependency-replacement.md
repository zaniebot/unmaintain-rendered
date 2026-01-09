---
number: 15566
title: Package Override/Dependency Replacement
type: issue
state: closed
author: qiansen1386
labels:
  - enhancement
assignees: []
created_at: 2025-08-28T08:23:36Z
updated_at: 2025-09-19T05:34:28Z
url: https://github.com/astral-sh/uv/issues/15566
synced_at: 2026-01-07T13:12:19-06:00
---

# Package Override/Dependency Replacement

---

_Issue opened by @qiansen1386 on 2025-08-28 08:23_

### Summary

Hi Astral team,

We've got this internal package `mypymongo` that works just like `pymongo` (same API), but uses our own custom protocol under the hood. What would really help us is if UV could let us swap packages, like whenever some package asks for pymongo, UV would automatically use `mypymongo` instead without actually installing the original. 

Right now we're in this awkward spot where both packages end up installed side-by-side. The behavior gets unpredictable because it depends on which package was installed last. I can't just uninstall pymongo either because packages like `pydantic-pymongo` still require it.

So we're stuck with this janky workaround:

```shell
uv uninstall pymongo
uv run --no-sync ./bootstrap.sh
```

Some AI chatbots fabricated some syntax like
```toml
[tool.uv.overrides]
# Replace any pymongo dependency with internal implementation
pymongo = { source = "mypymongo", version = "==0.0.5" }
```
Unfortunately, none of them works. Would be awesome if something like this actually existed.
What do you think? Could this happen?


### Example

_No response_

---

_Label `enhancement` added by @qiansen1386 on 2025-08-28 08:23_

---

_Comment by @konstin on 2025-08-28 13:08_

While we don't support dependency replacement or elimination yet (https://github.com/astral-sh/uv/issues/4422), you can use an override with `pymongo; sys_platform == "never"` (https://docs.astral.sh/uv/reference/settings/#override-dependencies) to hack the removal of pymongo from your lockfile.

---

_Closed by @qiansen1386 on 2025-08-29 07:11_

---

_Comment by @quantitative-technologies on 2025-09-14 03:47_

This is still installing `multitasking` in the lockfile:

```console
[tool.uv]
# hack to remove from lockfile: https://github.com/astral-sh/uv/issues/15566#issuecomment-3233440761
override-dependencies = ["multitask==0.0.12; sys_platform == 'never'"]  

```

Did I specify incorrectly?

---

_Comment by @konstin on 2025-09-14 11:02_

@quantitative-technologies Please create a minimal reproducible example and open a new issue.

---

_Comment by @quantitative-technologies on 2025-09-16 00:57_

> [@quantitative-technologies](https://github.com/quantitative-technologies) Please create a minimal reproducible example and open a new issue.

It may be difficult: The reason I wanted this is because `multitasking` is causing some other strange issue, and I don't know how to simplify to a MRE. I can open a new issue without an MRE if you want, but briefly uv is failing every other time I run it. For example:

```console
james@ubuntu-8gb-data-store:~/perpetual-arbitrage/scripts$ uv run python
error: Unable to uninstall `multitasking==0.0.12`. distutils-installed distributions do not include the metadata required to uninstall safely.
james@ubuntu-8gb-data-store:~/perpetual-arbitrage/scripts$ uv run python
Installed 1 package in 1ms
Python 3.12.3 (main, Aug 14 2025, 17:47:21) [GCC 13.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> quit()
james@ubuntu-8gb-data-store:~/perpetual-arbitrage/scripts$ uv run python
error: Unable to uninstall `multitasking==0.0.12`. distutils-installed distributions do not include the metadata required to uninstall safely.
```

---

_Comment by @zanieb on 2025-09-16 02:55_

A new issue is better than bumping this one, even if you can't produce an MRE. At least provide the debug logs please, as described in #9452

---

_Comment by @quantitative-technologies on 2025-09-19 05:34_

Done: https://github.com/astral-sh/uv/issues/15941#issue-3432851140

---
