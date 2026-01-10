```yaml
number: 19935
title: "`is_empty_f_string` causes false positives in PT015"
type: issue
state: closed
author: dscorbett
labels:
  - bug
assignees: []
created_at: 2025-08-16T12:54:40Z
updated_at: 2025-08-29T22:12:56Z
url: https://github.com/astral-sh/ruff/issues/19935
synced_at: 2026-01-10T11:09:59Z
```

# `is_empty_f_string` causes false positives in PT015

---

_Issue opened by @dscorbett on 2025-08-16 12:54_

### Summary

[`is_empty_f_string`](https://github.com/astral-sh/ruff/blob/0.12.9/crates/ruff_python_ast/src/helpers.rs#L1409) categorizes some f-string interpolations as empty strings which are non-empty strings, causing false positives in [`pytest-assert-always-false` (PT015)](https://docs.astral.sh/ruff/rules/pytest-assert-always-false/). [Example](https://play.ruff.rs/8d4dd448-d037-46ff-9a4b-e73fca1e7153):
```console
$ cat >pt015.py <<'# EOF'
assert f"{b""}"
assert f"{""=}"
assert f"{""!a}"
assert f"{""!r}"
assert f"{"":1}"
# EOF

$ python pt015.py; echo $?
0

$ ruff --isolated check pt015.py --select PT015 --output-format concise -q
pt015.py:1:1: PT015 Assertion always fails, replace with `pytest.fail()`
pt015.py:2:1: PT015 Assertion always fails, replace with `pytest.fail()`
pt015.py:3:1: PT015 Assertion always fails, replace with `pytest.fail()`
pt015.py:4:1: PT015 Assertion always fails, replace with `pytest.fail()`
pt015.py:5:1: PT015 Assertion always fails, replace with `pytest.fail()`

$ sed 's/assert \(.*\)/print(\1 + f": {len(\1)}")/' pt015.py | python
b'': 3
""='': 5
'': 2
'': 2
 : 1
```

### Version

ruff 0.12.9 (ef422460d 2025-08-14)

---

_Label `bug` added by @ntBre on 2025-08-18 18:00_

---

_Label `rule` added by @ntBre on 2025-08-18 18:00_

---

_Label `rule` removed by @ntBre on 2025-08-18 18:00_

---

_Closed by @dylwil3 on 2025-08-29 22:12_

---
