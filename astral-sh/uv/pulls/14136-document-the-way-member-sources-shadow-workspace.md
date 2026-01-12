```yaml
number: 14136
title: document the way member sources shadow workspace sources
type: pull_request
state: merged
author: oconnor663
labels:
  - documentation
assignees: []
merged: true
base: main
head: jack/document_overlapping_sources
created_at: 2025-06-18T19:13:38Z
updated_at: 2025-06-19T09:24:26Z
url: https://github.com/astral-sh/uv/pull/14136
synced_at: 2026-01-12T16:11:03Z
```

# document the way member sources shadow workspace sources

---

_@oconnor663_

Closes https://github.com/astral-sh/uv/issues/14093.

---

_Review requested from @zanieb by @oconnor663 on 2025-06-18 19:13_

---

_Review comment by @zanieb on `docs/concepts/projects/workspaces.md`:121 on 2025-06-18 21:23_

I think "marker" is better as it's more specific — "dependency specifier" includes the version parts.

```suggestion
    `tool.uv.sources` for the same dependency in the workspace root, even if the member's source is
    limited by a [marker](dependencies.md#platform-specific-sources) that doesn't
    match the current platform.
```

---

_@zanieb reviewed on 2025-06-18 21:23_

---

_@zanieb reviewed on 2025-06-18 21:25_

---

_Review comment by @zanieb on `docs/concepts/projects/workspaces.md`:121 on 2025-06-18 21:25_

We may also want to tweak this to be clearer why "limited" is relevant,  e.g.:

```suggestion
    `tool.uv.sources` for the same dependency in the workspace root, even if the member's source is
    not applicable for the current platform, e.g., if there's a [marker](dependencies.md#platform-specific-sources)
    that limits use of the source.
```

---

_@zanieb approved on 2025-06-18 21:25_

---

_Label `documentation` added by @zanieb on 2025-06-18 21:25_

---

_Merged by @oconnor663 on 2025-06-18 22:31_

---

_Closed by @oconnor663 on 2025-06-18 22:31_

---

_Branch deleted on 2025-06-18 22:31_

---

_Comment by @pirate on 2025-06-19 09:24_

is there a way to do something like this?

```
[tool.uv.sources]
somepackage = [
    { path = "../somepackage", editable = true },  # prefer local code dir if available
    { index = "pypi" },                            # fallback to getting it from pypi normally
]
```

I get a confusing error trying to do this currently:

```shell
error: Failed to parse: `pyproject.toml`
  Caused by: TOML parse error at line 81, column 9
   |
81 | somepackage = [
   |         ^
Source markers must be disjoint, but the following markers overlap: `true` and `true`.

hint: replace `true` with `python_version < '0'`.
```

perhaps worth documeting this in the same place? or maybe here https://docs.astral.sh/uv/concepts/indexes/#searching-across-multiple-indexes

---
