```yaml
number: 17112
title: S603 false positive on safe cmd
type: issue
state: closed
author: osauldmy
labels:
  - rule
  - help wanted
assignees: []
created_at: 2025-04-01T09:54:38Z
updated_at: 2025-05-02T19:53:23Z
url: https://github.com/astral-sh/ruff/issues/17112
synced_at: 2026-01-12T15:54:55Z
```

# S603 false positive on safe cmd

---

_@osauldmy_

### Summary

I have cmd as list of constant strings, not example with `input()` as S603 doc suggest to verify, but it still flags as untrusted.

```python
import subprocess

git_diff = subprocess.run(["/usr/bin/git", "diff", "main", "--name-status"], check=True, capture_output=True)
```

```bash
$ ruff check --select S ~/ruff_bug.py
ruff_bug.py:3:12: S603 `subprocess` call: check for execution of untrusted input
```

### Version

0.11.2

---

_Label `rule` added by @MichaReiser on 2025-04-01 12:13_

---

_Comment by @MichaReiser on 2025-04-01 12:14_

Ignoring `run` calls where all arguments are string literals seems reasonable

---

_Label `help wanted` added by @MichaReiser on 2025-04-01 12:14_

---

_Comment by @trag1c on 2025-04-01 17:26_

I'd like to have a go at this!

---

_Assigned to @trag1c by @ntBre on 2025-04-01 18:30_

---

_Closed by @ntBre on 2025-04-02 15:22_

---

_Closed by @ntBre on 2025-04-02 15:22_

---

_Comment by @Avasam on 2025-05-02 19:53_

Hi.
https://github.com/astral-sh/ruff/pull/17136 doesn't work on tuples
![image](https://github.com/user-attachments/assets/aa447131-b564-4ef0-bb6f-a2979bda3c13)


---
