---
number: 14757
title: "RUF055 ignores backslashes in the `repl` argument of `re.sub`"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-12-03T13:46:37Z
updated_at: 2024-12-03T15:18:38Z
url: https://github.com/astral-sh/ruff/issues/14757
synced_at: 2026-01-10T01:22:55Z
---

# RUF055 ignores backslashes in the `repl` argument of `re.sub`

---

_Issue opened by @dscorbett on 2024-12-03 13:46_

In Ruff 0.8.1, [`unnecessary-regular-expression` (RUF055)](https://docs.astral.sh/ruff/rules/unnecessary-regular-expression/) reports a false positive for `re.sub` when the replacement string contains a backslash.

```console
$ cat ruf055.py
import re
print(re.sub(" ", r"\n", "a b"))

$ python ruf055.py
a
b

$ ruff check --isolated --preview --select RUF055 ruf055.py --fix
Found 1 error (1 fixed, 0 remaining).

$ cat ruf055.py
import re
print("a b".replace(" ", "\\n"))

$ python ruf055.py
a\nb
```

This issue was mentioned in https://github.com/astral-sh/ruff/pull/14679#discussion_r1863802187 with the example of `\g<0>`, which was deemed acceptably implausible. Because a replacement string can also include character escapes, this issue is plausible.

Alternatively, this could be considered a true positive, in which case the fix needs to interpret escape sequences in the replacement string.

---

_Comment by @MichaReiser on 2024-12-03 14:00_

Thanks. 

Okay, so the part that we overlooked from the specification is:

> if it is a string, any backslash escapes in it are processed. That is, \n is converted to a single newline character, \r is converted to a carriage return, and so forth. 

---

_Label `bug` added by @MichaReiser on 2024-12-03 14:00_

---

_Label `help wanted` added by @MichaReiser on 2024-12-03 14:00_

---

_Comment by @MichaReiser on 2024-12-03 14:07_

@AlexWaygood pointed out that this could be addressed by https://github.com/astral-sh/ruff/pull/14679 

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-12-03 14:17_

---

_Referenced in [astral-sh/ruff#14679](../../astral-sh/ruff/pulls/14679.md) on 2024-12-03 15:13_

---

_Comment by @AlexWaygood on 2024-12-03 15:18_

Thanks @dscorbett, as ever! You were quite right that #14679 would not have fixed this in its earlier form. I pushed some updates to it, so I _think_ it now handles everything correctly, though. Let us know if you see any other issues with the check!

---

_Closed by @AlexWaygood on 2024-12-03 15:18_

---
