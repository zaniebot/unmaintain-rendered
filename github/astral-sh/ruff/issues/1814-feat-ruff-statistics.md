---
number: 1814
title: "feat: ruff --statistics"
type: issue
state: closed
author: spaceone
labels:
  - good first issue
  - cli
assignees: []
created_at: 2023-01-12T11:27:33Z
updated_at: 2023-01-29T18:44:57Z
url: https://github.com/astral-sh/ruff/issues/1814
synced_at: 2026-01-07T13:12:14-06:00
---

# feat: ruff --statistics

---

_Issue opened by @spaceone on 2023-01-12 11:27_

`flake8` provides a nice `--statistics` flags which summarizes the findings at the end with a list like the following:

```
3     E126 continuation line over-indented for hanging indent
1     E225 missing whitespace around operator
3     E231 missing whitespace after ','
5     E701 multiple statements on one line (colon)
2     E711 comparison to None should be 'if cond is None:'
1     E714 test for object identity should be 'is not'
8     E999 SyntaxError: invalid syntax
40    F541 f-string is missing placeholders
5     F821 undefined name 'x'
```

It would be nice to have this as well.


---

_Label `cli` added by @charliermarsh on 2023-01-14 01:48_

---

_Label `good first issue` added by @charliermarsh on 2023-01-17 21:53_

---

_Comment by @edgarrmondragon on 2023-01-19 00:03_

`flake8 --statistics` doesn't play too well with custom formatters, and for example, produce invalid output with `--format=json`:

```console
$ flake8 --select B resources/test/fixtures/flake8_bugbear/B002.py --statistics
resources/test/fixtures/flake8_bugbear/B002.py:15:9: B002 Python does not support the unary prefix increment. Writing ++n is equivalent to +(+(n)), which equals n. You meant n += 1.
resources/test/fixtures/flake8_bugbear/B002.py:20:12: B002 Python does not support the unary prefix increment. Writing ++n is equivalent to +(+(n)), which equals n. You meant n += 1.
2     B002 Python does not support the unary prefix increment. Writing ++n is equivalent to +(+(n)), which equals n. You meant n += 1.

$ flake8 --select B resources/test/fixtures/flake8_bugbear/B002.py --format json | jq
{
  "resources/test/fixtures/flake8_bugbear/B002.py": [
    {
      "code": "B002",
      "filename": "resources/test/fixtures/flake8_bugbear/B002.py",
      "line_number": 15,
      "column_number": 9,
      "text": "Python does not support the unary prefix increment. Writing ++n is equivalent to +(+(n)), which equals n. You meant n += 1.",
      "physical_line": "    x = ++n\n"
    },
    {
      "code": "B002",
      "filename": "resources/test/fixtures/flake8_bugbear/B002.py",
      "line_number": 20,
      "column_number": 12,
      "text": "Python does not support the unary prefix increment. Writing ++n is equivalent to +(+(n)), which equals n. You meant n += 1.",
      "physical_line": "    return ++n\n"
    }
  ]
}

$ flake8 --select B resources/test/fixtures/flake8_bugbear/B002.py --statistics --format json
{"resources/test/fixtures/flake8_bugbear/B002.py": [{"code": "B002", "filename": "resources/test/fixtures/flake8_bugbear/B002.py", "line_number": 15, "column_number": 9, "text": "Python does not support the unary prefix increment. Writing ++n is equivalent to +(+(n)), which equals n. You meant n += 1.", "physical_line": "    x = ++n\n"}, {"code": "B002", "filename": "resources/test/fixtures/flake8_bugbear/B002.py", "line_number": 20, "column_number": 12, "text": "Python does not support the unary prefix increment. Writing ++n is equivalent to +(+(n)), which equals n. You meant n += 1.", "physical_line": "    return ++n\n"}]2     B002 Python does not support the unary prefix increment. Writing ++n is equivalent to +(+(n)), which equals n. You meant n += 1.}%
```

`ruff --statistics` could improve this by sending the stats summary to stderr, so that the output can be safely piped.

---

_Referenced in [astral-sh/ruff#2092](../../astral-sh/ruff/pulls/2092.md) on 2023-01-22 20:27_

---

_Referenced in [astral-sh/ruff#2284](../../astral-sh/ruff/pulls/2284.md) on 2023-01-28 01:08_

---

_Closed by @charliermarsh on 2023-01-29 18:44_

---
