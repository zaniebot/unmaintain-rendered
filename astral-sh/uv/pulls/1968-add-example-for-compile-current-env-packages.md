```yaml
number: 1968
title: Add example for compile current env packages
type: pull_request
state: merged
author: T-256
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-2
created_at: 2024-02-25T20:04:38Z
updated_at: 2024-03-19T01:06:15Z
url: https://github.com/astral-sh/uv/pull/1968
synced_at: 2026-01-10T14:49:08Z
```

# Add example for compile current env packages

---

_Pull request opened by @T-256 on 2024-02-25 20:04_


## Summary
I was going to create a feature-request issue for adding graph like showing of currently installed dependencies in venv, but then I found `uv pip freeze | uv pip compile` is what I'm looking for.

As one who manually pip-installs packages every once in a while (instead of edit `requirements.in` each time), after a while I would need to have graph of installed packages (e.g. to see how uninstallinga packages effects on others).

I found this command is so useful and wondered if we could have this in docs as one of `uv` features.
## Test Plan

<!-- How was it tested? -->


---

_Comment by @mitsuhiko on 2024-02-26 12:40_

~~I don't think that command works in the general sense. For instance if you try to run this on the `rye` project you will see this failure:~~

```
$ ~/.rye/uv/0.1.9/uv pip freeze | ~/.rye/uv/0.1.9/uv pip compile -
error: Couldn't parse requirement in `-` at position 570
  Caused by: Trailing `(from file:///Users/mitsuhiko/Development/rye)` is not allowed
rye-dev==1.0.0 (from file:///Users/mitsuhiko/Development/rye)
       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```

~~(Still need to check if newer uv produces a different freeze output but I'm guessing not)~~

Nevermind. It does produce a better output on newer versions.

---

_Comment by @charliermarsh on 2024-03-03 20:16_

Can you say a bit more about how / when you use this?

---

_Label `documentation` added by @charliermarsh on 2024-03-03 20:16_

---

_Comment by @T-256 on 2024-03-03 20:28_

> Can you say a bit more about how / when you use this?

When I need a dependencies-tree in an environment, and I didn't have/use `requirements.in`.
So, first step is to export deps using `pip freeze`. Also, I don't want to create temporary `requirements.in`, so I pipe it to `pip compile`.


---

_@zanieb reviewed on 2024-03-18 14:28_

---

_Review comment by @zanieb on `README.md`:87 on 2024-03-18 14:28_

Can we include a pipe here?
```suggestion
echo flask | uv pip compile - -o requirements.txt  # Read from stdin.
```

---

_@zanieb reviewed on 2024-03-18 14:29_

---

_Review comment by @zanieb on `README.md`:88 on 2024-03-18 14:29_

```suggestion
uv pip freeze | uv pip compile - -o requirements.txt  # Lock the current environment.
```

---

_Assigned to @zanieb by @zanieb on 2024-03-18 14:29_

---

_Comment by @zanieb on 2024-03-19 01:06_

Thanks!

---

_Merged by @zanieb on 2024-03-19 01:06_

---

_Closed by @zanieb on 2024-03-19 01:06_

---
