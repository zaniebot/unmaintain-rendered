```yaml
number: 17344
title: "`ruff lint --fix` adds superfluous-seeming blank line to start of file when using isort and flake8 rules together"
type: issue
state: open
author: relrod
labels:
  - isort
  - incompatibility
assignees: []
created_at: 2025-04-11T00:47:54Z
updated_at: 2025-04-11T08:39:50Z
url: https://github.com/astral-sh/ruff/issues/17344
synced_at: 2026-01-12T15:54:55Z
```

# `ruff lint --fix` adds superfluous-seeming blank line to start of file when using isort and flake8 rules together

---

_@relrod_

### Summary

I tried to search the issues for this, apologies if I missed an existing issue.

Taking this file as an example:

```python
import somelib
import logging


def foo(a, b):
    print(somelib)
```

If I run this with `ruff check foo.py --select I,F` it correctly points out there are two issues: The imports aren't properly sorted, and `logging` is ununsed.

However, if I tell it to try to `--fix` the issues, it generates this change:

```diff
rick@Ricks-iMac /tmp % ruff check foo.py --select I,F --diff
--- foo.py
+++ foo.py
@@ -1,5 +1,5 @@
+
 import somelib
-import logging


 def foo(a, b):

Would fix 2 errors.
```

Notice the spurious blank line that gets added above `import somelib`.

This doesn't happen if I change `logging` to something like `foo`. I _think_ ruff is getting confused about import groups here. It sees `logging` is an stdlib import and should be in a separate group and it still tries to separate the "groups" even though it removes the unused import.

### Version

ruff 0.11.5 (7186d5e9a 2025-04-10)

---

_Comment by @relrod on 2025-04-11 00:50_

This might be a dupe of #4661 or at least related to it.

---

_Comment by @MichaReiser on 2025-04-11 06:35_

Thakns for creating this issue and finding #4661. Yes, that issue seems related. I wonder if we could change the priority of `isort` to always run last, or at least after removing unused imports in here 

https://github.com/astral-sh/ruff/blob/22de00de16da27d47d6e1a657fec4397138d6e40/crates/ruff_linter/src/fix/mod.rs#L133-L162

because I only observe this bug if both fixes are applied in one run. Applying either fix manually results in the desired outcome. 

https://play.ruff.rs/a721ac48-7343-464f-90cd-aab3cda4715f

---

_Label `isort` added by @MichaReiser on 2025-04-11 06:36_

---

_Label `incompatibility` added by @MichaReiser on 2025-04-11 06:36_

---
