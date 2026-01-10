---
number: 677
title: "Suppress ANN401 for dynamically typed *args and **kwargs."
type: issue
state: closed
author: tekumara
labels: []
assignees: []
created_at: 2022-11-11T05:09:55Z
updated_at: 2024-05-25T10:12:08Z
url: https://github.com/astral-sh/ruff/issues/677
synced_at: 2026-01-10T01:22:38Z
---

# Suppress ANN401 for dynamically typed *args and **kwargs.

---

_Issue opened by @tekumara on 2022-11-11 05:09_

Now that ANN401 has [landed](https://github.com/charliermarsh/ruff/pull/657), would be nice to support [allow-star-arg-any](https://github.com/sco1/flake8-annotations#--allow-star-arg-any) which is common idiom, eg:
```python
def mocked_fn(*args: Any, **kwargs: Any) -> None:
   ...
```





---

_Comment by @andersk on 2022-11-11 05:19_

This is supported.

```toml
[tool.ruff.flake8-annotations]
allow-star-arg-any = true
```

(Note though that `*args: object, **kwargs: object` or [`ParamSpec`](https://peps.python.org/pep-0612/) is often a safer alternative.)

---

_Comment by @charliermarsh on 2022-11-11 15:28_

Yeah I think this is supported. Let me know if it's not working for you @tekumara!

---

_Closed by @charliermarsh on 2022-11-11 15:28_

---

_Comment by @sanmai-NL on 2024-05-25 10:12_

See: https://github.com/astral-sh/ruff/issues/5803#issuecomment-1640820274

---

_Referenced in [ansible/event-driven-ansible#429](../../ansible/event-driven-ansible/pulls/429.md) on 2025-05-20 11:33_

---

_Referenced in [astral-sh/ruff#19152](../../astral-sh/ruff/issues/19152.md) on 2025-07-05 01:42_

---

_Referenced in [astral-sh/ruff#20644](../../astral-sh/ruff/issues/20644.md) on 2025-09-30 09:33_

---
