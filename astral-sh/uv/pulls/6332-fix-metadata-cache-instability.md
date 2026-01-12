```yaml
number: 6332
title: Fix metadata cache instability
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/fix-specifier-instability
created_at: 2024-08-21T14:41:36Z
updated_at: 2024-08-21T15:18:44Z
url: https://github.com/astral-sh/uv/pull/6332
synced_at: 2026-01-12T16:07:19Z
```

# Fix metadata cache instability

---

_@konstin_

For a path dep such as the root project, uv can read metadata statically from `pyproject.toml` or dynamically from the build backend.

Python's `packaging` [sorts](https://github.com/pypa/packaging/blob/cc938f984bbbe43c5734b9656c9837ab3a28191f/src/packaging/specifiers.py#L777) specifiers before emitting them, so all build backends built on top of it - such as hatchling - will change the specifier order compared to pyproject.toml. The core metadata spec does say "If a field is not marked as Dynamic, then the value of the field in any wheel built from the sdist MUST match the value in the sdist", but it doesn't specify if "match" means string equivalent or semantically equivalent, so it's arguable if that spec conformant. This change means that the specifiers have a different ordering when coming from the build backend than when read statically from pyproject.toml.

Previously, we tried to read path dep metadata in order:
* From the (built wheel) cache (`packaging` order)
* From pyproject.toml (verbatim specifier)
* From a fresh build (`packaging` order)

This behaviour is unstable: On the first run, we cache is cold, so we read the verbatim specifier from `pyproject.toml`, then we build and store the metadata in the cache. On the second run, we read the `packaging` sorted specifier from the cache.

Reproducer:

```shell
rm -rf newproj
uv init -q --no-config newproj
cd newproj/
uv add -q "anyio>=4,<5"
cat uv.lock | grep "requires-dist"
uv sync -q
cat uv.lock | grep "requires-dist"
cd ..
```

```
requires-dist = [{ name = "anyio", specifier = ">=4,<5" }]
requires-dist = [{ name = "anyio", specifier = "<5,>=4" }]
```

A project either has static metadata, so we can read from pyproject.toml, or it doesn't, and we always read from the build through `packaging`. We can use this to stabilize the behavior by slightly switching the order.

* From pyproject.toml (verbatim specifier)
* From the (built wheel) cache (`packaging` order)
* From a fresh build (`packaging` order)

Potentially, we still want to sort the specifiers we get anyway, after all, the is no guarantee that the specifiers from a build backend are deterministic. But our metadata reading behavior should be independent of the cache state, hence changing the order in the PR.

Fixes #6316


---

_Label `bug` added by @konstin on 2024-08-21 14:41_

---

_Comment by @charliermarsh on 2024-08-21 15:08_

Is this still strictly necessary with #6333? Or more that it's good to have consistent ordering independent of cache state?

---

_Comment by @konstin on 2024-08-21 15:11_

Both this PR and #6333 would fix #6316, but i think this behavior is more consistent, so i'd merge this independent of #6316/#6333.

---

_@charliermarsh approved on 2024-08-21 15:15_

---

_Merged by @konstin on 2024-08-21 15:18_

---

_Closed by @konstin on 2024-08-21 15:18_

---

_Branch deleted on 2024-08-21 15:18_

---
