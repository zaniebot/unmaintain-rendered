```yaml
number: 18941
title: "New rule: \"Prefer `Path(\"/foo/bar/\", baz)` over `Path(f\"/foo/bar/{baz}\"`"
type: issue
state: open
author: septatrix
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-06-25T19:02:42Z
updated_at: 2025-06-25T19:05:30Z
url: https://github.com/astral-sh/ruff/issues/18941
synced_at: 2026-01-12T15:54:56Z
```

# New rule: "Prefer `Path("/foo/bar/", baz)` over `Path(f"/foo/bar/{baz}"`

---

_@septatrix_

### Summary

The former is cleaner as it avoids the unnecessary f-string interpolation and instead uses the fact that `pathlib.Path` simply allows passing components as args. Using the latter can also be a source of subtle bugs as it uses normal string interpolation using `str()` instead of the `PathLike` protocol though one could do that explicitly `Path(f"/foo/bar/{os.fspath(baz)}"`.

This lint should only trigger if the replacement is a standalone component, e.g. `f"/foo/{bar}"`, `f"/{foo}/bar"`, or `f"/foo/{bar}/baz"`. It should likely not trigger (or be made configurable) if it is only part of a path component as in `f"/foo/{bar}.txt"` or `f"/foo/{bar}.backup/baz"`

---

_Label `rule` added by @ntBre on 2025-06-25 19:05_

---

_Label `needs-decision` added by @ntBre on 2025-06-25 19:05_

---
