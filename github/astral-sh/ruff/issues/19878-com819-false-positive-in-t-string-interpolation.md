---
number: 19878
title: COM819 false positive in t-string interpolation
type: issue
state: closed
author: dscorbett
labels:
  - bug
assignees: []
created_at: 2025-08-12T13:06:47Z
updated_at: 2025-10-03T18:29:36Z
url: https://github.com/astral-sh/ruff/issues/19878
synced_at: 2026-01-07T13:12:16-06:00
---

# COM819 false positive in t-string interpolation

---

_Issue opened by @dscorbett on 2025-08-12 13:06_

### Summary

[`prohibited-trailing-comma` (COM819)](https://docs.astral.sh/ruff/rules/prohibited-trailing-comma/) incorrectly detects the trailing comma in an unparenthesized tuple in a t-string interpolation. [Example](https://play.ruff.rs/b9fa193e-122e-42a3-9f1b-b1073f95f339):
```console
$ cat >com819.py <<'# EOF'
print(t"{0,}".interpolations[0].value)
# EOF

$ python3.14 com819.py
(0,)

$ ruff --isolated check com819.py --select COM819 --target-version py314 --preview --fix
All checks passed!

$ cat com819.py
print(t"{0}".interpolations[0].value)

$ python3.14 com819.py
0
```

### Version

ruff 0.12.8 (f51a228f0 2025-08-07)

---

_Label `bug` added by @ntBre on 2025-08-12 13:42_

---

_Comment by @vladNed on 2025-08-12 17:55_

I can have a look at it. Can I work on this issue ? 

---

_Assigned to @vladNed by @ntBre on 2025-08-12 17:58_

---

_Comment by @vladNed on 2025-08-13 05:16_

checked [trailing_commas.py](https://github.com/astral-sh/ruff/blob/79c949f0f75f6582782f3d5fddc65089789801d5/crates/ruff_linter/src/rules/flake8_commas/rules/trailing_commas.rs#L249) and saw that the t-strings are not treated. more than this, in f-strings, trailing commas are not treated either in tuples
```python
print(f"{(0, 1, )}")
```
I left an [example here](https://play.ruff.rs/5651bd7e-9446-46d3-b2c3-2271a26bd541)

I'll try to add the t-string and f-string cases in the linter

---

_Comment by @dscorbett on 2025-10-03 17:44_

I think #20357 fixed this.

---

_Closed by @ntBre on 2025-10-03 18:29_

---
