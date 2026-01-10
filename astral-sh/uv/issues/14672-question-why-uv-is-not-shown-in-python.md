---
number: 14672
title: "Question: why `uv` is not shown in Python documentation"
type: issue
state: open
author: PaulVe2024
labels:
  - question
assignees: []
created_at: 2025-07-16T21:25:40Z
updated_at: 2025-07-18T07:00:21Z
url: https://github.com/astral-sh/uv/issues/14672
synced_at: 2026-01-10T01:25:47Z
---

# Question: why `uv` is not shown in Python documentation

---

_Issue opened by @PaulVe2024 on 2025-07-16 21:25_

### Question

Question: why `uv` is not shown in https://packaging.python.org/en/latest/guides/writing-pyproject-toml/ ?

If it is modern and maintained tool, it should be listed with others.

Also should be here
https://packaging.python.org/en/latest/tutorials/managing-dependencies/#other-tools-for-application-dependency-management

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @PaulVe2024 on 2025-07-16 21:25_

---

_Comment by @InSyncWithFoo on 2025-07-17 06:11_

The source code for that site can be found at [@pypa/packaging.python.org](https://github.com/pypa/packaging.python.org). You can submit a PR to change it yourself.

---

_Comment by @hoechenberger on 2025-07-17 21:19_

There's a PR here:

https://github.com/pypa/packaging.python.org/pull/1880

---

_Comment by @InSyncWithFoo on 2025-07-17 22:10_

@hoechenberger That one only adds uv's build backend to the list of build backends that support [PEP 639](https://peps.python.org/pep-0639/).

---

_Comment by @hoechenberger on 2025-07-18 04:35_

You're right. But maybe it can serve as an example 😃

---

_Comment by @cbrnr on 2025-07-18 07:00_

I've modified the docs in that PR. Any other locations you would like to see uv?

---

_Referenced in [astral-sh/uv#14794](../../astral-sh/uv/issues/14794.md) on 2025-07-21 16:12_

---

_Referenced in [huggingface/transformers#40631](../../huggingface/transformers/pulls/40631.md) on 2025-09-08 10:34_

---
