```yaml
number: 19788
title: PLE2513 and format have different hex casing opinions
type: issue
state: closed
author: ember91
labels:
  - good first issue
  - fixes
assignees: []
created_at: 2025-08-06T17:24:46Z
updated_at: 2025-08-08T12:25:12Z
url: https://github.com/astral-sh/ruff/issues/19788
synced_at: 2026-01-10T11:09:59Z
```

# PLE2513 and format have different hex casing opinions

---

_Issue opened by @ember91 on 2025-08-06 17:24_

### Summary

## Minimal working example

Create a file with a string containing just the ASCII ESC character. For example like this in bash:

```bash
printf '"\x1b"\n' >file.py
```

Fix it:

```bash
ruff check --select PLE2513 --isolated ---fix file.py
```

It changed it to `"\x1B"` instead of ESC. Format it:

```
ruff format --isolated file.py
```

Now it's `"\x1b"`with a lowercase "b". I expected the checker and formatter to agree on the casing of the hexadecimal letters.

### Version

0.12.7

---

_Comment by @ntBre on 2025-08-06 18:13_

Oh, good catch. It looks like these are just hard-coded as capital letters in the fix replacement, so this should be as easy as swapping them to lowercase:

https://github.com/astral-sh/ruff/blob/4d57fcd5a488e628aa26aaacd0a61728609ff310/crates/ruff_linter/src/rules/pylint/rules/invalid_string_characters.rs#L212-L219

We don't always produce formatted fixes, but it seems a little silly not to in this case.

---

_Label `good first issue` added by @ntBre on 2025-08-06 18:13_

---

_Label `fixes` added by @ntBre on 2025-08-06 18:13_

---

_Comment by @ember91 on 2025-08-06 19:29_

Seems like a good fix.

Thanks for an awesome piece of software I use almost every day!

---

_Comment by @ember91 on 2025-08-07 14:50_

I made a PR in https://github.com/astral-sh/ruff/pull/19808

---

_Closed by @ntBre on 2025-08-08 12:25_

---
