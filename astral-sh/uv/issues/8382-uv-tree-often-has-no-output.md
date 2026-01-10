```yaml
number: 8382
title: uv tree often has no output
type: issue
state: closed
author: dimbleby
labels:
  - bug
assignees: []
created_at: 2024-10-20T12:45:51Z
updated_at: 2024-10-23T18:43:28Z
url: https://github.com/astral-sh/uv/issues/8382
synced_at: 2026-01-10T04:36:20Z
```

# uv tree often has no output

---

_Issue opened by @dimbleby on 2024-10-20 12:45_

sometimes `uv tree` will decline to say anything

```toml
[project]
name = "foo"
version = "0.1.0"
requires-python = ">=3.13"
dependencies = [
  "aiohttp>=3.9.5",
]
```
```shell
$ uv tree   # NB looks good
Resolved 10 packages in 19ms
foo v0.1.0
└── aiohttp v3.10.10
    ├── aiohappyeyeballs v2.4.3
    ├── aiosignal v1.3.1
    │   └── frozenlist v1.4.1
    ├── attrs v24.2.0
    ├── frozenlist v1.4.1
    ├── multidict v6.1.0
    └── yarl v1.15.5
        ├── idna v3.10
        ├── multidict v6.1.0
        └── propcache v0.2.0

$ uv tree --package propcache  # NB no output
Resolved 10 packages in 1ms

$ uv tree --package propcache --invert  # NB apparently invert will tell me something
Resolved 10 packages in 1ms
propcache v0.2.0
└── yarl v1.15.5
    └── aiohttp v3.10.10
        └── foo v0.1.0

$ uv tree --package yarl  # NB nothing to say about yarl
Resolved 10 packages in 1ms

$ uv tree --package yarl --invert  # NB even when inverted
Resolved 10 packages in 1ms
```

is there some not-so-obvious logic to this that I am missing?

---

_Comment by @j178 on 2024-10-20 13:30_

The `--package` flag refers to a package within the workspace itself, not a dependency. I agree that this flag can be a bit confusing.

---

_Comment by @dimbleby on 2024-10-20 14:23_

> The --package flag refers to a package within the workspace itself

it certainly must be more subtle than that, per the examples of `uv tree --package propcache --invert` and `uv tree --package yarl --invert`.  Neither of these is "a package within the workspace itself", the results appear to be inconsistent

---

_Label `bug` added by @zanieb on 2024-10-20 14:33_

---

_Comment by @zanieb on 2024-10-20 14:33_

Thanks! Seems like something weird is going on here.

---

_Comment by @dimbleby on 2024-10-20 14:35_

I think the actual answer is that the tree display insists on starting from a "root".  
- For non-inverted trees that is workspace packages.
- For inverted trees that is dependencies with no further dependencies

imo the first is maybe defensible - but it would be more helpful to show the tree starting from the package the user asked for.

also imo, the second is undesirable: eg it makes it harder for me to answer a question like "why is yarl installed?"

---

_Comment by @zanieb on 2024-10-20 14:39_

I think @charliermarsh is likely to have some insight.

I agree it seems sensible that `--package` should just start at an arbitrary package in the tree — but it's worth noting this is inconsistent with the meaning of `--package` in every other project command. I kind of feel like `--package` should have been `--member` 😢.

---

_Comment by @charliermarsh on 2024-10-21 01:06_

Yeah I think some things are going wrong here. I think we naively limit the tree to "roots" because we don't want to show duplicates.

---

_Comment by @charliermarsh on 2024-10-22 03:07_

(So yes, definitely some bugs here.)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-23 03:04_

---

_Comment by @charliermarsh on 2024-10-23 17:17_

I'm rewriting this to fix these problems and make things simpler.

---

_Closed by @charliermarsh on 2024-10-23 18:43_

---

_Closed by @charliermarsh on 2024-10-23 18:43_

---
