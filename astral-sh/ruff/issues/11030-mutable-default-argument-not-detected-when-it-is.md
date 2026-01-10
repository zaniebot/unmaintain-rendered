```yaml
number: 11030
title: Mutable default argument not detected when it is a constant (by B006 and others)
type: issue
state: open
author: omgMath
labels:
  - rule
assignees: []
created_at: 2024-04-19T08:03:42Z
updated_at: 2024-09-26T16:38:02Z
url: https://github.com/astral-sh/ruff/issues/11030
synced_at: 2026-01-10T11:09:53Z
```

# Mutable default argument not detected when it is a constant (by B006 and others)

---

_Issue opened by @omgMath on 2024-04-19 08:03_

Hi
I found a case where my expectations does not meet the results reported by ruff. But first, let's start with the issue basics:
* Keywords searched: "B006", "mutable default"
* Ruff version: `ruff 0.4.0`
* Command invoked: `ruff check case.py --select ALL --ignore D,FA --isolated`

Where my `case.py` contains the following lines:
```python
DEFAULT_ENTRIES = [1, 2, 3]

def append_one(entries: list[int] = DEFAULT_ENTRIES) -> None:
    entries.append(1)
```

I would have hoped that this would be detected by [rule B006](https://docs.astral.sh/ruff/rules/mutable-argument-default/) (or any other rule), but sadly it is not.

However, the rule detects the issue if instead of `DEFAULT_ENTRIES` we use `[]`.
Thanks for checking!


---

_Renamed from "B006 " to "Mutable default argument not detected when it is a constant (by B006 and others)" by @omgMath on 2024-04-19 08:11_

---

_Comment by @dhruvmanila on 2024-04-19 08:11_

Thanks for the report. I believe this is the current limitation of many rules where they _don't_ try to resolve the type of a variable. It will only check if it's a literal expression like `[]`, `{}`, etc.

---

_Label `rule` added by @dhruvmanila on 2024-04-19 08:12_

---

_Comment by @charliermarsh on 2024-04-19 13:48_

I feel like we _could_ catch this via the type annotation though?

---

_Comment by @dhruvmanila on 2024-04-19 13:56_

Yeah, it should be doable

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-24 03:11_

---

_Comment by @gerrymanoim on 2024-09-26 16:37_

Just confirming that as of ruff 0.6.6 it still doesn't catch the constant via type annotations:

```python
TEST = [1, 2, 3]

def test(l: list[int] = [1, 2, 3]):
    pass

def test2(l: list[int] = TEST):
    pass
```

Does https://github.com/astral-sh/ruff/pull/11122 need more work? Anything that we could help with? 

---
