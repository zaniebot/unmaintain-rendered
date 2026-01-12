```yaml
number: 1038
title: Target Python version does not take precedence when determining build interpreter
type: issue
state: closed
author: zanieb
labels: []
assignees: []
created_at: 2024-01-22T17:22:47Z
updated_at: 2024-01-24T17:12:04Z
url: https://github.com/astral-sh/uv/issues/1038
synced_at: 2026-01-12T15:58:25Z
```

# Target Python version does not take precedence when determining build interpreter

---

_@zanieb_

When providing a target version to `pip compile` we must determine two things:

1. What Python version should we use for tags during resolution?
1. What Python interpreter should we use to build wheels?

Currently

1. We use the latest known patch version of the Python version (if not explicitly provided and if available)
2. If in a virtual environment, we use that version regardless of the target version

For example, if I use `pip compile --python-version 3.11` and have 3.11 installed but am in a 3.12 virtual environment we use 3.12 to build wheels. Consequently, if a wheel cannot be built on Python 3.12 we will say the resolution is unsatisfiable.

I believe that we should change (2) to have a preference for the target Python version if available. If not available, it should use the closest patch version with a matching minor version. If that's not available, it should use the current environment's version. This change would mean that when you provide an alternative Python version you may end up with compiled requirements that are not installable in your _current_ environment but our outcome will be more correct with regards to installation in an environment with the target version available.

The current behavior appears to be historical, ensuring that we had a virtual environment to perform builds in. Now, I believe we create a temporary virtual environment for builds.

---

_Assigned to @zanieb by @zanieb on 2024-01-22 17:34_

---

_Comment by @charliermarsh on 2024-01-22 17:36_

> The current behavior appears to be historical, ensuring that we had a virtual environment to perform builds in.

It used to be that we required a virtualenv for `pip compile` and `pip sync`. But, we only really need one for `pip sync` (since we're installing into it). So that requirement was lifted.

Whatever we do here, it should probably be in sync with `puffin venv` which I think @konstin is tweaking.

---

_Closed by @zanieb on 2024-01-24 17:12_

---
