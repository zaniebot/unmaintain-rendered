```yaml
number: 4489
title: "Should `__main__.py` really be considered a public module?"
type: issue
state: closed
author: CobaltCause
labels:
  - docstring
assignees: []
created_at: 2023-05-18T05:36:11Z
updated_at: 2024-01-18T01:24:55Z
url: https://github.com/astral-sh/ruff/issues/4489
synced_at: 2026-01-12T15:54:44Z
```

# Should `__main__.py` really be considered a public module?

---

_@CobaltCause_

Reading [this](https://docs.python.org/3/library/__main__.html#main-py-in-python-packages), I wouldn't think so; it seems to imply that nothing should be defined in that file and then imported elsewhere. Yet, `ruff` will give me such errors as `D100 Missing docstring in public module` in `__main__.py`, which I found surprising. I'm not entirely certain which behavior is correct, though.

---

_Comment by @JonathanPlasse on 2023-05-18 14:42_

According to `pydocstyle` [implementation](https://github.com/PyCQA/pydocstyle/blob/07f6707e2c5612960347f7c00125620457f490a7/src/pydocstyle/parser.py#L157-L185), `__main__.py` is public.

---

_Comment by @CobaltCause on 2023-05-18 14:49_

Looking at `git blame`, that code hasn't been touched since 2020-09-05. Clicking through the version history on the page I linked in the issue description, `__main__.py` was added in Python 3.10 which was released on 2021-10-04. So I think `pydocstyle` simply doesn't account for this, since that code was written before this feature existed. Maybe I should open an issue over there as well?

---

_Comment by @charliermarsh on 2023-05-18 14:59_

I think it's worth asking over there, even just to get an opinion (regardless of whether it's changed in `pydocstyle`).

---

_Label `question` added by @charliermarsh on 2023-05-18 14:59_

---

_Label `docstring` added by @charliermarsh on 2023-05-18 14:59_

---

_Comment by @CobaltCause on 2023-05-18 17:02_

I've opened https://github.com/PyCQA/pydocstyle/issues/644

---

_Comment by @charliermarsh on 2023-05-18 17:02_

Thanks :)

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:13_

---

_Comment by @charliermarsh on 2023-07-10 01:13_

I'm gonna close for now to keep the issue tracker actionable.

---

_Closed by @charliermarsh on 2023-07-10 01:13_

---

_Comment by @CobaltCause on 2024-01-18 01:24_

Looks like pydocstyle was deprecated in favor of ruff somewhat recently

---
