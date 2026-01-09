---
number: 18863
title: "`bad-file-permissions` (S103) has false negatives and false positives"
type: issue
state: open
author: dscorbett
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-06-22T15:32:02Z
updated_at: 2025-06-25T06:41:00Z
url: https://github.com/astral-sh/ruff/issues/18863
synced_at: 2026-01-07T13:12:16-06:00
---

# `bad-file-permissions` (S103) has false negatives and false positives

---

_Issue opened by @dscorbett on 2025-06-22 15:32_

### Summary

[`bad-file-permissions` (S103)](https://docs.astral.sh/ruff/rules/bad-file-permissions/) has false negatives and false positives.

[The upstream rule checks `0o33`](https://github.com/PyCQA/bandit/blob/1.8.5/bandit/plugins/general_bad_file_permissions.py#L63-L69) but Ruff’s S103 only checks `0o12`.
```console
$ cat >s103_1.py <<'# EOF'
import os
os.chmod("/etc/secrets.txt", 0o21)
# EOF

$ flake8 ./s103_1.py
./s103_1.py:2:1: S103 Chmod setting a permissive mask 0o21 on file (/etc/secrets.txt).

$ ruff --isolated check s103_1.py --select S103
All checks passed!
```

Only the bits of `0o33` matter for the permissiveness check. S103 should not be suppressed when those bits can statically be determined in the result of a bitwise OR, even when the full result can’t.
```python
import os
def f(path, mode):
    os.chmod(path, mode | 0o777)
```

There is a false positive when the result of a bitwise AND can statically be determined to be valid, even when a subexpression has an invalid value.
```python
$ cat >s103_2.py <<'# EOF'
import os
os.chmod("/etc/secrets.txt", 0o777777 & 0o700)
os.chmod("/etc/secrets.txt", 0o777777 & 0o777)  # This is a true positive for the wrong reason.
# EOF

$ ruff --isolated check s103_2.py --select S103 --output-format concise -q
s103_2.py:2:30: S103 `os.chmod` setting an invalid mask on file or directory
s103_2.py:3:30: S103 `os.chmod` setting an invalid mask on file or directory
```

### Version

ruff 0.12.0 (87f0feb21 2025-06-17)

---

_Comment by @MichaReiser on 2025-06-23 07:09_

That Ruff only checks Group execute and world write, but not group write or world execute, has been the case since the rule was added (https://github.com/astral-sh/ruff/pull/1636/). 

The upstream rule distinguishes between group write/execute (medium) and world (write/execute) but that's not what's implemented in Ruff. https://github.com/PyCQA/bandit/pull/570/files


The rule does have special handling for bitwise operands but it only supports values up to `u16` as the left/right side:

https://github.com/astral-sh/ruff/blob/daa385c1a991cc87c77642eccdcc2a888d3586a0/crates/ruff_linter/src/rules/flake8_bandit/rules/bad_file_permissions.rs#L171-L176

---

_Label `bug` added by @MichaReiser on 2025-06-23 07:09_

---

_Label `help wanted` added by @MichaReiser on 2025-06-23 07:09_

---

_Comment by @Peiffap on 2025-06-24 23:29_

I'd be open to taking a stab at this.

@MichaReiser, do you have additional context on why the Ruff rule differs from the upstream rule? Would aligning them be desirable?
As for the `u16` limit, is there a specific reason to keep it? For reference, this limitation came about in #7584 to get rid of the `num-bigint` dependency/improve performance.

---

_Comment by @MichaReiser on 2025-06-25 06:41_

> @MichaReiser, do you have additional context on why the Ruff rule differs from the upstream rule? Would aligning them be desirable?

No, not more than what you can find from looking at the git history. Aligning with upstream is probably desired but we should double check that the change is in line with the rule's intent (and we probably need to gate it behind preview mode)

> As for the u16 limit, is there a specific reason to keep it? For reference, this limitation came about in https://github.com/astral-sh/ruff/pull/7584 to get rid of the num-bigint dependency/improve performance.

I don't think so. We shouldn't re-introduce `num-bigint`, it's probably enough to change the check to use `usize` instead (or `u64`)

---
