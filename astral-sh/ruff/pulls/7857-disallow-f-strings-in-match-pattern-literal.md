```yaml
number: 7857
title: Disallow f-strings in match pattern literal
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: dhruv/fstring-disallowed-in-match-literal
created_at: 2023-10-09T08:00:09Z
updated_at: 2023-10-09T10:19:28Z
url: https://github.com/astral-sh/ruff/pull/7857
synced_at: 2026-01-12T02:32:41Z
```

# Disallow f-strings in match pattern literal

---

_Pull request opened by @dhruvmanila on 2023-10-09 08:00_

## Summary

This PR fixes a bug to disallow f-strings in match pattern literal.

```
literal_pattern ::=  signed_number
                     | signed_number "+" NUMBER
                     | signed_number "-" NUMBER
                     | strings
                     | "None"
                     | "True"
                     | "False"
                     | signed_number: NUMBER | "-" NUMBER
```

Source: https://docs.python.org/3/reference/compound_stmts.html#grammar-token-python-grammar-literal_pattern

Also,

```console
$ python /tmp/t.py
  File "/tmp/t.py", line 4
    case "hello " f"{name}":
         ^^^^^^^^^^^^^^^^^^
SyntaxError: patterns may only match literals and attribute lookups
```

## Test Plan

Update existing test case and accordingly the snapshots. Also, add a new test case to verify that the parser does raise an error.


---

_Label `bug` added by @dhruvmanila on 2023-10-09 08:00_

---

_@dhruvmanila reviewed on 2023-10-09 08:02_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/python.lalrpop`:671 on 2023-10-09 08:02_

Quite a big diff for a one word change 😅 

---

_@konstin approved on 2023-10-09 08:19_

Could you add a test that f-strings in match raise an error?

---

_Merged by @dhruvmanila on 2023-10-09 10:11_

---

_Closed by @dhruvmanila on 2023-10-09 10:11_

---

_Branch deleted on 2023-10-09 10:11_

---

_Comment by @github-actions[bot] on 2023-10-09 10:19_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
