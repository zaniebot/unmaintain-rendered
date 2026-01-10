```yaml
number: 22394
title: "new rule - ban writing text to file without explicit `newline` argument"
type: issue
state: open
author: DetachHead
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2026-01-05T08:09:23Z
updated_at: 2026-01-08T17:44:04Z
url: https://github.com/astral-sh/ruff/issues/22394
synced_at: 2026-01-10T11:10:00Z
```

# new rule - ban writing text to file without explicit `newline` argument

---

_Issue opened by @DetachHead on 2026-01-05 08:09_

### Summary

`Path.write_text` (and other functions that use `io.TextIOWrapper`) is completely broken by default on windows:

```py
from pathlib import Path

path = Path("foo.txt")
path.write_text("foo\r\nbar")
print(path.read_bytes())  # b'foo\r\r\nbar'
```

python tries to be clever by "fixing" `\n` on windows (which [hasn't even been necessary for many years now](https://old.reddit.com/r/git/comments/7mg1q1/gits_crlf_handling_is_the_source_of_all_modern/)), but it doesn't even check whether the `\n` is already preceded by a `\r`, which causes it to write `\r\r\n` to the file.

i opened an issue for this at https://github.com/python/cpython/issues/143428, but it seems unlikely that it will be fixed because it could be considered a breaking change. it would be nice if ruff had a rule to warn about this behavior.

---

_Comment by @MichaReiser on 2026-01-05 11:00_

This does feel like a footgun but forcing `newline=''` on every `write_text` call also feels annoying, especially because the newline conversionts between reading and writing files must stay in sync. 

So I think this really comes down to finding a good heuristics, but I suspect that this might require very extensive data flow analysis if done properly (without too many false negatives)

---

_Label `rule` added by @MichaReiser on 2026-01-05 11:01_

---

_Label `needs-decision` added by @MichaReiser on 2026-01-05 11:01_

---

_Comment by @DetachHead on 2026-01-05 11:13_

alternatively it could recommend using `write_bytes` instead. i get that people might find it annoying but the rule can easily be disabled. personally i find this to be such an egregious bug that i'd definitely prefer *all* usages to be reported.

---

_Comment by @MichaReiser on 2026-01-05 12:13_

> alternatively it could recommend using write_bytes instead. 

I think this is only the case if it is known that the text doesn't come from a `read_text` call or they end up with converted newlines.

---

_Comment by @amyreese on 2026-01-08 17:44_

At the very least, I think `write_bytes()` would always be a good recommendation when `write_text()` is given a string literal that contains `\r`.

---
