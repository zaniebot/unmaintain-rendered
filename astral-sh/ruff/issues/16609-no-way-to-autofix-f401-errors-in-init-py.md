```yaml
number: 16609
title: No way to autofix F401 errors in __init__.py
type: issue
state: closed
author: ezyang
labels:
  - question
assignees: []
created_at: 2025-03-10T18:56:48Z
updated_at: 2025-03-21T23:01:34Z
url: https://github.com/astral-sh/ruff/issues/16609
synced_at: 2026-01-12T15:54:55Z
```

# No way to autofix F401 errors in __init__.py

---

_@ezyang_

### Summary

Repro:

__init__.py
```
from a import foo

def baz() -> None:
  pass
```

a.py
```
def foo():
  pass
```

```
ruff check --fix --unsafe-fixes --select ALL
```

The unused import is not removed. Now, I assume this is special cased because `__init__.py` is special. It would be nice if there a way to bypass the special case anyway though. For now, though, I think I'll get my code out of `__init__.py`...

As a stopgap, it would be nice if it said something about why the import was not fixed here, this was very difficult to Google for why it was not fixing it since everyone claimed unused imports would be removed by this.

### Version

ruff 0.9.10

---

_Comment by @ntBre on 2025-03-10 22:44_

This fix is only available with [preview mode](https://docs.astral.sh/ruff/preview/) enabled:

```shell
> cat __init__.py
from a import foo


def baz() -> None:
  pass
```

```shell
> ruff check __init__.py --diff --select F401 --unsafe-fixes --preview
--- __init__.py
+++ __init__.py
@@ -1,4 +1,3 @@
-from a import foo


 def baz() -> None:

Would fix 1 error.
```

This seems to work for me, even for `__init__.py`, which was a pleasant surprise since there are several existing issues around F401 for `__init__.py` files.

---

_Label `question` added by @MichaReiser on 2025-03-11 06:49_

---

_Comment by @vikyd on 2025-03-12 11:16_

https://docs.astral.sh/ruff/tutorial/

`--preview` `--unsafe-fixes`  are not mentioned in the tutorial page.

---

_Comment by @MichaReiser on 2025-03-12 11:55_

> [docs.astral.sh/ruff/tutorial](https://docs.astral.sh/ruff/tutorial/)
> 
> `--preview` `--unsafe-fixes` are not mentioned in the tutorial page.

This is intentional. The tutorial should cover the bascis. Both `--preview` and `--unsafe-fixes` are more advanced concepts.

---

_Closed by @charliermarsh on 2025-03-13 00:50_

---

_Comment by @vikyd on 2025-03-21 06:28_

But it will fail when copying and running command exactly in pag https://docs.astral.sh/ruff/tutorial/ .

It is not about intentional or not.

---

_Comment by @dhruvmanila on 2025-03-21 09:54_

> But it will fail when copying and running command exactly in pag [docs.astral.sh/ruff/tutorial](https://docs.astral.sh/ruff/tutorial/) .

Can you specify which command will fail from the tutorial docs?

---

_Comment by @ezyang on 2025-03-21 12:48_

Now that I know about preview, I guess I will try it the next time I have this sort of problem. Difficult to know how I could have figured it out without filing this issue though lol

---

_Comment by @ntBre on 2025-03-21 13:12_

The tutorial was updated in #16818 to avoid fixing `__init__.py`. The new version should now allow you to follow along directly!

---

_Comment by @ntBre on 2025-03-21 13:16_

@ezyang I see now that your report doesn't mention the tutorial explicitly. In your case, do the docs for the rule itself ([unused-import (F401)](https://docs.astral.sh/ruff/rules/unused-import/#unused-import-f401)) clarify things? There's some discussion of `__init__.py` and the preview behavior in the `Fix safety` section.

---

_Comment by @ezyang on 2025-03-21 23:01_

If I had thought to look at the docs, this indeed would have answered it (although "currently in preview" would not be meaningful to someone who didn't know about the preview flag). But it would have been better if the lint message itself mentioned that "although you had fixes on, this lint wasn't fixed because of" and then link to URL.

---
