```yaml
number: 1840
title: Respect per-file-ignore directory globs for files passed via stdin
type: issue
state: closed
author: john-kurkowski
labels: []
assignees: []
created_at: 2023-01-13T00:55:59Z
updated_at: 2023-01-14T03:30:42Z
url: https://github.com/astral-sh/ruff/issues/1840
synced_at: 2026-01-12T15:54:41Z
```

# Respect per-file-ignore directory globs for files passed via stdin

---

_@john-kurkowski_

`per-file-ignore` filename globs are respected when running Ruff against `.`  and stdin. However, directory globs are not respected when running Ruff against stdin.

```sh
❯ ruff --version
ruff 0.0.220

❯ mkdir ruff-example
❯ cd ruff-example
❯ mkdir tests
❯ echo 'import os' > tests/foo.py

❯ ruff --isolated .
tests/foo.py:1:8: F401 `os` imported but unused
Found 1 error(s).
1 potentially fixable with the --fix option.

# so far so good

❌1 ❮ ruff --isolated --per-file-ignores='foo*:F401' .

# filename glob match OK!

❯ ruff --isolated --per-file-ignores='tests/*:F401' .

# directory glob match OK!

❯ ruff --isolated --per-file-ignores='foo*:F401' --stdin-filename 'tests/foo.py' - < tests/foo.py

# filename glob match OK!

❯ ruff --isolated --per-file-ignores='tests/*:F401' --stdin-filename 'tests/foo.py' - < tests/foo.py
tests/foo.py:1:8: F401 `os` imported but unused
Found 1 error(s).
1 potentially fixable with the --fix option.

# directory glob match not ok

❌1 ❮
```

Related to #1342?

---

_Comment by @charliermarsh on 2023-01-13 01:25_

Thanks, I'll look into this!

---

_Closed by @charliermarsh on 2023-01-13 02:01_

---

_Comment by @john-kurkowski on 2023-01-14 03:30_

❤️ Thanks for Ruff!

---
