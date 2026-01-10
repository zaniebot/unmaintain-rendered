```yaml
number: 1744
title: "typing_extensions.Literal reimport is not \"detected\""
type: issue
state: closed
author: ashb
labels:
  - bug
assignees: []
created_at: 2023-01-09T12:47:44Z
updated_at: 2023-01-10T04:27:08Z
url: https://github.com/astral-sh/ruff/issues/1744
synced_at: 2026-01-10T11:09:43Z
```

# typing_extensions.Literal reimport is not "detected"

---

_Issue opened by @ashb on 2023-01-09 12:47_

As part of the Airflow project we have an `airflow.typing_compat` module that has this in it https://github.com/apache/airflow/blob/71306b31f1842ee2b1eb1cc2980b90f0fb6b11dc/airflow/typing_compat.py#L38-L41:

```python
if sys.version_info >= (3, 9):
    from typing import Literal
else:
    from typing_extensions import Literal
```

Ruff right now doesn't detect this use of Literal and tries to resolve the value inside it as a type:

```bash
% echo 'from airflow.typing_compat import Literal; from typing import Union; X = Union[Literal[False], Literal["db"]]' | \
  ruff - --line-length=999
-:1:104: F821 Undefined name `db`
```

---

_Label `bug` added by @charliermarsh on 2023-01-09 15:28_

---

_Comment by @charliermarsh on 2023-01-09 16:16_

The problem here is that we don't track imports across modules, so we can't tell (right now) that `Literal` there is coming from `typing` or `typing_extensions`.

I could see two ways to fix this in the short term:

1. I could special-case `airflow.typing_compat` within Ruff for now. It's easy to do on my end, though obviously a bit awkward :)
2. We could add a configuration option to treat a module as a `typing` re-exporter. The pattern you have here seems somewhat general, so this seems like a reasonable thing to have.


---

_Comment by @charliermarsh on 2023-01-09 17:01_

E.g., would something like this work for Airflow?

```toml
[tool.ruff]
typing-exports = ["airflow.typing_compat"]
```

---

_Comment by @ashb on 2023-01-09 19:09_

> E.g., would something like this work for Airflow?
> 
> ```toml
> [tool.ruff]
> typing-exports = ["airflow.typing_compat"]
> ```

Yup that would do it for us

---

_Comment by @charliermarsh on 2023-01-09 19:21_

Cool, I'll ship that for you today.

---

_Closed by @charliermarsh on 2023-01-09 23:17_

---

_Comment by @charliermarsh on 2023-01-10 04:27_

Going out in [v0.0.217](https://github.com/charliermarsh/ruff/releases/tag/v0.0.217).

---
