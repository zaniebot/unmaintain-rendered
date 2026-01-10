```yaml
number: 16033
title: Camelcase function names not raising error (N802)
type: issue
state: closed
author: plandrem
labels:
  - question
assignees: []
created_at: 2025-02-08T00:40:54Z
updated_at: 2025-02-08T14:35:05Z
url: https://github.com/astral-sh/ruff/issues/16033
synced_at: 2026-01-10T11:09:57Z
```

# Camelcase function names not raising error (N802)

---

_Issue opened by @plandrem on 2025-02-08 00:40_

### Description

**Steps to reproduce**

```
uv init ruff-test
cd ruff-test
echo "def MyFunction():\n    pass" > test.py
uvx ruff check
```

**Expected Result**
Ruff should raise an error concerning N802

**Observed Result**
All checks pass

Am I missing a config step to use pep8-naming?

---

_Comment by @InSyncWithFoo on 2025-02-08 03:26_

`N` rules are not enabled by default, so you need to [`--select`](https://docs.astral.sh/ruff/settings/#lint_select) them:

```shell
$ ruff check --isolated --select N -
def MyFunction():
    pass

-:1:5: N802 Function name `MyFunction` should be lowercase
  |
1 | def MyFunction():
  |     ^^^^^^^^^^ N802
2 |     pass
  |

Found 1 error.
```

See [this page](https://docs.astral.sh/ruff/configuration/) for the default configuration file.

---

_Label `question` added by @dylwil3 on 2025-02-08 08:49_

---

_Closed by @charliermarsh on 2025-02-08 14:35_

---
