---
number: 14545
title: How do I specify a conflict between an extra and a dev dependency group or python version?
type: issue
state: open
author: andrewtruong
labels:
  - question
assignees: []
created_at: 2025-07-10T16:13:12Z
updated_at: 2025-07-10T17:32:53Z
url: https://github.com/astral-sh/uv/issues/14545
synced_at: 2026-01-10T01:25:46Z
---

# How do I specify a conflict between an extra and a dev dependency group or python version?

---

_Issue opened by @andrewtruong on 2025-07-10 16:13_

### Question

Currently I can specify this in `pyproject.toml`

```toml
[tool.uv]
conflicts = [
    [{extra = "abc"}, {extra = "def"}]
]

[project.optional-dependencies]
abc = ["..."]
def = ["..."]
```

But how do I say "the dev dependency group called `def`"?  I was thinking of something like this:

```toml
[tool.uv]
conflicts = [
    [{extra = "abc"}, {group = "def"}]
]

[project.optional-dependencies]
abc = ["..."]

[dependency-groups]
def = ["..."]
```

---

Similarly, I have an extra where the package maintainer requires py310+, but my package only requires py39+.  I want to say "if you use the extra package, then you must be on py39+".

I was thinking of something like this
```toml
[tool.uv]
conflicts = [
    [{extra = "abc"}, {python_version = "<3.10"}]
]

[project.optional-dependencies]
abc = ["..."]
```

### Platform

linux/macos

### Version

0.7.20

---

_Label `question` added by @andrewtruong on 2025-07-10 16:13_

---

_Comment by @BurntSushi on 2025-07-10 17:32_

> But how do I say "the dev dependency group called def"? I was thinking of something like this:

What you have seems right. If it isn't working, can you provide an MRE?

> Similarly, I have an extra where the package maintainer requires py310+, but my package only requires py39+. I want to say "if you use the extra package, then you must be on py39+".

I would put the marker on the dependency specification. Conflicts can only be specified for extras/groups.

---
