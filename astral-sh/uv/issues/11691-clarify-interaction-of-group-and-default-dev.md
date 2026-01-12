```yaml
number: 11691
title: "Clarify interaction of `--group` and default dev dependencies"
type: issue
state: open
author: rsyring
labels:
  - question
assignees: []
created_at: 2025-02-21T07:25:23Z
updated_at: 2025-02-21T09:28:20Z
url: https://github.com/astral-sh/uv/issues/11691
synced_at: 2026-01-12T16:00:44Z
```

# Clarify interaction of `--group` and default dev dependencies

---

_@rsyring_

### Question

Considering dependency groups like:

```
[dependency-groups]
dev = [
    {include-group = "tests"},
    'hatch>=1.14.0',
    'pip-audit',
    'ruff>=0.9.6',
]
tests = [
    'nox>=2025.2.9',
    'pytest>=8.3.4',
]
```

When prepping for test runs in nox, I'd expect to be able to do:

```
uv sync --group tests
```
But with that invocation, the dev dependency group is also included.  My next attempt was:

```
uv sync --only-group tests
```

But that doesn't include the project's dependencies either, which doesn't work for testing.  So my final attempt is:

```
uv sync --no-dev --group tests
```

Which works but seems clunky.  IMO, it would be more intuitive if:

```
# install project + group tests
uv sync --group tests

# install project + group tests and group dev
uv sync --group dev --group tests
```

And if I always want dev installed when tests is installed then I use `include-group` to make that happen instead of relying on implicit dev install.

IMO, uv isn't currently using the dev group as a _default_.  A default implies it's the selection _unless_ something else is chosen.  I think if uv adjusted it to be a true default, the ux might be better.

Also, FWIW, that there are seven different CLI options for dealing with dependency groups.  Does that seem like a lot to anyone else?

As always, YMMV.  :)

Thanks.


### Platform

Ubuntu 24.04 x64

### Version

0.6.2

---

_Label `question` added by @rsyring on 2025-02-21 07:25_

---

_Comment by @T-256 on 2025-02-21 08:48_

IIUC, You mean using `--group` should disable `tool.uv.default-groups` functionality. I'm favor of that then 👍 

---
