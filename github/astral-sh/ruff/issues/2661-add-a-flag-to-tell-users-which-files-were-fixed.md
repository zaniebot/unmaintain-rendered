---
number: 2661
title: Add a flag to tell users which files were fixed, and via which rules
type: issue
state: closed
author: charliermarsh
labels:
  - cli
assignees: []
created_at: 2023-02-08T15:47:31Z
updated_at: 2023-02-12T20:48:03Z
url: https://github.com/astral-sh/ruff/issues/2661
synced_at: 2026-01-07T13:12:14-06:00
---

# Add a flag to tell users which files were fixed, and via which rules

---

_Issue opened by @charliermarsh on 2023-02-08 15:47_

_No description provided._

---

_Label `cli` added by @charliermarsh on 2023-02-08 15:47_

---

_Comment by @charliermarsh on 2023-02-09 01:21_

@henryiii - Sorry for the ping, but one more question here. At least in your case, would you want to see this output on every Ruff invocation? Or would it be sufficient to include it when run with `--verbose`, e.g., for debugging purposes?

---

_Comment by @henryiii on 2023-02-09 01:30_

I'd prefer it on every Ruff invocation - it's just as important as the list of things that are fixable, and no more verbose than that list if you didn't pass the `--fix`. Knowing what it changed and why is just as important as what still needs to be done, and I'd just run it again if I wanted only the remaining fixable items (I do that with Rubocop, actually). But I'm totally fine with it being a flag too, I'd just add it to pre-commit's args. I woudn't want too much extra info, though - the file, line number, code, and reason for the unfixed checks is just about perfect for the fixed checks too.

Recent example: I added a noqa for a problem, then ran Ruff:

```
ruff.....................................................................Failed
- hook id: ruff
- exit code: 1
- files were modified by this hook

tests/test_command.py:413:5: SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
Found 2 errors (1 fixed, 1 remaining).
```

It deleted the noqa I added (since I missed by one line, I put it on the inner with instead of the outer with). But I can't tell that it did that from the output above; if I got the RUF00x noqa removal fix listed as "fixed", it would be much clearer what happened.

Ref: https://github.com/wntrblm/nox/pull/691

---

_Referenced in [astral-sh/ruff#2705](../../astral-sh/ruff/pulls/2705.md) on 2023-02-10 03:00_

---

_Referenced in [astral-sh/ruff#2707](../../astral-sh/ruff/pulls/2707.md) on 2023-02-10 03:27_

---

_Closed by @charliermarsh on 2023-02-12 20:48_

---
