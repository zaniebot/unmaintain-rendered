```yaml
number: 4050
title: "Locking depends on the current python version if `requires-python` isn't set"
type: issue
state: closed
author: konstin
labels:
  - preview
assignees: []
created_at: 2024-06-05T15:19:14Z
updated_at: 2024-06-05T20:45:52Z
url: https://github.com/astral-sh/uv/issues/4050
synced_at: 2026-01-12T15:58:47Z
```

# Locking depends on the current python version if `requires-python` isn't set

---

_@konstin_

Locking dependencies with `uv lock` or `uv run` depends on the current python version when no explicit `requires-python` is given:

```
[project]
name = "albatross"
version = "0.1.0"
dependencies = ["numpy"]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

This resolves different numpy versions with `uv venv -p 3.8 && uv run --preview` vs. `uv venv -p 3.12 && uv run --preview`, since the latest numpy versions don't support python 3.8.

When the lockfile is committed to git, it means that each invocation by a developer with a different default python version will create lockfile diff, and it destroy the shared environment promise of lockfiles.

Poetry and pdm both generate a `require-python` or `Python` entry by default. When this entry is removed, they enforce compatibility with all python versions. pdm complains about this when backtracking and backtracks to 1.21.1, poetry silently backtracks to 1.16.6.

---

_Label `bug` added by @konstin on 2024-06-05 15:19_

---

_Label `preview` added by @konstin on 2024-06-05 15:19_

---

_Comment by @zanieb on 2024-06-05 15:26_

I'm not sure this is a "bug" but yeah we might want to change it. I'm not particularly opposed to the behavior, although a warning may make sense.

---

_Comment by @charliermarsh on 2024-06-05 15:35_

Yeah I actually think the current behavior is reasonable though I know it could be a footgun for users.

---

_Label `bug` removed by @charliermarsh on 2024-06-05 15:35_

---

_Comment by @konstin on 2024-06-05 15:58_

You can turn this into a bug easily:

```
uv venv -p 3.12 && uv lock --preview
uv venv -p 3.8 && uv sync
```

This ignores the `requires-python: ">=3.9"` bound on `numpy-1.26.4.tar.gz` and errors trying to build numpy with:

```
meson-python: error: Package requires Python version >=3.9, running on 3.8.18
```


---

_Comment by @zanieb on 2024-06-05 16:00_

Do we not encode the Python version range in the lock file and error if it cannot be fulfilled? That sounds like a separate issue from whatever we default to using.

---

_Comment by @konstin on 2024-06-05 16:08_

We currently don't, but picking a default range and adding it to the lockfile would be one solution. Others are:
* Make specifying `requires-python` mandatory
* Warn when `requires-python` is missing (any behavior after, users have been warned)
* Pick a default `requires-python` range, say all non-EOL python versions
* Assume all python versions (poetry seems to do this)

There might be more solutions, they work as long as we get a consistent range no matter from what python we invoke the locking. 

---

_Comment by @charliermarsh on 2024-06-05 16:12_

Adding the Python requirement to the lockfile seems important yeah.

---

_Comment by @zanieb on 2024-06-05 16:33_

I created a separate issue for that problem at #4052 

I think warning that `requires-python` is missing and requiring the current minor version seems pretty decent?

I think it's totally normal for an "application"-style project to just rely on the current Python version being consistent.

---

_Comment by @charliermarsh on 2024-06-05 16:34_

@konstin - are you planning to do this or should I take it?

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-05 16:45_

---

_Comment by @konstin on 2024-06-05 17:51_

I'm not certain yet which options of those is the right one, you can take it

---

_Comment by @charliermarsh on 2024-06-05 20:27_

In https://github.com/astral-sh/uv/pull/4070, I made it such that if `requires-python` is missing, we warn and use the current minor as a minimum version.

---

_Closed by @charliermarsh on 2024-06-05 20:45_

---
