```yaml
number: 2752
title: "Support pip's --target"
type: issue
state: closed
author: henryiii
labels:
  - duplicate
assignees: []
created_at: 2024-04-01T03:53:37Z
updated_at: 2024-04-02T08:31:10Z
url: https://github.com/astral-sh/uv/issues/2752
synced_at: 2026-01-12T15:58:40Z
```

# Support pip's --target

---

_@henryiii_

The recipe for making a Python zipapp is something like this:

```console
$ mkdir tmp
$ pip install --no-compile --target=tmp . dep_1 dep_2
$ cd tmpdir
$ python -m zipapp --compress --python=/usr/bin/env python3 --main=myapp.__main__:main --output=../myapp.pyz
```

However, uv is missing the `--target` option from pip. It would also be nice (for implementing this in a cross-installer way, inside something like nox) if `--no-compile` as a no-op in UV, rather than an error.

---

_Comment by @zanieb on 2024-04-01 14:32_

This is a duplicate of https://github.com/astral-sh/uv/issues/1517, could you chime in there instead?

---

_Closed by @zanieb on 2024-04-01 14:32_

---

_Label `duplicate` added by @zanieb on 2024-04-01 14:32_

---

_Comment by @zanieb on 2024-04-01 14:33_

Regarding `--no-compile` we can definitely do that we have other arguments like that, that would be better tracked in a separate issue.

---

_Comment by @henryiii on 2024-04-02 08:31_

I thought I searched - okay, thanks, did that. Also opened #2771 for `--no-compile`.

---
