---
number: 16234
title: C408 fix changes behavior for non-NFKC identifier
type: issue
state: open
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-02-18T16:23:11Z
updated_at: 2025-02-19T11:33:35Z
url: https://github.com/astral-sh/ruff/issues/16234
synced_at: 2026-01-10T01:22:57Z
---

# C408 fix changes behavior for non-NFKC identifier

---

_Issue opened by @dscorbett on 2025-02-18 16:23_

### Description

The fix for [unnecessary-collection-call (C408)](https://docs.astral.sh/ruff/rules/unnecessary-collection-call/) changes the program’s behavior when a `dict` call’s keyword argument’s runtime name does not equal the name as it appears in the source code. This happens when the name in the source code isn’t normalized in NFKC.

```console
$ cat >c408.py <<'# EOF'
print(dict(ℼ=3.14))
# EOF

$ python c408.py
{'π': 3.14}

$ ruff --isolated check --select C408 c408.py --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ python c408.py
{'ℼ': 3.14}
```

---

_Label `bug` added by @ntBre on 2025-02-18 16:54_

---

_Label `fixes` added by @ntBre on 2025-02-18 16:54_

---

_Comment by @ntBre on 2025-02-18 16:59_

I took a brief look at the code [here](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_comprehensions/fixes.rs#L217) and didn't immediately see anything weird going on with Unicode stuff. I wonder if something could be going wrong with the libcst codegen around here?

https://github.com/astral-sh/ruff/blob/0868e73d2c720839dfe7cc519335eed893bea534/crates/ruff_linter/src/rules/flake8_comprehensions/fixes.rs#L286-L291

This feels like it could be a more general issue than just this rule.

---

_Comment by @jelle-openai on 2025-02-18 17:43_

I believe the issue here is that Python NFKC-normalizes identifiers, but not string literals. In the original `dict(ℼ=3.14)`, `ℼ` is an identifier so it gets normalized:

```
>>> unicodedata.normalize('NFKC', 'ℼ')
'π'
```

But Ruff turns this into `{'ℼ': 3.14}`, which is not an identifier and doesn't get normalized.

The fix would be to NFKC-normalize any identifiers that the C408 autofix turns into string literals.

This issue could also appear in other fixes if they turn identifiers into string literals, but that's not going to be very common.

Perhaps Ruff should also have a rule against identifiers that are not NFKC-normalized.

---

_Comment by @ntBre on 2025-02-18 18:06_

Ah, thanks @jelle-openai! That makes more sense.

---

_Comment by @AlexWaygood on 2025-02-19 11:31_

We emulate Python's NFKC normalisation for identifiers in the lexer here:

https://github.com/astral-sh/ruff/blob/e84985e9b3d101d5bd0b4b475855f9e5ccc0a007/crates/ruff_python_parser/src/lexer.rs#L639-L655

We've had issues in the past when fixes use `libcst` due to the fact that `libcst` uses a different parser, and does _not_ do the same NFKC normalisation of identifiers. This looks like it stems from the same issue

---

_Comment by @AlexWaygood on 2025-02-19 11:33_

> Perhaps Ruff should also have a rule against identifiers that are not NFKC-normalized.

It actually surprises me that we don't already! Considering that we already have `RUF001`, `RUF002` and `RUF003`

---
