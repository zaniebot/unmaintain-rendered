```yaml
number: 13325
title: pep8-naming N rules fail to find misnamed variables in generator/list comprehensions
type: issue
state: open
author: ember91
labels: []
assignees: []
created_at: 2024-09-11T11:12:55Z
updated_at: 2024-09-12T05:09:50Z
url: https://github.com/astral-sh/ruff/issues/13325
synced_at: 2026-01-12T15:54:53Z
```

# pep8-naming N rules fail to find misnamed variables in generator/list comprehensions

---

_@ember91_

## List of keywords searched for

 - N816

## Minimal code snippet

```python
(someVar1 == 8 for someVar1 in [])
[someVar2 == 8 for someVar2 in []]
someVar3 = 8
```

## Invoked command

This assumes the file with the content above is named `test.py`.

```bash
ruff check --isolated --select N816 test.py
```

## Current Ruff settings

None

## Current Ruff version

`0.6.4`

## Expectations

I expected all of the lines 1-3 to report error N816, but only line 3 did.

---

_Comment by @dhruvmanila on 2024-09-11 13:31_

Thanks for the report. The rule `N816` is only for the global variable names while in your example the `someVar1` and `someVar2` variables are in the comprehension. So, only the variables that are in the global scope are checked while a comprehension creates it's own scope.

---

_Comment by @ember91 on 2024-09-11 13:34_

Ok. That's my mistake. But still with `ruff check --isolated --select N test.py` there is no check for this case. Seems like something's missing here.

---

_Comment by @ember91 on 2024-09-11 13:37_

I tried this with:
- `flake8==7.1.1`
- `pep8-naming==0.14.1`

and got:
```
test.py:1:21: N816 variable 'someVar1' in global scope should not be mixedCase
test.py:2:21: N816 variable 'someVar2' in global scope should not be mixedCase
test.py:3:2: N816 variable 'someVar3' in global scope should not be mixedCase
```

Although I do understand that `someVar1` and `someVar2` most likely are not globals.

---

_Comment by @dhruvmanila on 2024-09-11 13:46_

> That's my mistake. But still with `ruff check --isolated --select N test.py` there is no check for this case. Seems like something's missing here.

Yeah, I don't think a check like that exists which could mainly be a side effect of having a 1-1 implementation with the original plugin in terms of rules.



> and got:
> 
> ```
> test.py:1:21: N816 variable 'someVar1' in global scope should not be mixedCase
> test.py:2:21: N816 variable 'someVar2' in global scope should not be mixedCase
> test.py:3:2: N816 variable 'someVar3' in global scope should not be mixedCase
> ```
> 
> Although I do understand that `someVar1` and `someVar2` most likely are not globals.

Interesting, that seems like a bug because, as you mentioned, those variables are not in the global scope. I can't really find an issue in the issue tracker of `flake8-naming` so I'm not sure why is this the case.

---

_Comment by @ember91 on 2024-09-11 14:29_

I think that either:
- Ruff is wrong and the check should be included in N816.
- flake8 is wrong according to them and the check should be excluded from N816. They move to another rule or create a new one. Ruff updates for compatibility.
- flake8 is right according to them but wrong according to Ruff. Ruff adds a new RUF rule for this.

---

_Comment by @dhruvmanila on 2024-09-11 20:50_

I think I'd be interested to know why is `pep8-naming` raising a violation for this scenario when the variable is not in the global scope. There do exist rules for other scopes like [function](https://docs.astral.sh/ruff/rules/non-lowercase-variable-in-function/) and [class](https://docs.astral.sh/ruff/rules/mixed-case-variable-in-class-scope/). Ruff's semantic model is aware of the fact that comprehensions create a new scope and it could be that `pep8-naming` isn't considering that.

---

_Comment by @ember91 on 2024-09-12 04:59_

I created this one to discuss it a bit: https://github.com/PyCQA/pep8-naming/issues/233

---

_Renamed from "N816 fails to find misnamed variables in generator/list comprehensions" to "pep8-naming N rules fails to find misnamed variables in generator/list comprehensions" by @ember91 on 2024-09-12 05:09_

---

_Renamed from "pep8-naming N rules fails to find misnamed variables in generator/list comprehensions" to "pep8-naming N rules fail to find misnamed variables in generator/list comprehensions" by @ember91 on 2024-09-12 05:09_

---
