```yaml
number: 19045
title: "[`flake8-gettext`] Resolve qualified names and built-in bindings (`INT001`, `INT002`, `INT003`)"
type: pull_request
state: merged
author: robsdedude
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix/19028-flake8-gettext-false-negative
created_at: 2025-06-30T11:25:11Z
updated_at: 2025-10-21T05:32:08Z
url: https://github.com/astral-sh/ruff/pull/19045
synced_at: 2026-01-10T17:34:34Z
```

# [`flake8-gettext`] Resolve qualified names and built-in bindings (`INT001`, `INT002`, `INT003`)

---

_Pull request opened by @robsdedude on 2025-06-30 11:25_

## Summary
Make rules `INT001`, `INT002`, and `INT003` also 
* trigger on qualified names when we're sure the calls are calls to the `gettext` module. For example 
  ```python
  from gettext import gettext as foo
  
  foo(f"{'bar'}")  # very certain that this is a call to a real `gettext` function => worth linting
  ```
* trigger on `builtins` bindings
  ```python
  from builtins, gettext
  
  gettext.install("...")  # binds `gettext.gettext` to `builtins._`
  builtins.__dict__["_"] = ...  # also a common pattern

  _(f"{'bar'}")  # should therefore also be linted
  ```

Fixes: https://github.com/astral-sh/ruff/issues/19028

## Test Plan

Tests have been added to all three rules.


---

_Comment by @github-actions[bot] on 2025-06-30 11:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "[`flake8_gettext`]: Make `INTxxx` rules also trigger on aliased imports" to "[`flake8_gettext`]: Expand scope of `INTxxx` rules" by @robsdedude on 2025-06-30 21:59_

---

_Renamed from "[`flake8_gettext`]: Expand scope of `INTxxx` rules" to "[`flake8_gettext`] Expand scope of `INTxxx` rules" by @robsdedude on 2025-06-30 21:59_

---

_Comment by @codspeed-hq[bot] on 2025-06-30 22:08_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/robsdedude%3Afix%2F19028-flake8-gettext-false-negative)

### Merging #19045 will **not alter performance**

<sub>Comparing <code>robsdedude:fix/19028-flake8-gettext-false-negative</code> (da2ec00) with <code>main</code> (590ce9d)</sub>



### Summary

`✅ 30` untouched  
`⏩ 22` skipped[^skipped]  



[^skipped]: 22 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/robsdedude%3Afix%2F19028-flake8-gettext-false-negative?sectionId=benchmark-comparison-section-baseline-result-skipped).


---

_Marked ready for review by @robsdedude on 2025-07-01 06:30_

---

_Review requested from @ntBre by @MichaReiser on 2025-07-07 12:56_

---

_Label `rule` added by @MichaReiser on 2025-07-07 12:57_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_gettext/mod.rs`:22 on 2025-07-21 21:14_

```suggestion
    if let Some(name) = func.as_name_expr() {
        return functions_names.contains(name.id);
```

I think we can return here even if it's `Some` and _not_ `functions_names.contains(id)`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_gettext/mod.rs`:35 on 2025-07-21 21:19_

Do we need this `seen_module` check? I think we should still check `_` even if `builtins` hasn't been imported. I guess we will have already returned in the case of just `_` since it's a `Name`, but in that case we don't need to check `"" | "builtins"`, just `"builtins"`, if I'm following correctly.

---

_@ntBre reviewed on 2025-07-21 21:24_

Thanks, this looks good to me! Just a couple of small nits and the merge conflicts, if you don't mind resolving those.

---

_Label `preview` added by @ntBre on 2025-07-21 21:25_

---

_Comment by @MichaReiser on 2025-08-18 07:39_

@robsdedude do you think you get a chance to resolve the merge conflicts or would you prefer us to resolve them?

---

_Review requested from @ntBre by @MichaReiser on 2025-10-12 06:43_

---

_@robsdedude reviewed on 2025-10-12 08:33_

---

_Review comment by @robsdedude on `crates/ruff_linter/src/rules/flake8_gettext/mod.rs`:22 on 2025-10-12 08:33_

Oh, I didn't quite realize; but accepting the suggestion, some tests broke. This short circuits the logic too much. It won't work for something like this:

```python
from gettext import as gettext_fn

# this `gettext_fn` should be considered by the lint
gettext_fn(...)
```

---

_@robsdedude reviewed on 2025-10-12 08:52_

---

_Review comment by @robsdedude on `crates/ruff_linter/src/rules/flake8_gettext/mod.rs`:35 on 2025-10-12 08:52_

Yeah, you're right about that. Matching `""` as the first segment is pointless.

---

_Comment by @robsdedude on 2025-10-12 09:32_

Ok, this should be good for another review now. Sorry for the long wait. Life got in the way ;)

---

_Renamed from "[`flake8_gettext`] Expand scope of `INTxxx` rules" to "[`flake8-gettext`] Resolve qualified names and built-in bindings (`INT001`, `INT002`, `INT003`)" by @ntBre on 2025-10-20 22:23_

---

_@ntBre approved on 2025-10-20 22:24_

Thank you!

---

_Merged by @ntBre on 2025-10-20 22:24_

---

_Closed by @ntBre on 2025-10-20 22:24_

---

_Branch deleted on 2025-10-21 05:32_

---
