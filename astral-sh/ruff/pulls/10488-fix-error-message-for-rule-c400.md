```yaml
number: 10488
title: Fix error message for rule C400
type: pull_request
state: merged
author: VascoSch92
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-03-20T10:38:55Z
updated_at: 2024-03-20T13:30:53Z
url: https://github.com/astral-sh/ruff/pull/10488
synced_at: 2026-01-12T15:55:32Z
```

# Fix error message for rule C400

---

_@VascoSch92_

Fix a typos in the error message of rule C400

With the latest version of Ruff (0.3.3) if I have a `scratch.py` script like that:

```python
from typing import Dict, List, Tuple


def generate_samples(test_cases: Dict) -> List[Tuple]:
    return list(
        (input, expected)
        for input, expected in zip(test_cases["input_value"], test_cases["expected_value"])
    )
```

and I run ruff

```shell
>>> ruff check scratch.py --select C400
>>> scratch.py:5:12: C400 Unnecessary generator (rewrite using `list()`
```

This PR fixes the error message from _"(rewrite using `list()`"_ to _"(rewrite using `list()`)"_, and it fixes also the doc.

Related question: why I have this error message? The rule is not correct in this case. Should I open an issue for that?

---

_Comment by @github-actions[bot] on 2024-03-20 11:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-03-20 13:20_

Thank you

---

_Label `documentation` added by @MichaReiser on 2024-03-20 13:20_

---

_Comment by @MichaReiser on 2024-03-20 13:22_

> Related question: why I have this error message? The rule is not correct in this case. Should I open an issue for that?

I think the idea is that you write this as 

```python
def generate_samples(test_cases: Dict) -> List[Tuple]:
    return [
        (input, expected)
        for input, expected in zip(test_cases["input_value"], test_cases["expected_value"])
    ]

```

But I might be wrong (I don't know Python well). If t

---

_Merged by @MichaReiser on 2024-03-20 13:22_

---

_Closed by @MichaReiser on 2024-03-20 13:22_

---

_Comment by @VascoSch92 on 2024-03-20 13:28_

> > Related question: why I have this error message? The rule is not correct in this case. Should I open an issue for that?
> 
> I think the idea is that you write this as
> 
> ```python
> def generate_samples(test_cases: Dict) -> List[Tuple]:
>     return [
>         (input, expected)
>         for input, expected in zip(test_cases["input_value"], test_cases["expected_value"])
>     ]
> ```
> 
> But I might be wrong (I don't know Python well). If t

@MichaReiser 

But in this case the error message is not so useful right? It should be something like` (rewrite using [...])`

Should I open an issue to discuss that? 

Because there are also problem with the related rule `C416`. I think I have a user-case where this happen

---

_Branch deleted on 2024-03-20 13:28_

---

_Comment by @MichaReiser on 2024-03-20 13:30_

> But in this case the error message is not so useful right? It should be something like (rewrite using [...])

That's true and I think that's probably a bug. The rule has a second branch that suggests rewriting the rule as a generator.

> Should I open an issue to discuss that?

That would be great

---
