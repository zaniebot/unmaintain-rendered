```yaml
number: 9691
title: Add hint on build backend import with preview disabled
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/hint-backend
created_at: 2024-12-06T17:23:03Z
updated_at: 2024-12-06T20:08:03Z
url: https://github.com/astral-sh/uv/pull/9691
synced_at: 2026-01-10T12:00:01Z
```

# Add hint on build backend import with preview disabled

---

_Pull request opened by @zanieb on 2024-12-06 17:23_

See #9686

```
❯ uv run python -c "import uv; uv.build_sdist"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/Users/zb/workspace/uv/python/uv/__init__.py", line 45, in __getattr__
    raise AttributeError(err)
AttributeError: Using `uv.build_sdist` is not allowed. The uv build backend requires preview mode to be 
                enabled, e.g., via the `UV_PREVIEW=1` environment variable.

❯ uv run python -c "import uv; uv.foo"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/Users/zb/workspace/uv/python/uv/__init__.py", line 48, in __getattr__
    raise AttributeError(err)
AttributeError: module 'uv' has no attribute 'foo'

❯ uv run python -c "import uv; uv.find_uv_bin"
```

---

_Label `preview` added by @zanieb on 2024-12-06 17:23_

---

_Review comment by @konstin on `python/uv/__init__.py`:44 on 2024-12-06 17:36_

If you're hitting this branch, you need `UV_PREVIEW=1`, that's e.g. with `python -m build`. For `uv build`, you usually want `--preview` which goes through the direct path, except for `uv build --force-pep517`, in which case you need `UV_PREVIEW=1`, which is why I had just put "Use `UV_PREVIEW=1` and `--preview`" on top of the docs.

We could also set preview through PEP 517 config settings, but that seems even worse.

---

_@konstin approved on 2024-12-06 17:36_

---

_Marked ready for review by @zanieb on 2024-12-06 19:33_

---

_@zanieb reviewed on 2024-12-06 19:33_

---

_Review comment by @zanieb on `python/uv/__init__.py`:44 on 2024-12-06 19:33_

Just to confirm, is this asking for a change to what I wrote here?

---

_@konstin reviewed on 2024-12-06 19:59_

---

_Review comment by @konstin on `python/uv/__init__.py`:44 on 2024-12-06 19:59_

I was only adding context on why this preview feature is complicated beyond the usual "Add `--preview` to use".

---

_@zanieb reviewed on 2024-12-06 20:07_

---

_Review comment by @zanieb on `python/uv/__init__.py`:44 on 2024-12-06 20:07_

👍 sounds good, that makes sense to me.

---

_Merged by @zanieb on 2024-12-06 20:08_

---

_Closed by @zanieb on 2024-12-06 20:08_

---

_Branch deleted on 2024-12-06 20:08_

---
