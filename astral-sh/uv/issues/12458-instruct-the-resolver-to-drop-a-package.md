```yaml
number: 12458
title: Instruct the resolver to drop a package
type: issue
state: closed
author: zmeir
labels:
  - question
assignees: []
created_at: 2025-03-25T08:12:37Z
updated_at: 2025-10-31T14:18:47Z
url: https://github.com/astral-sh/uv/issues/12458
synced_at: 2026-01-12T16:01:03Z
```

# Instruct the resolver to drop a package

---

_@zmeir_

### Question

In my use-case I have a project that depends on package `a` which then depends on `b`.

It looks something like this:
```toml
[project]
name = "demo"
version = "0.1.0"
requires-python = ">=3.10"
dependencies = ["a"]
```

I want to override `b` with a custom distribution - say `custom_b`.  
If you `pip install b` or `pip install custom_b`, both will put `b.py` in your site-packages.

So if I do something like this:
```toml
dependencies = ["a", "custom_b"]
```

Then when I run `uv sync` the following happens:
1. `a` brings `b` which will put `b.py` in my site-packages
2. `custom_b` will put a different `b.py` in my site-packages
Whichever `b.py` wins depends on the order in which they were installed.

I want to be able to tell the resolver to drop `b` altogether.  
Similarly to how I can instruct it to override the required version set by `a`:
```toml
[tool.uv]
override-dependencies = ["b==0.0.1+not.what.a.required"]
```
I want to do something like this:
```toml
[tool.uv]
override-dependencies = ["b==<DROP>"]
```

Is there a way to do this?

### Platform

Darwin 23.6.0 arm64

### Version

uv 0.6.4 (04db70662 2025-03-03)

---

_Label `question` added by @zmeir on 2025-03-25 08:12_

---

_Comment by @charliermarsh on 2025-03-25 13:22_

This is typically solved by doing something like:

```
[tool.uv]
override-dependencies = ["b ; python_version < 0"]
```

---

_Comment by @zmeir on 2025-03-25 14:09_

That's exactly what I needed. Thanks @charliermarsh !

Small edit - I had to change your example to this in order to make it work:
```toml
[tool.uv]
override-dependencies = ["b ; python_version < '0'"]
```

---

_Closed by @zmeir on 2025-03-25 14:09_

---

_Comment by @zanieb on 2025-04-01 19:05_

Or `sys_platform == "never"`

---

_Comment by @charliermarsh on 2025-10-31 14:18_

We just added support for excluding dependencies entirely by name: https://github.com/astral-sh/uv/pull/16528 (available in the next release)

---
