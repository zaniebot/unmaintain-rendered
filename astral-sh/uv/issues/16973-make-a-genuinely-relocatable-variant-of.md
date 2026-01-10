---
number: 16973
title: Make a genuinely relocatable variant of --relocatable (i.e. fix or remove activate.csh/nu)
type: issue
state: closed
author: amluto
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2025-12-03T19:51:40Z
updated_at: 2025-12-09T14:39:57Z
url: https://github.com/astral-sh/uv/issues/16973
synced_at: 2026-01-10T01:26:12Z
---

# Make a genuinely relocatable variant of --relocatable (i.e. fix or remove activate.csh/nu)

---

_Issue opened by @amluto on 2025-12-03 19:51_

### Summary

Right now, `uv venv --relocatable` hardcodes the venv path in bin/activate.csh and bin/activate.nu.  I'm trying to use uv to build a deployable artifact for a Python application (not for outside distribution -- wheels are fine for this; I want to deploy it to my own prod system).

Prior to using uv, I worked fairly hard to avoid leaking the working directory into the build.  Right now, I have:

```
uv venv --relocatable "$dist_path"
# uv venv --relocatable will create two non-relocatable files.  We don't need them.
rm -- "$dist_path/bin/activate.csh" "$dist_path/bin/activate.nu"
```

which is rather sad.  Would uv consider either finding a way to fix up activate.csh and activate.nu or having a variant of --relocatable that means "make it genuinely fully relocatable" and omits the offending files?

See also #6970.  uv is *so close* to being a fantastic tool for deploying Python code.

### Example

_No response_

---

_Label `enhancement` added by @amluto on 2025-12-03 19:51_

---

_Label `help wanted` added by @zanieb on 2025-12-04 15:21_

---

_Comment by @zanieb on 2025-12-04 15:22_

It seems reasonable to make them relocatable unless there are firm blockers to that in which case... I guess I'd omit them? That's a breaking change though.

---

_Referenced in [astral-sh/uv#17036](../../astral-sh/uv/pulls/17036.md) on 2025-12-08 17:47_

---

_Closed by @zanieb on 2025-12-09 14:39_

---
