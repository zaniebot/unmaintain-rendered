```yaml
number: 19927
title: "S602, S604, and S609 treat `{**{}}` as truthy"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - rule
assignees: []
created_at: 2025-08-15T12:32:56Z
updated_at: 2025-09-10T21:06:34Z
url: https://github.com/astral-sh/ruff/issues/19927
synced_at: 2026-01-12T15:54:57Z
```

# S602, S604, and S609 treat `{**{}}` as truthy

---

_@dscorbett_

### Summary

As the value of a `shell` argument, [`subprocess-popen-with-shell-equals-true` (S602)](https://docs.astral.sh/ruff/rules/subprocess-popen-with-shell-equals-true/), [`call-with-shell-equals-true` (S604)](https://docs.astral.sh/ruff/rules/call-with-shell-equals-true/), and [`unix-command-wildcard-injection` (S609)](https://docs.astral.sh/ruff/rules/unix-command-wildcard-injection/) treat a dictionary display as truthy when its elements are all double-starred non-name expressions, but a non-name expression can evaluate to an empty mapping, so the `shell` value might actually be falsey. These rules should check the truthiness of the double-starred expressions, or they should treat them as having unknown truthiness. [Example](https://play.ruff.rs/d23da6b9-9d36-4c44-a8e9-4018621ee7f4):
```
$ cat >s60.py <<'# EOF'
import subprocess
subprocess.Popen(["chmod", "777", "*.py"], shell={**{}})
dict(shell={**{}})
# EOF

$ ruff --isolated check s60.py --select S602,S604,S609 --output-format concise -q
s60.py:2:1: S602 `subprocess` call with truthy `shell` identified, security issue
s60.py:2:18: S609 Possible wildcard injection in call due to `*` usage
s60.py:3:1: S604 Function call with truthy `shell` parameter identified, security issue
```

### Version

ruff 0.12.9 (ef422460d 2025-08-14)

---

_Label `bug` added by @ntBre on 2025-08-15 15:50_

---

_Label `rule` added by @ntBre on 2025-08-15 15:50_

---

_Comment by @hauntsaninja on 2025-08-16 21:02_

What does the real world code where this comes up in look like?

---

_Comment by @dscorbett on 2025-08-18 01:10_

This is [a bug](https://github.com/astral-sh/ruff/blob/0.12.9/crates/ruff_python_ast/src/helpers.rs#L1302) in the general helper function `Truthiness::from_expr` which happens to manifest in these three rules. `{**{}}` is the simplest way to reproduce it. A more realistic example would be something like `{**self.shell_defaults, **self.fetch_shell_config(self.username)}`.

---

_Closed by @ntBre on 2025-09-10 21:06_

---
