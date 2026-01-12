```yaml
number: 11473
title: "Behaviour of `uv sync` with extras?"
type: issue
state: closed
author: mschwoer
labels:
  - question
assignees: []
created_at: 2025-02-13T10:05:04Z
updated_at: 2025-03-18T22:27:23Z
url: https://github.com/astral-sh/uv/issues/11473
synced_at: 2026-01-12T16:00:37Z
```

# Behaviour of `uv sync` with extras?

---

_@mschwoer_

### Question

Hey,
I'd like to transition from `pip` to `uv`, but there's still a behavior that puzzles me with `uv`.

Basically, I want to offer two installation flavours, one with pinned requirements ('stable') and one with loose requirements (to allow easier integration into existing environments, on the users' risk).

With the below `pyproject.toml`, `pip`'s behaviour is nicely reproduced by `uv`
(all commands assume a 'clean' environment, so `.venv`, `build` and `egg-info` being absent):

> `uv pip install .`    => scikit-learn==1.6.1   OK
> `uv pip install ".[stable]"`    => scikit-learn==1.6.0   OK 

However, when using `uv sync`, the behaviour is different:
> `uv sync` => scikit-learn==1.6.0  would expect 1.6.1 here
> `uv sync --no-extra stable` => scikit-learn==1.6.0  would expect 1.6.1 here

How can I select the 'stable' extra in `uv sync`?

Moreover, when adding this
`
optional-dependencies.future = [
    "scikit-learn==1.6.1"
]
`
I get an `No solution found when resolving dependencies`

It could well be that I didn't get the role of `uv sync` right? I think of it as an equivalent of `pip install [whatever requirement is defined in the given extra]`?

best,
Magnus

--- 
Steps to reproduce:
> uv init explore-uv

pyproject.toml:

```
[project]
name = "explore-uv"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"

dependencies = [
    "scikit-learn"
]
optional-dependencies.stable = [
    "scikit-learn==1.6.0"
]

#optional-dependencies.stable2 = [
#    "scikit-learn==1.6.1"
#]
```

### Platform

macOS

### Version

uv 0.5.29 (ca73c4754 2025-02-05)

---

_Label `question` added by @mschwoer on 2025-02-13 10:05_

---

_Comment by @konstin on 2025-02-13 14:13_

For the pinned requirements stable flavor, you can use `uv export` and have users use that file as constraints file. This trick means that the package ship the loose requirements but user can still coerce their resolution into the test pinned version with e.g. `pip install -c <constraint_file>`. 

In `uv sync`, uv requires all extras to be compatible with each other, including the base dependencies and each extra. When uv see that one extra requires a specific version, that version gets coerced for the entire package. That is different from `uv pip`, that only looks at the actually used extras.

[conflicting dependencies](https://docs.astral.sh/uv/concepts/projects/config/#conflicting-dependencies) may be another option for you: You can declare that two extras are never installed at the same time, so their dependencies are allowed to conflict and can have different version (even tough uv generally tries to reuse versions where possible).

---

_Closed by @charliermarsh on 2025-03-18 22:27_

---
