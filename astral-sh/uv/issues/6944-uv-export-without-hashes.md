---
number: 6944
title: uv export --without-hashes
type: issue
state: closed
author: maxfirman
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2024-09-02T20:38:55Z
updated_at: 2024-09-03T02:12:30Z
url: https://github.com/astral-sh/uv/issues/6944
synced_at: 2026-01-10T01:24:08Z
---

# uv export --without-hashes

---

_Issue opened by @maxfirman on 2024-09-02 20:38_

Please could you add a `--without-hashes` option to `uv export` similar to [`pdm export`](https://pdm-project.org/latest/reference/cli/#export)

---

_Comment by @charliermarsh on 2024-09-02 20:53_

What's the motivation for excluding hashes?

---

_Label `configuration` added by @charliermarsh on 2024-09-02 21:11_

---

_Label `needs-decision` added by @charliermarsh on 2024-09-02 21:11_

---

_Comment by @maxfirman on 2024-09-02 21:11_

I'm trying to migrate an existing process. There is a CI pipeline stage which installs dependencies with a requirements.txt which fails with:

```
ERROR: In --require-hashes mode, all requirements must have their versions pinned with ==. These do not:
    sqlglot[rs] from .....
```

Previously I was generating the requirements.txt using `pdm export --without-hashes` and this was working. I'm not 100% sure why it doesn't like the hashes. I'm guessing it could be related to the order in which the index and extra-index-urls are specified, perhaps there is a different index resolution order between the environments where I ran `uv lock`.

Unfortunately I don't own the code in the ci pipeline stage, otherwise I would just change it to use `uv sync`


---

_Comment by @maxfirman on 2024-09-02 21:16_

I wonder whether this could be mitigated if `uv export` were to include the index-url and extra-index-url in the requirements.txt. I know that `pip-compile` does this. I will test this theory.

---

_Comment by @charliermarsh on 2024-09-02 21:18_

Thanks. Is that error coming from a `pip install` call or a `uv` call?

---

_Comment by @maxfirman on 2024-09-02 21:21_

`pip install`

---

_Comment by @charliermarsh on 2024-09-03 00:45_

I suppose it's reasonable to allow this given that pip doesn't have a way to turn it off.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-03 00:45_

---

_Referenced in [astral-sh/uv#6954](../../astral-sh/uv/pulls/6954.md) on 2024-09-03 02:04_

---

_Closed by @charliermarsh on 2024-09-03 02:12_

---
