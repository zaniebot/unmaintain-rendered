```yaml
number: 10516
title: Mypy (type) directives no longer relevant after running isort check
type: issue
state: open
author: jaraco
labels:
  - bug
  - fixes
assignees: []
created_at: 2024-03-21T20:56:06Z
updated_at: 2025-07-05T00:34:34Z
url: https://github.com/astral-sh/ruff/issues/10516
synced_at: 2026-01-12T15:54:50Z
```

# Mypy (type) directives no longer relevant after running isort check

---

_@jaraco_

I'm struggling to create a minimal example, so bear with this more complicated one:

In the [keyring](/jaraco/keyring) project, I'm exploring introducing the isort ("I") linter. When I add it and invoke it on the project, it produces a diff in the `keyring/_compat.py` module:

```shell
 keyring main @ ruff check --select I --fix keyring/_compat.py
Found 1 error (1 fixed, 0 remaining).
```

```diff
 keyring main @ git diff
diff --git a/keyring/_compat.py b/keyring/_compat.py
index 46868a8..7d7c8fa 100644
--- a/keyring/_compat.py
+++ b/keyring/_compat.py
@@ -4,4 +4,6 @@ __all__ = ['properties']
 try:
     from jaraco.classes import properties  # pragma: no-cover
 except ImportError:
-    from . import _properties_compat as properties  # type: ignore[no-redef] # pragma: no-cover
+    from . import (
+        _properties_compat as properties,  # type: ignore[no-redef] # pragma: no-cover
+    )
```

After making that change, the mypy tests start failing:

```
 keyring main @ tox -qq --notest
 keyring main @ .tox/py/bin/mypy keyring/_compat.py
keyring/_compat.py:7: error: Name "properties" already defined (by an import)  [no-redef]
Found 1 error in 1 file (checked 1 source file)
```

Note that running mypy from another environment will not suffice - you need mypy in the environment where keyring was installed in order to get the right dependencies and stubs.

Moving the `# type: ignore[no-redef]` directive to the line above fixes the issue:

```diff
 keyring main @ git diff
diff --git a/keyring/_compat.py b/keyring/_compat.py
index 46868a8..c06f247 100644
--- a/keyring/_compat.py
+++ b/keyring/_compat.py
@@ -4,4 +4,6 @@ __all__ = ['properties']
 try:
     from jaraco.classes import properties  # pragma: no-cover
 except ImportError:
-    from . import _properties_compat as properties  # type: ignore[no-redef] # pragma: no-cover
+    from . import (  # type: ignore[no-redef]
+        _properties_compat as properties,  # pragma: no-cover
+    )
```

```shell
 keyring main @ .tox/py/bin/mypy keyring/_compat.py
Success: no issues found in 1 source file
```

Relatedly, note also that the coverage directive, which was previously not well placed, also now misses line 7 where it previously exempted it.

What I expect is that an isort `--fix` should retain the semantic meaning of directives in comments, or at least protect the rewrite in an "unsafe" fixer.

---

_Comment by @zanieb on 2024-03-21 21:59_

Thanks for the clear write-up.

I worry about the behavior of `mypy` here, the required placement of the suppression comment seems bizarre and inconsistent with suppression comments in other tools. I would be curious to see if they are receptive to supporting a suppression comment in either position (the earlier position applying to the full `import` expression and the latter to the single line).

Regardless I agree we should not be breaking this. Without further investigation, I think it would be reasonable to mark the fix as unsafe if we detect suppression comments.



---

_Label `bug` added by @zanieb on 2024-03-21 21:59_

---

_Label `fixes` added by @zanieb on 2024-03-21 21:59_

---

_Comment by @MichaReiser on 2024-03-22 07:54_

@zanieb I think we shouldn't look at this issue in isolation of `isort` because the same happens if you format the above code ([Playground](https://play.ruff.rs/6dff7d71-2b28-4a6d-9703-20b6dbc76125)). Adding special handling for isort only would limit our options when it comes to integrating `isort` into the formatter. 

For the formatter, pragma comments are handled differently in that they are excluded from the line-width computation. However, we decided not to implement any special placement logic for pragma comments (or suppress formatting) because a) the correct placement depends on the specific pragma comment, e.g. `type: ignore` and a `noqa` comment might both need a different placement, and b) disabling formatting for lines ending with pragma comments no longer delivers on the key value proposition, consistent formatting of your code (see https://github.com/astral-sh/ruff/discussions/6670). 

I'm open to suggestions for improving pragma comment placement, but I fear moving some pragma comments manually might be necessary. This is inherent to the design decision to encode semantic meaning into comments with no grammar rules. 

---

_Comment by @AlexWaygood on 2024-03-22 09:47_

Possibly mypy requires the `type: ignore` comment to be on the first line of the whole import statement due to the fact that `ast.alias` nodes in Python's AST (as provided by the stdlib `ast` module) only have `lineno` and `col_offset` attributes on 3.10+. So on older Python versions, mypy likely knows the lineno of the `type: ignore`, and it knows the lineno of the start of the import statement as a whole, but it doesn't actually know the lineno of the specific import in the multiline import statement that it deems as problematic 

---

_Comment by @dpoznik on 2024-05-08 17:39_

I've run into this issue as well and figured I'd post my example in case helpful:
```
$ cat foo.py 
from metaflow import batch  # type: ignore
from metaflow import environment  # type: ignore
from metaflow import resources  # type: ignore
from metaflow import Flow, Run, S3, Step, Task, step
$ isort foo.py 
$ python foo.py 
$ mypy foo.py 
Success: no issues found in 1 source file

$ ruff check --select I --fix foo.py
Found 1 error (1 fixed, 0 remaining).
$ cat foo.py
from metaflow import (
    Flow,
    Run,
    S3,
    Step,
    Task,
    batch,  # type: ignore
    environment,  # type: ignore
    resources,  # type: ignore
    step,
)
$ mypy foo.py
foo.py:1: error: Module "metaflow" has no attribute "batch"  [attr-defined]
foo.py:1: error: Module "metaflow" has no attribute "environment"  [attr-defined]
foo.py:1: error: Module "metaflow" has no attribute "resources"  [attr-defined]
Found 3 errors in 1 file (checked 1 source file)
```
As indicated in the Issue description, removing the three specific `# type: ignore` directives and adding one to the first line resolves the issue, albeit with slightly altered `mypy` inspection.

---

_Comment by @QuLogic on 2025-07-05 00:34_

I'm not sure if it's exactly the same issue, but one I've run into is:
```
from matplotlib.backends.qt_compat import QT_API
from matplotlib.backends.qt_compat import QtCore, QtGui  # type: ignore[attr-defined]
```
Since `qt_compat` imports any of PyQt/PySide 5/6, there are no type hints for it. There is, however, a type hint for `QT_API`. So I do want those to be separate, but `ruff` combines them into:
```
from matplotlib.backends.qt_compat import (  # type: ignore[attr-defined]
    QT_API,
    QtCore,
    QtGui,
)
```
which does not appear problematic at first glance.

However, it is technically different, since now `mypy` will not try to verify the `QT_API` attribute. And `pyright` no longer sees the comment and errors out on the "unknown" symbols.

---
