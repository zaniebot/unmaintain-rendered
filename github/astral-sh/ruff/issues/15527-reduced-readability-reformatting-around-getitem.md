---
number: 15527
title: Reduced readability reformatting around getitem
type: issue
state: open
author: behrmann
labels:
  - formatter
  - style
assignees: []
created_at: 2025-01-16T09:43:36Z
updated_at: 2025-01-16T11:51:26Z
url: https://github.com/astral-sh/ruff/issues/15527
synced_at: 2026-01-07T13:12:16-06:00
---

# Reduced readability reformatting around getitem

---

_Issue opened by @behrmann on 2025-01-16 09:43_

ruff is a lovely tool, but with ruff 0.9.1 we just experienced a reformatting that reduces readability in the result in [this PR](https://github.com/systemd/mkosi/pull/3366#discussion_r1918087220)

The [bit of code](https://github.com/systemd/mkosi/pull/3366/files#diff-ac46bb8a2229a019bcf1d0667948e3539896a4488e46ab7ca248f8c6c6e4daf8R918) with ruff's reformatting is
```diff
         return needs_repository_key_fetch(namespace.distribution)
 
     if namespace.tools_tree != Path("default"):
-        return (
-            detect_distribution(namespace.tools_tree)[0] == Distribution.ubuntu
-            and needs_repository_key_fetch(namespace.distribution)
-        ) 
+        return detect_distribution(namespace.tools_tree)[
+            0
+        ] == Distribution.ubuntu and needs_repository_key_fetch(namespace.distribution)
 
     return (
         namespace.tools_tree_distribution == Distribution.ubuntu
```

A `# fmt: skip` solved this, but it would nice if it were not necessary.

The used ruff version is 0.9.1 and the relevant settings from `pyproject.toml` are
```toml
[tool.ruff]
target-version = "py39"
line-length = 109
```

---

_Referenced in [systemd/mkosi#3366](../../systemd/mkosi/pulls/3366.md) on 2025-01-16 09:43_

---

_Label `formatter` added by @AlexWaygood on 2025-01-16 10:44_

---

_Label `style` added by @AlexWaygood on 2025-01-16 10:44_

---

_Comment by @MichaReiser on 2025-01-16 11:51_

Thanks for reporting and the kind words. I tend to prefer adding parentheses over splitting the expression the way Ruff does here. I also expect that changing the behavior here might have far reaching effects unless we make it more specific, e.g. never break subscripts with single int literals (I think that's what Prettier does). 

[Playground](https://play.ruff.rs/d840e22a-34d8-4bb5-8a26-ee279648ef9b)

---
