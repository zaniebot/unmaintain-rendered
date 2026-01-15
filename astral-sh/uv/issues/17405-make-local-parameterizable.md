```yaml
number: 17405
title: Make .local Parameterizable
type: issue
state: open
author: muellert
labels:
  - question
assignees: []
created_at: 2026-01-11T15:12:07Z
updated_at: 2026-01-15T12:01:59Z
url: https://github.com/astral-sh/uv/issues/17405
synced_at: 2026-01-15T12:53:16Z
```

# Make .local Parameterizable

---

_@muellert_

### Summary

There's a way to override the location of the cache, --cache-dir, but there seems to be no such option to override the directory to be used for ~/.local . It would be great to have such an option to make it easier to create read-only deployments, or where ~/.local doesn't have enough space.

### Example

_No response_

---

_Label `enhancement` added by @muellert on 2026-01-11 15:12_

---

_Comment by @konstin on 2026-01-12 10:34_

uv reads the `XDG_` variables, can you use those?

---

_Label `enhancement` removed by @zanieb on 2026-01-12 23:58_

---

_Label `question` added by @zanieb on 2026-01-12 23:58_

---

_Comment by @zanieb on 2026-01-12 23:58_

Yeah using `XDG_*` is the appropriate pattern here.

---

_Comment by @muellert on 2026-01-13 01:50_

I'm unconvinced. I've tried that for a different application, and it completely messed up my desktop configuration even though I was only setting these variables to where the configurations usually are (ie, ~/.config and ~/.local). Whether it'd work for non-interactive situations, I'll have to check separately. Also, if it was, why is it not enough for the --cache-dir, ie. XDG_CACHE_HOME?

---

_Comment by @zanieb on 2026-01-13 02:08_

Have you reviewed https://docs.astral.sh/uv/reference/storage/ ?

---

_Comment by @muellert on 2026-01-15 12:01_

I just did, and it suggests that I should set up something with symlinks and/or .env files. Not exactly what I was hoping for.

---
