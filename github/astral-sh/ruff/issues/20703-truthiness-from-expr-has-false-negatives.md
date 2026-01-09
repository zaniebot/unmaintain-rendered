---
number: 20703
title: "`Truthiness:from_expr` has false negatives"
type: issue
state: closed
author: dscorbett
labels:
  - bug
assignees: []
created_at: 2025-10-05T19:41:39Z
updated_at: 2025-10-14T08:06:19Z
url: https://github.com/astral-sh/ruff/issues/20703
synced_at: 2026-01-07T13:12:16-06:00
---

# `Truthiness:from_expr` has false negatives

---

_Issue opened by @dscorbett on 2025-10-05 19:41_

### Summary

[`Truthiness:from_expr`](https://github.com/astral-sh/ruff/blob/0.13.3/crates/ruff_python_ast/src/helpers.rs#L1217) has false negatives for certain expressions that are guaranteed to be truthy: generator expressions, lambda expressions, f-string interpolations containing byte strings (even if they are empty), and f-string interpolations containing debug text. This affects [`subprocess-popen-with-shell-equals-true` (S602)](https://docs.astral.sh/ruff/rules/subprocess-popen-with-shell-equals-true/), [`call-with-shell-equals-true` (S604)](https://docs.astral.sh/ruff/rules/call-with-shell-equals-true/), [`unix-command-wildcard-injection` (S609)](https://docs.astral.sh/ruff/rules/unix-command-wildcard-injection/), and [`expr-or-true` (SIM222)](https://docs.astral.sh/ruff/rules/expr-or-true/), at least. [Example](https://play.ruff.rs/3e39aaa8-8e77-4c80-866d-3cec57cfa924):

```console
$ cat >truthiness_fn.py <<'# EOF'
import subprocess
subprocess.run("echo | xxd -p", shell=(x for x in ()))
print(bool(dict(shell=lambda: 0)["shell"]))
print(subprocess.check_output("echo chmod 777 truthiness_*.py", shell=f"{b""}"))
print(f"{""=}" or 0)
# EOF

$ python truthiness_fn.py
0a
True
b'chmod 777 truthiness_fn.py\n'
""=''

$ ruff --isolated check truthiness_fn.py --select S602,S604,S609,SIM222
All checks passed!
```

### Version

ruff 0.13.3 (188c0dce2 2025-10-02)

---

_Referenced in [astral-sh/ruff#20704](../../astral-sh/ruff/pulls/20704.md) on 2025-10-05 22:27_

---

_Label `bug` added by @ntBre on 2025-10-06 13:13_

---

_Closed by @dylwil3 on 2025-10-14 08:06_

---
