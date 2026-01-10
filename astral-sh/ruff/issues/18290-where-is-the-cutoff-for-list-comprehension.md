```yaml
number: 18290
title: Where is the cutoff for list comprehension suggestion from PERF401
type: issue
state: closed
author: kaddkaka
labels:
  - question
assignees: []
created_at: 2025-05-24T16:44:15Z
updated_at: 2025-05-28T07:51:46Z
url: https://github.com/astral-sh/ruff/issues/18290
synced_at: 2026-01-10T11:09:58Z
```

# Where is the cutoff for list comprehension suggestion from PERF401

---

_Issue opened by @kaddkaka on 2025-05-24 16:44_

### Question

How advanced is the check for PERF401. For these two for loops ruff does *not* give the suggestion for example 1, but it does for example 2.
```
def func1(a):
    connections = []

    for left, _right in a:
        connections.append(left.get_connection())
    return connections


def func2(a):
    connections = []

    for left in a:
        connections.append(left.get_connection())  #  Ruff: Use a list comprehension to create a transformed list [PERF401]
    return connections
```

It seems reasonable to do example 1 as a list comprehension imo, but I'm also afraid that this kind of detection can get infinitely complex and for too complexes cases a regular for loop might be easier to read for a human.

### Version

_No response_

---

_Label `question` added by @kaddkaka on 2025-05-24 16:44_

---

_Comment by @MichaReiser on 2025-05-25 10:24_

Yeah, PERF401 only supports simple for loop targets, see here

https://github.com/astral-sh/ruff/blob/9ae698fe30cf3526f0e7ae237b800b3ed19a819f/crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs#L102-L108

---

_Closed by @MichaReiser on 2025-05-28 07:51_

---
