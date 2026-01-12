```yaml
number: 13527
title: Do not offer an invalid fix for PLR1716 when the comparisons contain parenthesis
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/plr1716-paren
created_at: 2024-09-26T15:01:09Z
updated_at: 2024-09-26T19:10:15Z
url: https://github.com/astral-sh/ruff/pull/13527
synced_at: 2026-01-12T15:55:44Z
```

# Do not offer an invalid fix for PLR1716 when the comparisons contain parenthesis

---

_@zanieb_

Related to https://github.com/astral-sh/ruff/issues/13524

Doesn't offer a valid fix, opting to instead just not offer a fix at all. If someone points me to a good way to handle parenthesis here I'm down to try to fix the fix separately, but it looks quite hard.

---

_Label `bug` added by @zanieb on 2024-09-26 15:01_

---

_Comment by @github-actions[bot] on 2024-09-26 15:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-09-26 15:19_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:94 on 2024-09-26 15:19_

This is probably fine considering that we're opting out of a fix but you can use `SimpleTokenizer` to get proper lexing. But the solution might actually be to use `parenthesized_range` which gives you the parenthesized expression range.

---

_@zanieb reviewed on 2024-09-26 16:11_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:94 on 2024-09-26 16:11_

Ah `parenthesized_range` is my dream

---

_@zanieb reviewed on 2024-09-26 16:23_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:94 on 2024-09-26 16:23_

This still looks hard to get quite right. What do you think about patching the bug then dealing with adding a fix later?

---

_Marked ready for review by @zanieb on 2024-09-26 16:23_

---

_@MichaReiser reviewed on 2024-09-26 16:31_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:94 on 2024-09-26 16:31_

I would need to look into what's needed to fix this but I would prefer using `parenthesized_range` to detect if the left or right are parenthesized if that's possible. 

---

_@zanieb reviewed on 2024-09-26 16:42_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:94 on 2024-09-26 16:42_

Sure, we can do that at least.

---

_@zanieb reviewed on 2024-09-26 16:48_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:94 on 2024-09-26 16:48_

Added that, and we'll offer a fix if they're both parenthesized.

---

_Renamed from "Do not offer an invalid fix for PLR1716 where the comparison contains parenthesis" to "Do not offer an invalid fix for PLR1716 where the comparison contains unbalanced parenthesis" by @zanieb on 2024-09-26 16:53_

---

_Renamed from "Do not offer an invalid fix for PLR1716 where the comparison contains unbalanced parenthesis" to "Do not offer an invalid fix for PLR1716 when the comparisons contain unbalanced parenthesis" by @zanieb on 2024-09-26 16:54_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:110 on 2024-09-26 18:15_

Does this work in the case of (((a < b))) and (b < c)?

---

_@MichaReiser reviewed on 2024-09-26 18:15_

---

_@zanieb reviewed on 2024-09-26 18:19_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:110 on 2024-09-26 18:19_

No, which brings me back to not wanting to offer a fix here — I just want to fix the syntax bug and find a way to introduce a correct fix again separately.

---

_Renamed from "Do not offer an invalid fix for PLR1716 when the comparisons contain unbalanced parenthesis" to "Do not offer an invalid fix for PLR1716 when the comparisons contain parenthesis" by @zanieb on 2024-09-26 18:21_

---

_@MichaReiser reviewed on 2024-09-26 18:26_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:110 on 2024-09-26 18:26_

Yeah. I would just not offer a fix if either side is parenthesized (Or you could use the `parentheses_range` iterator if you want to detect the balanced case

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:96 on 2024-09-26 18:27_

Nit: You can do the `into` game if you want to
```suggestion
                left_compare.into(),
                expr_bool_op.into(),
```

---

_@MichaReiser approved on 2024-09-26 18:27_

Thanks

---

_Comment by @zanieb on 2024-09-26 18:35_

Thanks for the reviews!

---

_Merged by @zanieb on 2024-09-26 19:01_

---

_Closed by @zanieb on 2024-09-26 19:01_

---

_Branch deleted on 2024-09-26 19:01_

---
