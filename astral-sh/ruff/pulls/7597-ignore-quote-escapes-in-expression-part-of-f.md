```yaml
number: 7597
title: Ignore quote escapes in expression part of f-string
type: pull_request
state: merged
author: dhruvmanila
labels:
  - formatter
  - python312
assignees: []
merged: true
base: dhruv/pep-701
head: dhruv/formatter-fstring-quote
created_at: 2023-09-22T13:07:52Z
updated_at: 2023-09-28T03:58:59Z
url: https://github.com/astral-sh/ruff/pull/7597
synced_at: 2026-01-12T02:39:10Z
```

# Ignore quote escapes in expression part of f-string

---

_Pull request opened by @dhruvmanila on 2023-09-22 13:07_

## Summary

This PR fixes the following issues w.r.t. the PEP 701 changes:
1. Mark all unformatted comments inside f-strings as formatted only _after_ the
   f-string has been formatted.
2. Do not escape or remove the quote escape when normalizing the expression
   part of a f-string.

This PR also updates the `--files-with-errors` number to be 1 less. This is
because we can now parse the [`test_fstring.py`](https://discord.com/channels/1039017663004942429/1082324263199064206/1154633274887516254)
file in the CPython repository which contains the new f-string syntax. This is
also the file which updates the similarity index for CPython compared to main.

## Test Plan

`cargo test -p ruff_python_formatter`

### Ecosystem checks

#### `dhruv/formatter-fstring-quote`

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76051 |              1789 |              1632 |
| django       |           0.99983 |              2760 |                36 |
| transformers |           0.99963 |              2587 |               323 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99979 |              3496 |                22 |
| warehouse    |           0.99967 |               648 |                15 |
| zulip        |           0.99972 |              1437 |                21 |

#### `main`

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1631 |
| django       |           0.99983 |              2760 |                36 |
| transformers |           0.99963 |              2587 |               323 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99979 |              3496 |                22 |
| warehouse    |           0.99967 |               648 |                15 |
| zulip        |           0.99972 |              1437 |                21 |


---

_Comment by @dhruvmanila on 2023-09-22 13:08_

Current dependencies on/for this PR:
* main
  * **PR #7376** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7376" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7690** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7690" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7667** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7667" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7597** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7597" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #7588** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7588" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #7589** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7589" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7586** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7586" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7515** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7515" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7477** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7477" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7387** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7387" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7378** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7378" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7329** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7329" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7328** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7328" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7327** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7327" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7326** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7326" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7325** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7325" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/7597?utm_source=stack-comment).

---

_Label `formatter` added by @dhruvmanila on 2023-09-22 13:12_

---

_Label `python312` added by @dhruvmanila on 2023-09-22 13:12_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/string.rs`:790 on 2023-09-22 13:35_

Should we make `chars` peekable?

---

_@charliermarsh reviewed on 2023-09-22 13:35_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:174 on 2023-09-22 13:35_

Nit: That works, but may incorrectly mark unformatted comments of the fstring as formatted too (which we should handle). I would prefer if we itrate over the values instead and call the function for each value.

---

_@charliermarsh approved on 2023-09-22 13:37_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:786 on 2023-09-22 13:37_

Does this work with nested formatted values? 

```
f"{expression + f'a{other} + boom '}"
```

---

_@MichaReiser reviewed on 2023-09-22 13:38_

---

_@dhruvmanila reviewed on 2023-09-22 13:44_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/expression/string.rs`:786 on 2023-09-22 13:44_

Good point. I guess we'll need a counter.

On a side note, your example works but the following will fail:
```python
f"{f'{expr}\'foo\''}"
```

---

_Review requested from @MichaReiser by @dhruvmanila on 2023-09-22 17:21_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:790 on 2023-09-25 07:34_

I don't have any evidence for this but I tried to avoid `peekable` because it leads to an additional write (to the peelable internal memory) and a conditional lookup, which can be expensive in a hot loop... But the compiler might be able to optimize it away. 

We could also use `Cursor` here which avoids this problem too

---

_@MichaReiser reviewed on 2023-09-25 07:34_

---

_@MichaReiser approved on 2023-09-25 07:35_

---

_Merged by @dhruvmanila on 2023-09-25 07:36_

---

_Closed by @dhruvmanila on 2023-09-25 07:36_

---

_Branch deleted on 2023-09-25 07:36_

---
