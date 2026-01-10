```yaml
number: 8037
title: "W291: ruff --fix removes trailing whitespace in multiline string"
type: issue
state: closed
author: Jasha10
labels:
  - bug
assignees: []
created_at: 2023-10-18T05:37:46Z
updated_at: 2024-01-31T21:45:25Z
url: https://github.com/astral-sh/ruff/issues/8037
synced_at: 2026-01-10T11:09:50Z
```

# W291: ruff --fix removes trailing whitespace in multiline string

---

_Issue opened by @Jasha10 on 2023-10-18 05:37_

When `W291` ([trailing-whitespace](https://docs.astral.sh/ruff/rules/trailing-whitespace/)) is enabled, ruff warns about trailing whitespace in multiline strings.
When `--fix` is passed, ruff modifies the multiline string. This could result in a behavior change of the python code.

## Repro:

```python
# repro.py
string = """
this line has trailing whitespace 
"""
```
```
$ ruff repro.py --isolated --select W291
repro.py:3:34: W291 [*] Trailing whitespace
Found 1 error.
[*] 1 potentially fixable with the --fix option.
$ ruff repro.py --isolated --select W291 --fix  # This modifies the multiline string!
Found 1 error (1 fixed, 0 remaining).
$ ruff --version
ruff 0.0.278
```

I noticed that the [implementation `trailing_whitespace`](https://github.com/astral-sh/ruff/blob/dda4ceda715c185aa1916cf361bbd31e1d28a9a0/crates/ruff_linter/src/rules/pycodestyle/rules/trailing_whitespace.rs#L75) categorizes the `--fix` action as [`Fix::safe_edit`](https://github.com/astral-sh/ruff/blob/dda4ceda715c185aa1916cf361bbd31e1d28a9a0/crates/ruff_linter/src/rules/pycodestyle/rules/trailing_whitespace.rs#L105C41-L105C41). I feel that modifying a multiline string is not safe; it can change the behavior of my python code. The doc for [`Applicability::Safe`](https://github.com/astral-sh/ruff/blob/dda4ceda715c185aa1916cf361bbd31e1d28a9a0/crates/ruff_diagnostics/src/fix.rs#L21-L23) say:
```
    /// The fix is safe and can always be applied.
    /// The fix is definitely what the user intended, or it maintains the exact meaning of the code.
```
I feel that this does not apply to `W291` as implemented.

---

_Comment by @charliermarsh on 2023-10-18 13:47_

Yeah, we should change to `unsafe_edit` when the diagnostic is inside a multiline string.

---

_Label `bug` added by @charliermarsh on 2023-10-18 13:47_

---

_Closed by @charliermarsh on 2024-01-31 21:45_

---
