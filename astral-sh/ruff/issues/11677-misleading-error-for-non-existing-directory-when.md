---
number: 11677
title: Misleading error for non-existing directory when checking format
type: issue
state: closed
author: Holt59
labels: []
assignees: []
created_at: 2024-06-01T12:53:56Z
updated_at: 2024-06-01T17:37:32Z
url: https://github.com/astral-sh/ruff/issues/11677
synced_at: 2026-01-10T01:22:51Z
---

# Misleading error for non-existing directory when checking format

---

_Issue opened by @Holt59 on 2024-06-01 12:53_

When running `ruff format --check XXX` on a directory that does not exist, the error currently is

> error: Failed to format tests: The system cannot find the file specified. (os error 2)

This is a bit misleading since this was just checking format, not actually formatting, should be "Failed to check format for" or something like that.

Ruff Version: 0.4.7

---

_Comment by @charliermarsh on 2024-06-01 17:37_

Candidly I think this is ok. With `--check`, the first step is to format the file, we just don't write the formatted file to-disk. So it is true that formatting failed. If `--check` fails for other reasons (e.g., if we failed to parse the file), we show a different message based on that failure.


---

_Closed by @charliermarsh on 2024-06-01 17:37_

---
