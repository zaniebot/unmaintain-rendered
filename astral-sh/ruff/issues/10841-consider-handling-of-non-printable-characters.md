```yaml
number: 10841
title: Consider handling of non-printable characters when showing code/diffs
type: issue
state: closed
author: carljm
labels:
  - bug
assignees: []
created_at: 2024-04-08T23:45:05Z
updated_at: 2024-06-08T06:22:04Z
url: https://github.com/astral-sh/ruff/issues/10841
synced_at: 2026-01-10T11:09:53Z
```

# Consider handling of non-printable characters when showing code/diffs

---

_Issue opened by @carljm on 2024-04-08 23:45_

Search keyword used: "non-printable"

Python source code containing non-printable chars (e.g. `^H` or backspace, `^Z` or substitution, `^[` or escape; see https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/resources/test/fixtures/pylint/invalid_characters.py for examples) can cause unpredictable and confusing results if we output it as-is.

Here's a minimal example of the current behavior:
```
➜ python3 -c "print('b = \'\x08\'')" > bad.py

➜ cat bad.py
b = '

➜ cat -v bad.py
b = '^H'

➜ xxd bad.py
00000000: 6220 3d20 2708 270a                      b = '.'.

➜ cargo run -- check --diff --no-cache --preview --select PLE2510 bad.py
    Finished dev [unoptimized + debuginfo] target(s) in 0.09s
     Running `target/debug/ruff check --diff --no-cache --preview --select PLE2510 bad.py`
--- bad.py
+++ bad.py
@@ -1 +1 @@
-b = '
+b = '\b'

Would fix 1 error.
```

Here the diff is confusing because it looks like the original string doesn't even have matching quotes, just a single quote char. In fact it has two quotes, but the intervening backspace character deleted one of them. (Note this is the same behavior shown by `cat` without `-v`; it just allows control characters to take effect.)

Interestingly, this is also the same as the default behavior of both `diff` and `git diff`, which means we may want to be cautious about diverging from it. But we could consider doing some kind of escaping of non-printable characters before outputting code frames or diffs.

---

_Label `bug` added by @charliermarsh on 2024-04-09 00:10_

---

_Closed by @MichaReiser on 2024-06-08 06:22_

---
