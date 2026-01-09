---
number: 5691
title: "F601: Repeated tuple keys with different values are not detected"
type: issue
state: closed
author: MatthewHoadSWO
labels:
  - bug
assignees: []
created_at: 2023-07-11T16:20:16Z
updated_at: 2023-07-12T08:15:28Z
url: https://github.com/astral-sh/ruff/issues/5691
synced_at: 2026-01-07T13:12:15-06:00
---

# F601: Repeated tuple keys with different values are not detected

---

_Issue opened by @MatthewHoadSWO on 2023-07-11 16:20_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Duplicate tuple keys with different values in a dict are not detected. Not sure if `F601` is the correct rule to file this under since I'm unsure if tuples are technically literals. If not, then it would be really handy to have a rule for this. Flake8 seems to think that duplicate tuple keys fall under F601 though. Reproduced with ruff `0.0.269`, `0.0.273`, `0.0.275`, and `0.0.277`. 

To reproduce: 
Default ruff settings (no `pyproject.toml`)

```sh
>>> ruff --version
ruff 0.0.277
```

`f601_sample.py` 👇 
```python
my_dict = {
    ('a', 'b'): 'asdf',
    ('a', 'b'): 'qwer',
    'a': 'asdf',
    'a': 'qwer',
}
```

Ruff correctly detects the duplicated string literal key, but not the tuple key:
```sh
>>> ruff --isolated f601_sample.py
f601_sample.py:5:5: F601 Dictionary key literal `'a'` repeated
Found 1 error.
```

Flake8 detects both string and tuple key duplication:
```sh
>>> flake8 f601_sample.py 
f601_sample.py:2:5: F601 dictionary key ('a', 'b') repeated with different values
f601_sample.py:3:5: F601 dictionary key ('a', 'b') repeated with different values
f601_sample.py:4:5: F601 dictionary key 'a' repeated with different values
f601_sample.py:5:5: F601 dictionary key 'a' repeated with different values
```

---

_Label `bug` added by @dhruvmanila on 2023-07-11 16:23_

---

_Comment by @dhruvmanila on 2023-07-11 16:27_

Yeah, this should be supported. For anyone interested, a new `DictionaryKey` type would be added [here](https://github.com/astral-sh/ruff/blob/main/crates/ruff/src/rules/pyflakes/rules/repeated_keys.rs#L129) and the `match` expressions using this value would need to account for it [here](https://github.com/astral-sh/ruff/blob/main/crates/ruff/src/rules/pyflakes/rules/repeated_keys.rs#L135) and [here](https://github.com/astral-sh/ruff/blob/main/crates/ruff/src/rules/pyflakes/rules/repeated_keys.rs#L157).

---

_Comment by @charliermarsh on 2023-07-11 16:32_

I wonder why we have a dedicated type there rather than just using ComparableExpr.

---

_Comment by @dhruvmanila on 2023-07-11 16:59_

I haven't looked into `ComparableExpr` much but I think as dictionary keys needs to be hashable we can't consider all of `ComparableExpr` variants. Or, are you referring to something else?

---

_Comment by @charliermarsh on 2023-07-11 17:02_

I was suggesting just using ComparableExpr and removing DictionaryKey. If a key uses an unhashable Python type, like a set, that’s a problem regardless, right? But it’s not a big deal, it just seems simpler and would also catch things like attribute accesses and subscript accesses.

---

_Referenced in [astral-sh/ruff#5696](../../astral-sh/ruff/pulls/5696.md) on 2023-07-11 22:48_

---

_Closed by @charliermarsh on 2023-07-12 03:46_

---

_Comment by @MatthewHoadSWO on 2023-07-12 08:15_

Thanks @charliermarsh @dhruvmanila @qdegraaf for the speedy response and action!

---
