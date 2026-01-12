```yaml
number: 15805
title: ruff adds import aliases on unused import with F401 rule and the preview option
type: issue
state: closed
author: skshetry
labels:
  - question
  - fixes
  - preview
assignees: []
created_at: 2025-01-29T07:23:06Z
updated_at: 2025-03-24T12:16:37Z
url: https://github.com/astral-sh/ruff/issues/15805
synced_at: 2026-01-12T15:54:55Z
```

# ruff adds import aliases on unused import with F401 rule and the preview option

---

_@skshetry_

### Description

This requires creating a minimal python module, and add an unused import, which could be reproduced with the following script:

```bash
mkdir -p mylib
echo "from mylib.mod import klass" > mylib/__init__.py
```

```console
$ ruff --version
ruff 0.9.3
$ cat mylib/__init__.py
from mylib.mod import klass⏎
$ ruff check mylib --select F401 --preview --fix --isolated
Found 1 error (1 fixed, 0 remaining).
$ cat mylib/__init__.py
from mylib.mod import klass as klass⏎
```

^ Ruff added a `klass as klass` import alias.


Without `--preview`, I get following error,
```console
$ ruff check mylib --select F401  --fix --isolated
mylib/__init__.py:1:23: F401 `mylib.mod.klass` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
  |
1 | from mylib.mod import klass
  |                       ^^^^^ F401
  |
  = help: Use an explicit re-export: `klass as klass`

Found 1 error.
```


---

_Comment by @dylwil3 on 2025-01-29 10:40_

This is the expected behavior- see the [documentation](https://docs.astral.sh/ruff/rules/unused-import/).

Is the fix unwelcome? That would be good feedback and help when the decision is made on whether to stabilize this behavior.

---

_Label `question` added by @dylwil3 on 2025-01-29 10:41_

---

_Label `fixes` added by @dylwil3 on 2025-01-29 10:41_

---

_Label `preview` added by @dylwil3 on 2025-01-29 10:41_

---

_Comment by @skshetry on 2025-01-29 10:57_

Thank you for the link @dylwil3. I should have read the documentation before creating the issue.
Apologies.

I encountered this issue when I was refactoring and removing some code in `__init__.py` file (it's inside a nested module), that left the imports unused. I expected `ruff` to either complain or remove the unused import, but instead it created an alias, which I found surprising. 

---

_Comment by @dylwil3 on 2025-01-30 17:28_

Thank you for the feedback! Hopefully the explanation in the docs helps make the re-export seem less surprising, or at least justified.

---

_Closed by @dylwil3 on 2025-01-30 17:28_

---

_Comment by @smackesey on 2025-02-20 12:15_

It's unclear to me whether the behavior here is going to be configurable. From the docs:

>Applying fixes to __init__.py files is currently in preview. The fix offered depends on the type of the unused import. Ruff will suggest a safe fix to export first-party imports with either a redundant alias or, if already present in the file, an __all__ entry. If multiple __all__ declarations are present, Ruff will not offer a fix. Ruff will suggest an unsafe fix to remove third-party and standard library imports -- the fix is unsafe because the module's interface changes.

Want to make sure it will be possible to remove all unused imports, all the time, regardless of whether it is a first or third-party import.

---

_Comment by @WilliamStam on 2025-03-24 12:16_

an option to choose if it must alias or use the `__all__ = []` thing would be great

---
