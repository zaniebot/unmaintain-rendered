---
number: 4127
title: "Don't warn about prerelease markers for the root package"
type: issue
state: closed
author: konstin
labels:
  - bug
  - error messages
assignees: []
created_at: 2024-06-07T08:57:46Z
updated_at: 2024-06-07T18:43:51Z
url: https://github.com/astral-sh/uv/issues/4127
synced_at: 2026-01-07T13:12:17-06:00
---

# Don't warn about prerelease markers for the root package

---

_Issue opened by @konstin on 2024-06-07 08:57_

When i get a resolution failure (`uv lock`) for

```
[project]
name = "transformers"
version = "4.39.0.dev0"
```

i get told:

```
hint: transformers was requested with a pre-release marker (e.g., any of:
  transformers<4.39.0.dev0
  transformers>4.39.0.dev0
), but pre-releases weren't enabled (try: `--prerelease=allow`)
```

which is misleading.

---

_Label `error messages` added by @konstin on 2024-06-07 08:57_

---

_Comment by @charliermarsh on 2024-06-07 13:12_

This is already fixed on main, right?

---

_Comment by @konstin on 2024-06-07 14:39_

This happens with d3651c13f0a25821e486b8d9d09e6f537cb303b0:

```toml
[project]
name = "transformers"
version = "4.39.0.dev0"
dependencies = [
  "tqdm==4",
  "tqdm==3",
]
```

```
$ cargo run -q -- lock
warning: `uv lock` is experimental and may change without warning.
warning: No `requires-python` field found in `transformers`. Defaulting to `>=3.12`.
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of tqdm==3 and transformers==4.39.0.dev0 depends on tqdm==3, we can conclude that
      transformers==4.39.0.dev0 cannot be used.
      And because only transformers==4.39.0.dev0 is available and transformers depends on transformers, we can conclude that the
      requirements are unsatisfiable.

      hint: transformers was requested with a pre-release marker (e.g., any of:
          transformers<4.39.0.dev0
          transformers>4.39.0.dev0
      ), but pre-releases weren't enabled (try: `--prerelease=allow`)
```

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-07 14:42_

---

_Comment by @charliermarsh on 2024-06-07 14:47_

Weird, ok, thanks.

---

_Referenced in [astral-sh/uv#4140](../../astral-sh/uv/pulls/4140.md) on 2024-06-07 18:30_

---

_Label `bug` added by @charliermarsh on 2024-06-07 18:34_

---

_Closed by @charliermarsh on 2024-06-07 18:43_

---
