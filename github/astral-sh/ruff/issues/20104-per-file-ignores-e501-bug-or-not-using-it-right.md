---
number: 20104
title: "per-file-ignores + E501: bug or not using it right?"
type: issue
state: closed
author: vcaraulean
labels:
  - question
assignees: []
created_at: 2025-08-26T19:15:57Z
updated_at: 2025-08-26T19:51:05Z
url: https://github.com/astral-sh/ruff/issues/20104
synced_at: 2026-01-07T13:12:16-06:00
---

# per-file-ignores + E501: bug or not using it right?

---

_Issue opened by @vcaraulean on 2025-08-26 19:15_

### Summary

Following [per-file-ignores](https://docs.astral.sh/ruff/settings/#lint_per-file-ignores) documentation I have following config section in my `pyproject.toml`:

```toml
[tool.ruff.lint.per-file-ignores]
"models.py" = ["E501"]
# Doubling, just because
"src/**/models.py" = ["E501"]
# ... to be sure
"src/client/connectivity/models.py" = ["E501"]
```

`E501` is: 

```
uv run ruff rule E501
# line-too-long (E501)

Derived from the **pycodestyle** linter.

## What it does
Checks for lines that exceed the specified maximum character length.
```

However, running `uv run ruff format` in the project's root directory still trims the long lines:

<img width="1049" height="368" alt="Image" src="https://github.com/user-attachments/assets/fd7e0360-c7e1-4654-9945-57855b02ed03" />

Am I setting it wrong or my expectations are off?
I want to disable long line re-formatting for readability but only for specific files..

### Version

ruff 0.12.10 (c68ff8d90 2025-08-21)

---

_Renamed from "per-file-ignores - bug or not using it right?" to "per-file-ignores + E501: bug or not using it right?" by @vcaraulean on 2025-08-26 19:16_

---

_Comment by @ntBre on 2025-08-26 19:22_

E501 is a lint rule, so any configuration for it will only affect `ruff check` calls, not `ruff format`. The formatter doesn't really have separate "rules" like the linter, so I don't think there's a `per-file-ignores` analog in the formatter settings. There's [`tool.ruff.format.exclude`](https://docs.astral.sh/ruff/settings/#format_exclude), but that will exclude the files from any formatting, not just the line length.

Otherwise, you may be able to set [`line-length`](https://docs.astral.sh/ruff/settings/#line-length) using a [hierarchical configuration](https://docs.astral.sh/ruff/configuration/#config-file-discovery), if all of the files you want to exclude are in a separate directory.

---

_Label `question` added by @ntBre on 2025-08-26 19:22_

---

_Comment by @ntBre on 2025-08-26 19:24_

This might be a bit more tedious, but you can also use [suppression comments](https://docs.astral.sh/ruff/formatter/#format-suppression) if there are just small blocks of code you'd like to avoid formatting.

---

_Comment by @vcaraulean on 2025-08-26 19:51_

Thanks @ntBre for explaining and pointing to caveats and solutions !
(I'm still learning thing about ruff)

For the time being I'll be happily maintaining my `models.py` formatted for readability and telling ruff to close eyes when going over:
```
[tool.ruff.format]
exclude = ["models.py"]
```


---

_Closed by @vcaraulean on 2025-08-26 19:51_

---
