```yaml
number: 13535
title: "Add support for `uvx python@latest`"
type: issue
state: open
author: zanieb
labels:
  - enhancement
assignees: []
created_at: 2025-05-19T13:48:10Z
updated_at: 2025-06-05T19:06:36Z
url: https://github.com/astral-sh/uv/issues/13535
synced_at: 2026-01-12T16:01:31Z
```

# Add support for `uvx python@latest`

---

_@zanieb_

### Summary

Currently, this is a TODO

https://github.com/astral-sh/uv/blob/c5032aee80d5a666a45ec5e095b8c8abeffc11fb/crates/uv/src/commands/tool/run.rs#L705

### Example

```
$ uvx python@latest --version
Python 3.13.3
```

---

_Label `enhancement` added by @zanieb on 2025-05-19 13:48_

---

_Comment by @maxmynter on 2025-06-05 12:28_

Trying a jab at it :) 

---

_Comment by @zanieb on 2025-06-05 13:00_

What's your approach? Are you going to take the latest download or the latest interpreter on the system?

---

_Comment by @maxmynter on 2025-06-05 19:06_

I'm taking the interpreter with the highest version number. It is also how i would intuitively understand how latest works, though I can see why someone would have the other intuition.

What is your preference? 

---
