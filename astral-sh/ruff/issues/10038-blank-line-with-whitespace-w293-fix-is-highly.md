```yaml
number: 10038
title: (🐞) blank-line-with-whitespace (W293) fix is highly unsafe
type: issue
state: closed
author: KotlinIsland
labels:
  - bug
  - fixes
assignees: []
created_at: 2024-02-19T05:59:41Z
updated_at: 2024-02-19T11:58:57Z
url: https://github.com/astral-sh/ruff/issues/10038
synced_at: 2026-01-12T15:54:49Z
```

# (🐞) blank-line-with-whitespace (W293) fix is highly unsafe

---

_@KotlinIsland_

```py
def test_something():
    assert foo() == """\
assert [1, 2, 3] == [1, 4, 3]
  
  At index 1 diff: 2 != 4
  Use -v to get more diff"""
```
Here, the white space on line 2 of the expected value is intentional, yet `ruff check --fix` deletes it without question.

Or is this actually a bug, and it shouldn't be applying to strings at all?

# Workaround
Disable this rule and use `ruff format` instead, as it doesn't modify the content of strings.



---

_Comment by @MichaReiser on 2024-02-19 07:19_

I think that rule shouldn't apply to strings, or if it does, it should be marked as unsafe. 

We generally recommend the use of the formatter over style related lint rules if you use the formatter anyway (avoids running unnecessary rules )

---

_Label `bug` added by @MichaReiser on 2024-02-19 07:19_

---

_Label `fixes` added by @MichaReiser on 2024-02-19 07:19_

---

_Comment by @dhruvmanila on 2024-02-19 07:21_

It might be an actual bug because we do mark it as unsafe inside a multiline string:

https://github.com/astral-sh/ruff/blob/3209b8545fa749c3c9dd9d37d6931d0f2ccdc26c/crates/ruff_linter/src/rules/pycodestyle/rules/trailing_whitespace.rs#L105-L113

---

_Closed by @MichaReiser on 2024-02-19 11:58_

---
