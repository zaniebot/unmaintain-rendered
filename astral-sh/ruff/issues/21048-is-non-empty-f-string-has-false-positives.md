```yaml
number: 21048
title: "`is_non_empty_f_string` has false positives"
type: issue
state: open
author: dscorbett
labels:
  - bug
assignees: []
created_at: 2025-10-23T15:48:56Z
updated_at: 2025-10-24T20:54:26Z
url: https://github.com/astral-sh/ruff/issues/21048
synced_at: 2026-01-12T15:54:57Z
```

# `is_non_empty_f_string` has false positives

---

_@dscorbett_

### Summary

[`is_non_empty_f_string`](https://github.com/astral-sh/ruff/blob/0.14.1/crates/ruff_python_ast/src/helpers.rs#L1350) has false positives for f-strings with certain interpolations. The interpolation `{f""!s}` is empty. Interpolations with format specs or containing comparison expressions have unknown emptiness. [Here is an example](https://play.ruff.rs/5612a49d-2c07-4852-9725-340777c1982a) with [`call-with-shell-equals-true` (S604)](https://docs.astral.sh/ruff/rules/call-with-shell-equals-true/):
```console
$ cat >is_non_empty_f_string_fp.py <<'# EOF'
print(bool(dict(shell=f"{f""!s}")["shell"]))

print(bool(dict(shell=f"{"x":.0}")["shell"]))

class C:
    def __init__(self, chars):
        self.chars = chars
    def __gt__(self, other):
        return "".join(sorted(c for c in self.chars if all(c > o for o in other.chars)))
print(bool(dict(shell=f"{C("abc") > C("xyz")}")["shell"]))
# EOF

$ python is_non_empty_f_string_fp.py
False
False
False

$ ruff --isolated check is_non_empty_f_string_fp.py --select S604 --output-format concise -q
is_non_empty_f_string_fp.py:1:12: S604 Function call with truthy `shell` parameter identified, security issue
is_non_empty_f_string_fp.py:3:12: S604 Function call with truthy `shell` parameter identified, security issue
is_non_empty_f_string_fp.py:10:12: S604 Function call with truthy `shell` parameter identified, security issue
```

### Version

ruff 0.14.1 (2bffef596 2025-10-16)

---

_Label `bug` added by @ntBre on 2025-10-23 18:14_

---

_Comment by @IDrokin117 on 2025-10-24 20:17_

I will work on this

---

_Assigned to @IDrokin117 by @ntBre on 2025-10-24 20:54_

---
