```yaml
number: 3405
title: "[`pylint`] C1901: compare-to-empty-string"
type: pull_request
state: merged
author: AreamanM
labels:
  - rule
assignees: []
merged: true
base: main
head: C1901
created_at: 2023-03-08T16:19:21Z
updated_at: 2023-04-24T15:32:28Z
url: https://github.com/astral-sh/ruff/pull/3405
synced_at: 2026-01-12T04:28:19Z
```

# [`pylint`] C1901: compare-to-empty-string

---

_Pull request opened by @AreamanM on 2023-03-08 16:19_

There's a few finishing touches left to make(docs, diagnostic message,  make the logic a little cleaner) and I think this is autofixable, so if the diagnostic impl itself looks good, I'll have a go at autofix for this as well.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/compare_to_empty_string.rs`:35 on 2023-03-08 18:49_

My recommendation would be to instead `match` on the `value`, and check for `value.is_empty()` in `Constant::Str(value)`. Would that have any problems? (It could also catch cases in which the string had a prefix, like `u`, which this would miss.)

---

_@charliermarsh reviewed on 2023-03-08 18:49_

---

_@charliermarsh reviewed on 2023-03-08 18:50_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/compare_to_empty_string.rs`:35 on 2023-03-08 18:50_

Separately: does Pylint catch `x == b""`? Or `x == f""`?

---

_@AreamanM reviewed on 2023-03-08 21:08_

---

_Review comment by @AreamanM on `crates/ruff/src/rules/pylint/rules/compare_to_empty_string.rs`:35 on 2023-03-08 21:08_

I think matching on the value definitely makes sense.

I just tested the latest stable release of pylint and it does not catch the b"" or f"" cases, it only shows the lint warning for `""` without any specifiers. Pylint also shows the lint warning for `x is ""` and `x is not ""` which seems sensible as well. 

---

_@charliermarsh reviewed on 2023-03-08 22:59_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/compare_to_empty_string.rs`:24 on 2023-03-08 22:59_

I'd suggest just making a new `enum` in this file specifically for this violation, to encode this assumption in the types themselves.

---

_Comment by @AreamanM on 2023-03-09 00:12_

I've fixed the diagnostics message and tested the code and it all seems to work, unfortunately I've got a busier week than anticipated so I'll see if I can implement autofix in another PR sometime later

---

_Marked ready for review by @AreamanM on 2023-03-09 00:12_

---

_Comment by @charliermarsh on 2023-03-09 00:13_

No prob, thanks for this!

---

_Label `rule` added by @charliermarsh on 2023-03-09 21:25_

---

_Renamed from "Pylint C1901: compare-to-empty-string" to "[`pylint`] C1901: compare-to-empty-string" by @charliermarsh on 2023-03-09 21:25_

---

_Merged by @charliermarsh on 2023-03-09 21:33_

---

_Closed by @charliermarsh on 2023-03-09 21:33_

---

_Comment by @gil-cohen on 2023-04-03 11:24_

hey, i encountered this rule just now and i think its problematic, take the following example:
```
def foo(a: str | None):
    if a == '':
        print('empty string')
    if a is None:
        print('none')
    if not a:
        print('not a')
        
        
foo('')
foo(None)
```
which outputs:
```
empty string
not a
none
not a
```

running `ruff --select PL a.py` will output:
```a.py:2:13: PLC1901 `a == ''` can be simplified to `not a` as an empty string is falsey
Found 1 error.```

so following this suggestion creates code that doesnt differentiate between `None` and an empty string :(

---

_Comment by @charliermarsh on 2023-04-03 13:55_

Yeah this is a known limitation of this rule unfortunately -- see, e.g., https://github.com/charliermarsh/ruff/issues/3503. Pylint has the same problem. I've considered removing it for this reason.

You can restructure your code in various ways to ignore the lint, or disable the rule, but the rule itself can't really be improved without being made useless:

```py
def foo(a: str | None):
    if a is None:
        print('none')
    if not a:
        print('empty string')
```

Or:

```py
def foo(a: str | None):
    if not a:
        if isinstance(a, str):
            print('empty string')
        else:
            print('none')
```

(I'm not suggesting that these are better in any subjective sense, only that they avoid the issue.)

---

_Comment by @NJindia on 2023-04-24 15:32_

Going to echo that I think the rule is a bit of an issue. My team prefers to declare falsey values explicitly to avoid being unsure of `None` vs `""` and thus have to ignore this rule, which is fine, but is the functionality to enforce explicit checks of falsey values possible? i.e. finding an error when code _passes_ PLC1901 instead of when it fails.

Thanks

---
