```yaml
number: 471
title: Correct EOL offset for lines ending with multi-byte char
type: pull_request
state: merged
author: sgryjp
labels: []
assignees: []
merged: true
base: main
head: fix/line-end-multibyte-char
created_at: 2022-10-26T03:04:23Z
updated_at: 2022-10-28T03:56:02Z
url: https://github.com/astral-sh/ruff/pull/471
synced_at: 2026-01-12T15:55:04Z
```

# Correct EOL offset for lines ending with multi-byte char

---

_@sgryjp_

For each line, ruff iterates over UTF-32 characters and calculates the candidate of newline offset by adding 1 to byte-offset of the characters. This does not work correctly if there is a line which ends with a multi-byte character.

This commit fixes the issue by adding byte-length of the iterating characters instead of 1.

---

Without this fix, ruff panics when processing a file below with `--select D211`:

```python
# 亜
class Foo:
    """."""
```

---

_Comment by @charliermarsh on 2022-10-26 15:00_

Awesome, thank you!

---

_Merged by @charliermarsh on 2022-10-26 15:00_

---

_Closed by @charliermarsh on 2022-10-26 15:00_

---

_Branch deleted on 2022-10-28 03:56_

---
