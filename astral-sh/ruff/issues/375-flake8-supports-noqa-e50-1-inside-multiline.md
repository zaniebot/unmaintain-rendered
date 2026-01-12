```yaml
number: 375
title: "Flake8 supports `# noqa: E50`1 inside multiline strings"
type: issue
state: closed
author: ghuls
labels:
  - question
assignees: []
created_at: 2022-10-09T20:47:43Z
updated_at: 2022-10-09T22:13:42Z
url: https://github.com/astral-sh/ruff/issues/375
synced_at: 2026-01-12T15:54:40Z
```

# Flake8 supports `# noqa: E50`1 inside multiline strings

---

_@ghuls_

Flake8 supports `# noqa: E50`1 inside multiline strings.

Ruff README says that is has to be after. Will the former be ever supported?
```
"""Lorem ipsum dolor sit amet.

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
"""  # noqa: E501
```

---

_Comment by @charliermarsh on 2022-10-09 21:29_

Ruff actually used to support this but I removed it to simplify the code. I would consider re-adding it though. Do you find it useful?

I (personally) prefer putting the `noqa` at the end, since if you put it _inside_ the string, then it gets treated as actual string contents. So, e.g., if you include it in a docstring, then when you render the docstrings in other tools, the `noqa` is included.

---

_Label `question` added by @charliermarsh on 2022-10-09 21:29_

---

_Comment by @ghuls on 2022-10-09 22:00_

Indeed you are right that is shows up in rendered doc strings. In that case is indeed not that useful.

---

_Closed by @charliermarsh on 2022-10-09 22:13_

---
