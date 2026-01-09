---
number: 8620
title: Using a consistent set of dependencies across many tools with isolated virtual environments
type: issue
state: open
author: jarshwah
labels:
  - question
assignees: []
created_at: 2024-10-28T00:52:15Z
updated_at: 2024-10-28T14:05:56Z
url: https://github.com/astral-sh/uv/issues/8620
synced_at: 2026-01-07T13:12:18-06:00
---

# Using a consistent set of dependencies across many tools with isolated virtual environments

---

_Issue opened by @jarshwah on 2024-10-28 00:52_

I'm curious how we can ensure that we use a consistent set of dependencies across multiple tools and a "development" environment.

A simplified scenario is that we have a bunch of different tools we want to run, some of which share dependencies, and we want to ensure that only the minimal set of dependencies are installed for each tool. Consider a project:

```
[project]
name = "example"
dependencies = [
    "django==5.0",
    ...
]

[dependency-groups]
dev = [
    { include-group = "mypy" }
]
mypy = [
	"mypy==1.2.3",
    "mypy-plugin-x=3.2.1"
]
```

How can I run `mypy` against my project without installing `django` into the environment?

If I'm also using `pre-commit` - how can I ensure that the pre-commit `mypy` hook also uses the specific versions above?

Prior to the announcement of PEP 735 support - I would also wonder how to ensure that developers have the right version of `mypy` locally so that their language server/extension used the correct version of `mypy` - but dependency groups seems to cover that.

What I'm looking for is a single place to define "mypy==1.2.3" so that `uv tool run mypy` uses that version, mypy running in the vscode mypy extension uses that version, and pre-commit running a mypy hook uses that version. If any other tools we run locally also need to reference `mypy` they should *also* be locked to `mypy==1.2.3`.

---

_Comment by @zanieb on 2024-10-28 14:05_

> How can I run mypy against my project without installing django into the environment?

```
uv sync --only-group mypy
```

I think `mypy` generally requires that your project is installed though, so it can inspect dependencies for type information?

> If I'm also using pre-commit - how can I ensure that the pre-commit mypy hook also uses the specific versions above?

Unfortunately, pre-commit doesn't play well with reading versions from outside it's configuration. Maybe with a [system](https://pre-commit.com/#system) hook? Historically, I've written a script to patch its versions to match the external versions.

---

_Label `question` added by @zanieb on 2024-10-28 14:05_

---
