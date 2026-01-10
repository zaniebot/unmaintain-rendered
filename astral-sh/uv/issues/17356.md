```yaml
number: 17356
title: uv pip compile ignoring --output-file
type: issue
state: closed
author: phw
labels:
  - question
assignees: []
created_at: 2026-01-08T07:30:44Z
updated_at: 2026-01-09T09:56:31Z
url: https://github.com/astral-sh/uv/issues/17356
synced_at: 2026-01-10T03:11:36Z
```

# uv pip compile ignoring --output-file

---

_Issue opened by @phw on 2026-01-08 07:30_

### Summary

If I run `uv pip compile` with the `-o` / `--output-file` parameter it ignores the output file and writes always to stdout.

E.g.

```
uv pip compile pyproject.toml -o requirements.txt
```

Expected result: The changed requirements output is written to `requirements.txt` and the file is updated
Actual result: The `requirements.txt` is unchanged, the output is written only to stdout.

Workaround is to redirect to the file:

```
uv pip compile pyproject.toml > requirements.txt
```

But I also experience this issue with the `uv-pre-commit` hook

### Platform

Linux

### Version

0.9.21

### Python version

3.14.2

---

_Label `bug` added by @phw on 2026-01-08 07:30_

---

_Comment by @zanieb on 2026-01-08 13:14_

It writes to stdout and to the output file. You can silence the former with `-q`. If there's an output file, we'll use it as a reference and only upgrade packages if you ask us to, e.g., `--upgrade` or `--upgrade-package foo`.

---

_Label `bug` removed by @zanieb on 2026-01-08 13:14_

---

_Label `question` added by @zanieb on 2026-01-08 13:14_

---

_Comment by @zanieb on 2026-01-08 13:15_

See https://docs.astral.sh/uv/pip/compile/#upgrading-requirements

---

_Comment by @phw on 2026-01-09 09:56_

@zanieb Thanks for the quick answer. You are right, I missed that the command is using the information from the file, and hence there was no actual change. That explains a lot, not a bug at all :) The `--upgrade` works.

---

_Closed by @phw on 2026-01-09 09:56_

---
