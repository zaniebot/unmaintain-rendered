```yaml
number: 17683
title: FURB129 fix should add spaces between tokens
type: issue
state: closed
author: dscorbett
labels:
  - bug
assignees: []
created_at: 2025-04-28T15:34:22Z
updated_at: 2025-06-08T16:00:32Z
url: https://github.com/astral-sh/ruff/issues/17683
synced_at: 2026-01-10T11:09:58Z
```

# FURB129 fix should add spaces between tokens

---

_Issue opened by @dscorbett on 2025-04-28 15:34_

### Summary

The fix for [`readlines-in-for` (FURB129)](https://docs.astral.sh/ruff/rules/readlines-in-for/) can introduce a syntax error when `.readlines()` is followed immediately by a keyword without white space.
```console
$ cat >furb129.py <<'# EOF'
with open("furb129.py") as f:
    print([line for line in f.readlines()if True])
# EOF

$ ruff --isolated check --select FURB129 --unsafe-fixes --diff furb129.py 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

### Version

ruff 0.11.7 (f7b48510b 2025-04-24)

---

_Label `bug` added by @MichaReiser on 2025-04-28 17:45_

---

_Comment by @ntBre on 2025-04-28 18:11_

This kind of issue has popped up in a couple different places recently. I wonder if there's some way to tackle this at the `Edit` or other infrastructural level.

---

_Comment by @MichaReiser on 2025-04-28 20:41_

Doing edits at the ast level would help with this but it has the downside that we loose source formatting 

---

_Comment by @MichaReiser on 2025-04-28 20:43_

Related https://github.com/astral-sh/ruff/issues/14293

---

_Comment by @dscorbett on 2025-04-29 02:05_

`Edit` could run something like [`ruff_linter::fix::edits::pad`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/fix/edits.rs#L538) automatically if it detects that the inserted text needs padding relative to its surroundings.

---

_Closed by @charliermarsh on 2025-06-08 16:00_

---
