```yaml
number: 15923
title: "[red-knot] MDTest parsing problems"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - ty
assignees: []
created_at: 2025-02-04T07:47:14Z
updated_at: 2025-02-04T13:01:54Z
url: https://github.com/astral-sh/ruff/issues/15923
synced_at: 2026-01-12T15:54:55Z
```

# [red-knot] MDTest parsing problems

---

_@sharkdp_

### Description

Unfortunately, I only noticed this *after* merging #15890 when I worked on some tests. This looks like a regression. In the following file, the code snippet will not be detected and never executed. In a larger file, this could easily lead to mistakes where someone writes a test that seems to succeed.

This "succeeds" on main, but failed with *"unexpected error: [invalid-syntax]"* before merging pre-#15890 (as it should):
````markdown
# My test

No newline between prose and code:
```py
# A syntax error:
;
```
````

While writing a MRE for this bug, I also found this weird behavior which definitely looks like a bug. When the `;` in the example above is replaced with `§`, I get:
````
thread 'mdtest__binary_integers' panicked at crates/ruff_python_trivia/src/cursor.rs:138:41:
byte index 1 is not a char boundary; it is inside '§' (bytes 0..2) of `§
```
`
````

---

_Label `bug` added by @sharkdp on 2025-02-04 07:47_

---

_Label `red-knot` added by @sharkdp on 2025-02-04 07:47_

---

_Assigned to @sharkdp by @sharkdp on 2025-02-04 08:11_

---

_Closed by @sharkdp on 2025-02-04 13:01_

---
