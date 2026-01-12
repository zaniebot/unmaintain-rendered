```yaml
number: 8902
title: UP031 does not flag errors
type: issue
state: closed
author: abhijeetbodas2001
labels: []
assignees: []
created_at: 2023-11-29T06:06:24Z
updated_at: 2023-11-29T10:27:44Z
url: https://github.com/astral-sh/ruff/issues/8902
synced_at: 2026-01-12T15:54:48Z
```

# UP031 does not flag errors

---

_@abhijeetbodas2001_

The `UP031` fixer seems to be broken. I picked up one of the examples from the documentation for the fixer itself (https://docs.astral.sh/ruff/rules/printf-string-formatting/), but latest `ruff v0.1.6` did not flag it.

```bash
[~] $ cat test.py
[~] $ cat -n test.py
     1  value = 1
     2  print("%s" % value)  # "1"
     3  print("{}".format(value))  # "1"
     4
     5  value = (1,)
     6  print("%s" % value)  # "1"
     7  print("{}".format(value))  # "(1,)"
     8

[~] $ ruff --select UP test.py
test.py:3:7: UP032 [*] Use f-string instead of `format` call
test.py:7:7: UP032 [*] Use f-string instead of `format` call
Found 2 errors.
[*] 2 fixable with the `--fix` option.
[~] $ ruff --version
ruff 0.1.6
[~] $
```

(Notice that line 2 and line 6 did not get flagged by `UP031`)

Please LMK if any other info is required for debugging. Thanks a lot for all your efforts on this project!

---

_Comment by @Avasam on 2023-11-29 06:33_

I think you meant `UP031`.  I don't think Ruff currently infers the type of the variable. The example in the doc shows you why Ruff can't autofix this w/o type information. It's expected to not be fixable with that specific example.  Although I think it should still raise it, see #6796, which this issue might duplicate.

---

_Comment by @abhijeetbodas2001 on 2023-11-29 10:25_

> I think you meant UP031

Oops, yes. I will update the title.

> Although I think it should still raise it, see https://github.com/astral-sh/ruff/issues/6796, which this issue might duplicate.

Yup, that issue describes the same use case. Even I wanted just flagging the occurence, even if not autofixing it.
I will close out this issue. Thanks!

---

_Renamed from "UP301 does not flag errors" to "UP031 does not flag errors" by @abhijeetbodas2001 on 2023-11-29 10:26_

---

_Closed by @abhijeetbodas2001 on 2023-11-29 10:27_

---
