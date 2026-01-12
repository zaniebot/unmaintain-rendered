```yaml
number: 7332
title: Break before slice colon
type: pull_request
state: closed
author: konstin
labels:
  - formatter
assignees: []
base: main
head: break-before-slice-colon
created_at: 2023-09-13T10:02:27Z
updated_at: 2023-09-19T12:13:58Z
url: https://github.com/astral-sh/ruff/pull/7332
synced_at: 2026-01-12T02:39:09Z
```

# Break before slice colon

---

_Pull request opened by @konstin on 2023-09-13 10:02_

**Summary** Break slices at the colon first, since the colon is separator with the lowest precedence and we're in a parenthesized context.

**Input**
```python
section_header_data = byte_array[byte_begin_index + byte_step_index * event_index : byte_begin_index + byte_step_index * (event_index + 1)]
```
**Black**
```python
section_header_data = byte_array[
    byte_begin_index
    + byte_step_index * event_index : byte_begin_index
    + byte_step_index * (event_index + 1)
]
```
**Current formatting**
```python
section_header_data = byte_array[
    byte_begin_index + byte_step_index * event_index : byte_begin_index
    + byte_step_index * (event_index + 1)
]
```
**Proposed formatting**
```python
section_header_data = byte_array[
    byte_begin_index + byte_step_index * event_index 
    : byte_begin_index + byte_step_index * (event_index + 1)
]
```

This is another intentional black deviation, but i find it a clear style improvement.

This is consistent with adding a step:
```python
section_header_data2 = byte_array[
    byte_begin_index + byte_step_index * event_index 
    : byte_begin_index + byte_step_index 
    : section_size
]
```

As-is, this regresses trailing colon comments:

**in**
```python
c1 = "c"[
    1:  # e
    # f
    2
]
```
**out**
```python
c1 = "c"[
    1
    :  # e
    # f
    2
]
```

**main**

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1632 |
| **django**       |           0.99981 |              2760 |                40 |
| **transformers** |           0.99944 |              2587 |               413 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99983 |              3496 |                18 |
| warehouse    |           0.99834 |               648 |                20 |
| **zulip**        |           0.99956 |              1437 |                23 |

**PR**

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1632 |
| **django**       |           0.99981 |              2760 |                41 |
| **transformers** |           0.99942 |              2587 |               430 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99983 |              3496 |                18 |
| warehouse    |           0.99834 |               648 |                20 |
| **zulip**        |           0.99956 |              1437 |                24 |

For the similarity index, this is a regression.

Fixes #7316

**Test Plan** Added the fixtures above.

---

_Label `formatter` added by @konstin on 2023-09-13 10:02_

---

_Comment by @MichaReiser on 2023-09-13 10:08_

Nice! Does this impact our similarity index? How would it look if we intended the right side if the line breaks?

---

_@MichaReiser reviewed on 2023-09-13 10:10_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_slice.rs`:94 on 2023-09-13 10:10_

We should move this into the `else` branch or we may end up with an unintentional a `\n<space>` 

---

_@MichaReiser reviewed on 2023-09-13 10:11_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@expression__slice.py.snap`:178 on 2023-09-13 10:11_

Hmm, this isn't ideal. Do we need to group the line break with the left side so that it only renders if the left expands (or the `:` and the right side don't fit on the line)

---

_Comment by @konstin on 2023-09-19 12:13_

We have decided to not merge this to instead focus on black compatibility instead of adding more deviations

---

_Closed by @konstin on 2023-09-19 12:13_

---
