---
number: 10673
title: Invoking isort can break circular imports
type: issue
state: closed
author: jaraco
labels:
  - isort
assignees: []
created_at: 2024-03-30T18:30:34Z
updated_at: 2024-03-30T18:48:27Z
url: https://github.com/astral-sh/ruff/issues/10673
synced_at: 2026-01-07T13:12:15-06:00
---

# Invoking isort can break circular imports

---

_Issue opened by @jaraco on 2024-03-30 18:30_

In the [cssutils project](/jaraco/cssutils), I learned that running isort fixers can break the code:

```
 cssutils ead999fc @ ruff --version
ruff 0.3.4
 cssutils ead999fc @ ruff check --fix --select I
Found 85 errors (85 fixed, 0 remaining).
 cssutils ead999fc @ tox -q
ImportError while loading conftest '/Users/jaraco/code/jaraco/cssutils/conftest.py'.
conftest.py:5: in <module>
    import cssutils
cssutils/__init__.py:79: in <module>
    from . import css, errorhandler, stylesheets
cssutils/css/__init__.py:61: in <module>
    from .csscharsetrule import CSSCharsetRule
cssutils/css/csscharsetrule.py:10: in <module>
    from . import cssrule
cssutils/css/cssrule.py:10: in <module>
    class CSSRule(cssutils.util.Base2):
E   AttributeError: partially initialized module 'cssutils' has no attribute 'util' (most likely due to a circular import)
py: exit 4 (0.17 seconds) /Users/jaraco/code/jaraco/cssutils> pytest pid=8630
  py: FAIL code 4 (1.67 seconds)
  evaluation failed :( (1.71 seconds)
```
```diff
 cssutils ead999fc @ git diff cssutils/__init__.py
diff --git a/cssutils/__init__.py b/cssutils/__init__.py
index 37c54894..38d22353 100644
--- a/cssutils/__init__.py
+++ b/cssutils/__init__.py
@@ -69,20 +69,17 @@ Example::
 
 """
 
+import functools
+import itertools
 import os.path
-import urllib.request
 import urllib.parse
+import urllib.request
 import xml.dom
-import itertools
-import functools
 
-from . import errorhandler
-from . import css
-from . import stylesheets
+from . import css, errorhandler, stylesheets
 from .parse import CSSParser
-from .serialize import CSSSerializer
 from .profiles import Profiles
-
+from .serialize import CSSSerializer
 
 __all__ = ['css', 'stylesheets', 'CSSParser', 'CSSSerializer']
 ```

In cssutils, the imports are sensitive to order to avoid circular imports, but isort prefers alphabetical irrespective of the dependency graph.

I presumed that because I didn't pass `--unsafe-fixes`, the fixes would be guaranteed safe, but it seems they're not.

I suspect what needs to be done here is I need to invest some time in understanding how the isort actions work and how to apply them to the cssutils codebase to allow for sorting without breaking the package.

I suppose if ruff was really ambitious, it could analyze the imports and detect circular imports and try to solve the import order for a viable order (keeping stability otherwise).

---

_Label `isort` added by @AlexWaygood on 2024-03-30 18:31_

---

_Comment by @jaraco on 2024-03-30 18:38_

Oh! The error message from Python was misleading. The issue isn't a circular import, but the fact that `cssutils.util` was never imported (it must have been implicitly imported elsewhere prior to the isort operation). Making sure that import was made (jaraco/cssutils@4fd6bbe3) prior to running isort works around the issue.

---

_Comment by @jaraco on 2024-03-30 18:41_

So maybe this issue should be closed as wontfix, at least until such a time that someone else encounters the issue in another real-world case. I wonder if this finding is indicative of a missing linter - should ruff be checking for a missing import of a submodule? I'll file a separate ticket for that (after searching).

---

_Closed by @charliermarsh on 2024-03-30 18:48_

---

_Referenced in [astral-sh/ruff#10674](../../astral-sh/ruff/issues/10674.md) on 2024-03-30 18:52_

---
