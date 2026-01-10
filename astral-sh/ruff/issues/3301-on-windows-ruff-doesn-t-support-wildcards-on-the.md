```yaml
number: 3301
title: "On Windows, ruff doesn't support wildcards on the command line"
type: issue
state: closed
author: pfmoore
labels:
  - bug
  - good first issue
  - cli
assignees: []
created_at: 2023-03-02T12:14:12Z
updated_at: 2023-03-26T14:32:47Z
url: https://github.com/astral-sh/ruff/issues/3301
synced_at: 2026-01-10T11:09:46Z
```

# On Windows, ruff doesn't support wildcards on the command line

---

_Issue opened by @pfmoore on 2023-03-02 12:14_

The command `ruff *.py` gives the error "error: Failed to lint *.py: The filename, directory name, or volume label syntax is incorrect. (os error 123)". I would have expected it to check all `.py` files in the current directory.

---

_Comment by @charliermarsh on 2023-03-02 15:29_

Thanks! Will take a look. My guess is that we're relying on patterns being expanded by the shell (so e.g. on macOS, `ruff *.py` works but `ruff "*.py"` does not).

---

_Label `bug` added by @charliermarsh on 2023-03-02 15:29_

---

_Label `cli` added by @charliermarsh on 2023-03-02 15:29_

---

_Comment by @pfmoore on 2023-03-02 16:20_

Yeah, that's usually the issue. The Windows C runtime often does automatic glob expansion, but I guess Rust doesn't.

---

_Label `good first issue` added by @charliermarsh on 2023-03-18 04:11_

---

_Comment by @charliermarsh on 2023-03-18 04:11_

We may be able to leverage this: https://gitlab.com/kornelski/wild

---

_Closed by @charliermarsh on 2023-03-26 14:32_

---
