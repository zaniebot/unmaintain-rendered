```yaml
number: 3052
title: "`install --require-hashes`'s \"must be pinned upfront\" error should say where the requirement comes from"
type: issue
state: open
author: bluetech
labels:
  - error messages
assignees: []
created_at: 2024-04-16T07:12:45Z
updated_at: 2024-04-16T13:20:01Z
url: https://github.com/astral-sh/uv/issues/3052
synced_at: 2026-01-10T05:40:32Z
```

# `install --require-hashes`'s "must be pinned upfront" error should say where the requirement comes from

---

_Issue opened by @bluetech on 2024-04-16 07:12_

Using `--require-hashes` requires that all requirements including transitive are pinned with `==`. When a transitive requirement is missing, `uv pip install` emits this error:

```
error: In `--require-hashes` mode, all requirements must be pinned upfront with `==`, but found: setuptools
```

For comparison, `pip install` emits this error:

```
ERROR: In --require-hashes mode, all requirements must have their versions pinned with ==. These do not:
    setuptools from https://files.pythonhosted.org/packages/f7/29/13965af254e3373bceae8fb9a0e6ea0d0e571171b80d6646932131d6439b/setuptools-69.5.1-py3-none-any.whl (from zope.event==5.0->-r requirements.txt (line 6))
```

The pip error is more helpful: I have some projects where I don't use `uv pip sync` (yet?), instead I just pin all requirements myself. I rely on the `pip install` error to tell me which transitive dep I'm missing, and the `(from zope.event==5.0->-r requirements.txt (line 6))` part is very helpful for figuring out where the requirements comes from.

Maybe the way I do it is a bit silly and redundant with `sync` but I figure a better error message doesn't hurt anyway :)

Note: the "setuptools from <url>" part I don't personally care about.

---

Version: uv 0.1.32

---

_Comment by @bluetech on 2024-04-16 07:15_

> Note: the "setuptools from " part I don't personally care about.

Correction: the "69.5.1" part (the version of the missing transitive dep) *is* helpful, it lets me know to which version I should pin the dep when I add it to the requirements.

---

_Label `error messages` added by @charliermarsh on 2024-04-16 13:17_

---

_Comment by @charliermarsh on 2024-04-16 13:20_

Makes sense.

---
