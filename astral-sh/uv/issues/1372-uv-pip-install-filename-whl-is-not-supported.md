```yaml
number: 1372
title: "`uv pip install filename.whl` is not supported"
type: issue
state: closed
author: hauntsaninja
labels:
  - question
assignees: []
created_at: 2024-02-15T22:28:47Z
updated_at: 2025-04-04T23:27:36Z
url: https://github.com/astral-sh/uv/issues/1372
synced_at: 2026-01-12T15:58:26Z
```

# `uv pip install filename.whl` is not supported

---

_@hauntsaninja_

If you have a wheel file floating around, pip gives you a way to install it. Is there a way to do this in uv?

Context: we have some custom packaging code at work, currently in some cases we end up doing `pip install --no-deps firstparty1.whl firstparty2.whl firstparty3.whl` and it would be nice for this to be much faster than it is with pip

---

_Comment by @zanieb on 2024-02-15 22:35_

Yes this is possible, you need to provide a name for the packages though

```
❯ python3.12 -m packse build-pkg example 0.1.0 --rm
Generated project for 'example-0.1.0'in 5.43ms
Built package 'example-0.1.0' in 0.20s
Linked distribution to dist/example/example-0.1.0-py3-none-any.whl
Linked distribution to dist/example/example-0.1.0.tar.gz
Done in 203.84ms

❯ uv pip install example@./dist/example/example-0.1.0-py3-none-any.whl
Resolved 1 package in 8ms
Downloaded 1 package in 3ms
Installed 1 package in 1ms
 + example==0.1.0 (from file:///Users/mz/eng/src/astral-sh/uv/dist/example/example-0.1.0-py3-none-any.whl)

```

---

_Label `question` added by @zanieb on 2024-02-15 22:36_

---

_Comment by @hauntsaninja on 2024-02-15 22:41_

Thank you! Yeah I tried to guess that but then got the error in https://github.com/astral-sh/uv/issues/1374

---

_Closed by @hauntsaninja on 2024-02-15 22:41_

---

_Comment by @carlosperate on 2024-02-15 23:54_

Could this be a feature request? to be able to install the wheel without having to specify the name?
Mostly to keep feature parity with pip and be able to be a drop-in replacement.

---

_Comment by @zanieb on 2024-02-16 00:25_

This mismatch in behavior is actually intentional because it was hard to implement, but we plan to do it. See https://github.com/astral-sh/uv/issues/313 for the tracking issue.

---

_Comment by @charliermarsh on 2024-03-22 21:24_

This is supported as of `v0.1.24`. You can `uv pip ./filename.whl ` directly, without including a package name.


---

_Comment by @jovezhong on 2025-04-04 23:27_

`uv pip install ./filename.whl` actually

---
