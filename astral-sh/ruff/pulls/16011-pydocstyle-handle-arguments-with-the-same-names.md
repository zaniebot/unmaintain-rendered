```yaml
number: 16011
title: "[`pydocstyle`] Handle arguments with the same names as sections (`D417`)"
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - docstring
assignees: []
merged: true
base: main
head: brent/docstring-nested-attributes
created_at: 2025-02-07T04:48:44Z
updated_at: 2025-02-24T09:01:27Z
url: https://github.com/astral-sh/ruff/pull/16011
synced_at: 2026-01-10T19:49:01Z
```

# [`pydocstyle`] Handle arguments with the same names as sections (`D417`)

---

_Pull request opened by @ntBre on 2025-02-07 04:48_

## Summary

Fixes #16007. The logic from the last fix for this (#9427) was sufficient, it just wasn't being applied because `Attributes` sections aren't expected to have nested sections. I just deleted the outer conditional, which should hopefully fix this for all section types.

## Test Plan

New regression test, plus the existing D417 tests.


---

_Comment by @github-actions[bot] on 2025-02-07 04:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @MichaReiser on 2025-02-07 08:25_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pydocstyle/D417.py`:185 on 2025-02-07 08:30_

Would you mind adding a test where one of the function arguments is named like a section header? I'm curious what happens if that argument is undocumented.

---

_@MichaReiser reviewed on 2025-02-07 08:30_

---

_Label `docstring` added by @AlexWaygood on 2025-02-07 11:21_

---

_@ntBre reviewed on 2025-02-07 13:48_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pydocstyle/D417.py`:185 on 2025-02-07 13:48_

Is this what you had in mind? I added these two cases, both of which cause D417. The first one is right, `Args` is not documented, but the second is still a false positive that I can keep working on.

```python
# undocumented argument with the same name as a section
def f(payload, Args):
    """
    Send a message.

    Args:
        payload:
            The message payload.
    """


# documented argument with the same name as a section
def f(payload, Args):
    """
    Send a message.

    Args:
        payload:
            The message payload.

        Args:
            The other arguments.
    """
```

---

_@MichaReiser reviewed on 2025-02-07 14:01_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pydocstyle/D417.py`:185 on 2025-02-07 14:01_

Yes, that's what I had in mind

---

_@ntBre reviewed on 2025-02-07 16:53_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pydocstyle/D417.py`:185 on 2025-02-07 16:53_

These cases should work now! I initially deleted the inner conditional here
https://github.com/astral-sh/ruff/blob/1f7a29d3476b7c869789b68388daa4f4270374b2/crates/ruff_linter/src/docstrings/sections.rs#L525-L529
which worked well for D417 but broke other rules like [D214 - overindented-section](https://docs.astral.sh/ruff/rules/overindented-section/). So I went for the more involved approach of passing down a `Definition` and extracting known parameter names from it.

One other thing I considered is just searching for a known parameter at line 514 instead of collecting a `HashSet` up front.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/docstrings/sections.rs`:442 on 2025-02-09 15:37_

Having this at the top here is sort of confusing, considering that its only use is 70 lines further down. I suggest moving the logic closer to where it is used. I'd also argue that computing a hash set isn't worth it, considering that functions tend to have only a few parameters and we only perform one `contains` lookup at most (constructing a HashSet is `O(n)`, we can just do a linear search)

The alternative is to move the hash set calculation out of `is_docstring_section` and only run it once for the entire function. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/docstrings/sections.rs`:500 on 2025-02-09 15:39_

I think it would be good to add an example related to `known_args`.

---

_@MichaReiser approved on 2025-02-09 15:39_

---

_@ntBre reviewed on 2025-02-09 16:39_

---

_Review comment by @ntBre on `crates/ruff_linter/src/docstrings/sections.rs`:442 on 2025-02-09 16:39_

That makes sense to me. I factored out a function called `has_parameter` that does the linear search and call it in the conditional where it's actually needed.

---

_@ntBre reviewed on 2025-02-09 16:56_

---

_Review comment by @ntBre on `crates/ruff_linter/src/docstrings/sections.rs`:500 on 2025-02-09 16:56_

I added this to the docs discussing exact matches a few lines farther up (line 487), but I'm happy to move them here if you prefer.

---

_@MichaReiser reviewed on 2025-02-09 18:35_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/docstrings/sections.rs`:500 on 2025-02-09 18:35_

I think I'd prefer that because it is otherwise somewhat unclear on where this behavior is implemented.

---

_Merged by @ntBre on 2025-02-11 17:05_

---

_Closed by @ntBre on 2025-02-11 17:05_

---

_Branch deleted on 2025-02-11 17:05_

---

_Label `rule` removed by @dhruvmanila on 2025-02-24 09:01_

---

_Label `bug` added by @dhruvmanila on 2025-02-24 09:01_

---
