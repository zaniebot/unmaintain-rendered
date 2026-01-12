```yaml
number: 16391
title: "[FURB156] Do not consider docstring(s)"
type: pull_request
state: merged
author: VascoSch92
labels:
  - bug
assignees: []
merged: true
base: main
head: furb-156
created_at: 2025-02-26T12:57:01Z
updated_at: 2025-02-26T16:33:18Z
url: https://github.com/astral-sh/ruff/pull/16391
synced_at: 2026-01-12T15:55:54Z
```

# [FURB156] Do not consider docstring(s)

---

_@VascoSch92_

The PR solves issue #16351 


I wanted to take the definition of docstring from PEP [PEP257](https://peps.python.org/pep-0257/#what-is-a-docstring), but it is quite strict, i.e., docstring are always with triple double quotes.

I relaxed the definition to accomodate also docstring just with double quotes. In this way, the bugs presented in the issue are resolved. 

Let me know what you think.

PS: I also mentioned in the description of the rule that the rule doesn't consider docstrings.

---

_@MichaReiser reviewed on 2025-02-26 12:59_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs`:56 on 2025-02-26 12:59_

I think you can use `checker.semantic().in_pep_257_docstring()` here instead. This should give us consistent behavior to other rules ignoring docstrings.

---

_Comment by @github-actions[bot] on 2025-02-26 13:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@VascoSch92 reviewed on 2025-02-26 13:31_

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs`:56 on 2025-02-26 13:31_

Yes, I was that method. The problems with this method are that 

```python
"""01234567"""

"01234567"
```

are not considered as docstrings and marked as violations. 

To fix all the issues reported in the issue, I had to use the current condition present in the PR.

But I can change it to `if checker.semantic().in_pep_257_docstring() {...}` if you want to be consistent between docstrings ;-) 

---

_Review requested from @MichaReiser by @VascoSch92 on 2025-02-26 13:31_

---

_@VascoSch92 reviewed on 2025-02-26 13:41_

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs`:56 on 2025-02-26 13:41_

Actually I double checked and just the second string in my above example will be marked as an error as it is not at the top of the module. So it makes sense to use: `in_pep_257_docstring()`.

I will change 

---

_Label `bug` added by @ntBre on 2025-02-26 13:58_

---

_@MichaReiser approved on 2025-02-26 16:26_

---

_Merged by @MichaReiser on 2025-02-26 16:30_

---

_Closed by @MichaReiser on 2025-02-26 16:30_

---

_Branch deleted on 2025-02-26 16:31_

---
