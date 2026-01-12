```yaml
number: 2119
title: RET503 - broken autofix/false positive
type: issue
state: closed
author: akx
labels:
  - bug
assignees: []
created_at: 2023-01-24T10:25:27Z
updated_at: 2023-01-24T13:15:27Z
url: https://github.com/astral-sh/ruff/issues/2119
synced_at: 2026-01-12T15:54:42Z
```

# RET503 - broken autofix/false positive

---

_@akx_

I found a snippet where RET503 erroneously adds a `return` inside a loop.

The violation is also a bit suspect; it's not uncommon to early-return from a generator.

### A minimal code snippet that reproduces the bug
```
class X:
    def prompts(self, foo):
        if not foo:
            return []

        for x in foo:
            yield x
            yield x + 1
```
> ret503_bug.py:8:13: RET503 Missing explicit `return` at the end of function able to return non-`None` value
```diff
$ cargo run -- --select=RET --no-cache --isolated ./ret503_bug.py --fix --diff
--- ret503_bug.py
+++ ret503_bug.py
@@ -6,3 +6,4 @@
         for x in foo:
             yield x
             yield x + 1
+            return None
```
### The current Ruff version (`ruff --version`)
cc63a4be6a1bce9b4045826cece50cd84d61d876, but reproducable with 0.0.230 too

---

_Label `bug` added by @charliermarsh on 2023-01-24 12:26_

---

_Comment by @charliermarsh on 2023-01-24 12:26_

It's probably safest to disable these checks entirely for generators.

---

_Comment by @charliermarsh on 2023-01-24 13:04_

I'll do it :)

---

_Closed by @charliermarsh on 2023-01-24 13:15_

---
