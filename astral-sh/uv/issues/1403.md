```yaml
number: 1403
title: "Installing from local wheel fails when using pip install <path> "
type: issue
state: closed
author: cfculhane
labels: []
assignees: []
created_at: 2024-02-16T00:40:32Z
updated_at: 2024-03-22T21:23:39Z
url: https://github.com/astral-sh/uv/issues/1403
synced_at: 2026-01-10T05:40:31Z
```

# Installing from local wheel fails when using pip install <path> 

---

_Issue opened by @cfculhane on 2024-02-16 00:40_

This is occuring when installing a package with cuda version specified with `+cu117` in the wheel name, e.g. for `detectron2` or `torch`

To repro: 

1. Download wheel, e.g.  botocore from pypi: https://files.pythonhosted.org/packages/74/f8/fb598ee499f19c1532cf47a6eb34c3c20447f9f81e48bb82a017a49bab6a/botocore-1.34.43-py3-none-any.whl
2. `cp botocore-1.34.43-py3-none-any.whl botocore-1.34.43+cu117-py3-none-any.whl`
3. `uv pip install ./botocore-1.34.43+cu117-py3-none-any.whl` # fails with `Expected package name starting with an alphanumeric character`
4. `uv pip install botocore-1.34.43+cu117-py3-none-any.whl` # fails with `URL requirement must be preceded by a package name.`
5. Trying the original package without the +cu117 fails too, `Because botocore-1-34-43-py3-none-any-whl was not found in the package registry and you require botocore-1-34-43-py3-none-any-whl, we can conclude that the requirements are unsatisfiable.`

All of the above works with `pip` just fine, it would be great if it did!

Note that `uv pip install "botocore @ ./botocore-1.34.43+cu117-py3-none-any.whl"` and `uv pip install "botocore @ file:////botocore-1.34.43-py3-none-any.whl"` work just fine, but this is a little unintuitive compared the behaviour from `pip`

---

_Comment by @zanieb on 2024-02-16 00:44_

Thanks for the report. I think this is a duplicate of #313 (I know that one's pretty buried!). Or is it specifically with regards to the `+...` part?

Similar https://github.com/astral-sh/uv/issues/1372

---

_Comment by @cfculhane on 2024-02-16 00:50_

Apoligies, didnt see that (I didnt know a local path counted as a URL dependency, although I guess that makes sense!). The + works fine when the fully specified package @ path is given, so I think this can be closed as duplicate in that case.

---

_Comment by @zanieb on 2024-02-16 00:55_

Okay thanks!

---

_Closed by @zanieb on 2024-02-16 00:55_

---

_Comment by @charliermarsh on 2024-03-22 21:23_

This is supported as of `v0.1.24`. You can `uv pip ./botocore-1.34.43+cu117-py3-none-any.whl` directly, without including a package name.

---
