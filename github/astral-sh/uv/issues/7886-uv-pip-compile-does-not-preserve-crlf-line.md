---
number: 7886
title: uv pip compile does not preserve CRLF line endings in requirements.txt
type: issue
state: open
author: petermbauer
labels:
  - needs-decision
assignees: []
created_at: 2024-10-03T07:41:46Z
updated_at: 2024-10-03T23:31:07Z
url: https://github.com/astral-sh/uv/issues/7886
synced_at: 2026-01-07T13:12:17-06:00
---

# uv pip compile does not preserve CRLF line endings in requirements.txt

---

_Issue opened by @petermbauer on 2024-10-03 07:41_

It seems that `uv pip compile` does not preserve the line endings in the `requirements.txt` and i found no setting to use native line endings either.
`pip-compile` offers the `--newline` setting to cover this:
```
--newline [LF|CRLF|native|preserve]
                                Override the newline control characters used
```
In my case, either `native` or `preserve` would solve the issue, maybe it makes sense to preserve the line endings by default.
This is particularly annoying in combination with `pre-commit` when running all hooks since this results in a modified `requirements.txt` every time.

### How to reproduce:
- Windows
- uv 0.4.18
- `.gitattributes` file:
```
* text=auto
```
so also the `requirements.txt` file is being checked out with CRLF line endings
- `uv pip compile requirements.in -o requirements.txt --universal --annotation-style line`
- `git diff requirements.txt`
```
warning: in the working copy of 'requirements.txt', LF will be replaced by CRLF the next time Git touches it
```

### Workaround:
- `.gitattributes` file:
```
requirements.txt text eol=lf
```

---

_Label `needs-decision` added by @charliermarsh on 2024-10-03 23:31_

---
