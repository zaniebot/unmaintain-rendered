```yaml
number: 5095
title: "uv can't read pyproject.toml with extra-index-url"
type: issue
state: closed
author: kimdwkimdw
labels: []
assignees: []
created_at: 2024-07-16T05:25:11Z
updated_at: 2024-07-16T13:24:35Z
url: https://github.com/astral-sh/uv/issues/5095
synced_at: 2026-01-12T15:58:54Z
```

# uv can't read pyproject.toml with extra-index-url

---

_@kimdwkimdw_

with pyproject.toml below, uv can't read pyproject.toml correctly.
```
[tool.uv.pip]
index-url = "https://pypi.org/simple"
extra-index-url = "https://pypi.custom.domain"
```

always shows warning
```
warning: Failed to parse `pyproject.toml` during settings discovery; skipping...
```

uv 0.2.25 in ubuntu 22.04

---

_Comment by @j178 on 2024-07-16 05:28_

`extra-index-url` should be an array:

```toml
[tool.uv.pip]
index-url = "https://pypi.org/simple"
extra-index-url = ["https://pypi.custom.domain"]

```

---

_Comment by @kimdwkimdw on 2024-07-16 05:45_

@j178 thanks. anyway 

uv can't install packages with both `index-url` and `extra-index-url`.

When I set extra-index-url, uv ignore `index-url`

---

_Closed by @kimdwkimdw on 2024-07-16 05:45_

---

_Comment by @charliermarsh on 2024-07-16 13:02_

@kimdwkimdw - Take a look at the documentation on multiple indexes: https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#packages-that-exist-on-multiple-indexes. Depending on your requirements, you may need `--index-strategy unsafe-best-match`.

---

_Comment by @kimdwkimdw on 2024-07-16 13:23_

@charliermarsh 

Thanks! 🙏🏻

https://github.com/astral-sh/uv/issues/5096#issuecomment-2230266574



---

_Comment by @charliermarsh on 2024-07-16 13:24_

@kimdwkimdw -- No problem, sorry for the confusion.

---
