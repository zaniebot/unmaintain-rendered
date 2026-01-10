```yaml
number: 1749
title: fix priority of namespace vs non-namespace packages
type: issue
state: open
author: carljm
labels:
  - imports
assignees: []
created_at: 2025-12-03T21:33:45Z
updated_at: 2025-12-03T21:37:36Z
url: https://github.com/astral-sh/ty/issues/1749
synced_at: 2026-01-10T01:56:41Z
```

# fix priority of namespace vs non-namespace packages

---

_Issue opened by @carljm on 2025-12-03 21:33_

Consider a directory structure like this:

```
path-one/
   mod/
      sub1.py
path-two/
   mod/
      __init__.py
      sub2.py
```

Where `path-one` and `path-two` are both import-search-path entries, in that order (e.g. `PYTHONPATH=path-one:path-two"` should make this happen both for ty and at runtime).

At runtime, a non-namespace package _anywhere on the search path_ takes precedence over a namespace package. This means that with the above setup, `import mod.sub1` fails. Despite being first on the search path, `path-one/mod` is never considered as a candidate for the `mod` package, because `path-two/mod/__init__.py` exists.

Given `import mod` in the above scenario, we do prefer `path-two/mod/__init__.py` over the `path-one/mod` namespace package. But we wrongly allow `import mod.sub1` to succeed, resolving to the `path-one` namespace package.

In this particular case, that might seem harmless, but it can cause us to import the wrong thing in some scenarios, and it can lead to some very odd inconsistencies with relative imports, documented in `import/workspaces.md`. Allowing imports of modules in the "should be ignored" namespace package to just fail would allow our "desperate resolution" to kick in (for imports within that "should be ignored" namespace package) and actually give more consistent behavior.

---

_Added to milestone `Stable` by @carljm on 2025-12-03 21:33_

---

_Comment by @carljm on 2025-12-03 21:34_

I'm currently marking this a P2 for stable, but it should probably get bumped up in priority if we get many reports of it in the wild, or maybe bumped out of stable entirely if we never do.

---

_Label `imports` added by @carljm on 2025-12-03 21:35_

---
