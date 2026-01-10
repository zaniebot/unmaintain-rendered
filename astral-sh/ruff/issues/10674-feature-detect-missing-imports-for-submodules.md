```yaml
number: 10674
title: "feature: detect missing imports for submodules"
type: issue
state: closed
author: jaraco
labels:
  - question
assignees: []
created_at: 2024-03-30T18:52:32Z
updated_at: 2024-04-01T02:03:31Z
url: https://github.com/astral-sh/ruff/issues/10674
synced_at: 2026-01-10T11:09:53Z
```

# feature: detect missing imports for submodules

---

_Issue opened by @jaraco on 2024-03-30 18:52_

In #10673, I stumbled on a piece of lint that if detected would have helped me avoid trouble when using isort. Although ruff does detect missing imports for top-level modules, it appears not to do so for submodules. Consider this simple example:

```
 draft @ cat > mod.py
import urllib


class MyRequest(urllib.request.Request):
  pass
 draft @ ruff check mod.py
All checks passed!
 draft @ py -m mod
Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/Users/jaraco/draft/mod.py", line 4, in <module>
    class MyRequest(urllib.request.Request):
                    ^^^^^^^^^^^^^^
AttributeError: module 'urllib' has no attribute 'request'
 draft @ sed -i -e 's/import urllib/import urllib.request/' mod.py
 draft @ py -m mod && echo done
done
```

It's my understanding that best practice dictates the following:

- If the top level module is used directly, it should be imported separately.
- All submodules referenced in the code should get explicit imports.

So if one uses `os.getcwd()`, they should `import os`, and if they use `os.path.join`, they should `import os.path`, even if `import os` implicitly imports `os.path`.

I can't currently find the reference where this question was answered by the Python Core Devs, but I do recall that it contradicted my interpretation of the documentation (which was that it's fine to let the implied import suffice in both cases).

I'm not sure how tricky it could be to detect these missing imports (as a submodule name could be a separate module or could be a name programmatically added to the parent module), but I file the issue here for consideration anyway.

---

_Comment by @jaraco on 2024-03-30 19:01_

I see now my current understanding is based on a mixed opinion from core devs in a non-public chat:

![image](https://github.com/astral-sh/ruff/assets/308610/f6112f9f-4d62-427a-b01f-8bff51194e70)

There's definitely a [mixed opinion in the community as well](https://stackoverflow.com/q/2724348/70170), so it's not obvious that ruff could enforce one pattern or another, even with the technical challenges aside.

---

_Comment by @charliermarsh on 2024-03-30 23:12_

A few related issues from the past:

- https://github.com/astral-sh/ruff/issues/9193
- https://github.com/astral-sh/ruff/issues/4656
- https://github.com/astral-sh/ruff/issues/60

---

_Comment by @charliermarsh on 2024-04-01 02:03_

Thank you for the clear issue!

Some background: in Ruff's semantic model, we treat `import os` and `import os.path` as _roughly_ equivalent, with a few exceptions (e.g., `import os; import os` will lead to a "redefined while unused" diagnostic, while `import os; import os.path` will not). So, e.g., `import os` allows you to access `os.getcwd() ` or `os.path.join`. Similarly, `import os.path` allows you to access `os.getcwd()` or `os.path.join`.

This matches Pyflakes' approach and tends to yield reasonable behavior _most of_ the time. `urllib` is the most common exception (as seen here), and we've considered special-casing it before since it's in the standard library...

I think we're unlikely to change the high-level rules around import _norms_ in the near future, since it would be fairly disruptive and I don't see consensus in the community. However, I'd like to fix cases in which it's _actually_ wrong (e.g., don't remove `from urllib import request` when it's necessary) once we've extended our semantic model to support analyzing these imports across files.

So, for now, I'm gonna close this as "working as intended" and track correctness fixes in https://github.com/astral-sh/ruff/issues/9193.


---

_Closed by @charliermarsh on 2024-04-01 02:03_

---

_Label `question` added by @charliermarsh on 2024-04-01 02:03_

---
