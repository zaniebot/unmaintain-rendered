```yaml
number: 19325
title: "[`perflint`] Parenthesize generator expressions (`PERF401`)"
type: pull_request
state: merged
author: CodeMan62
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: parenthesize-generator-expr
created_at: 2025-07-14T09:57:31Z
updated_at: 2025-07-23T16:08:15Z
url: https://github.com/astral-sh/ruff/pull/19325
synced_at: 2026-01-12T15:56:36Z
```

# [`perflint`] Parenthesize generator expressions (`PERF401`)

---

_@CodeMan62_

## Summary
closes #19204 

## Test Plan
1. test case is added in dedicated file
2. locally tested the code manually 

---

_Comment by @github-actions[bot] on 2025-07-14 10:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:413 on 2025-07-14 18:17_

nit: this is arguably a bit less readable since it duplicates the long `format!` string, but it avoids allocating a separate `String` in the `format!`/`to_string` calls for `elt_str`, which I think makes it worth it.

```suggestion
    let elt_str = locator.slice(to_append);
    let generator_str = if to_append.is_generator_expr() {
        format!("({elt_str}) {for_type} {target_str} in {for_iter_str}{if_str}")
    } else {
        format!("{elt_str} {for_type} {target_str} in {for_iter_str}{if_str}")
    };
```

---

_@ntBre approved on 2025-07-14 18:18_

Thanks! This looks good, just one suggestion to avoid allocating a second string.

---

_Label `bug` added by @ntBre on 2025-07-14 18:18_

---

_Label `fixes` added by @ntBre on 2025-07-14 18:18_

---

_Comment by @CodeMan62 on 2025-07-15 02:10_

Done as said

---

_Comment by @dscorbett on 2025-07-15 03:18_

Does this add extra parentheses if the generator expression is already parenthesized?

---

_Comment by @CodeMan62 on 2025-07-15 16:08_

I think it will, but I don't think it will affect the result. I have tested it 

---

_Comment by @ntBre on 2025-07-15 16:33_

> Does this add extra parentheses if the generator expression is already parenthesized?

That's a good point. Can we add a test case demonstrating this behavior? I think we should be able to use `parenthesized_range` to see if the expression already has parentheses and avoid adding new ones:

https://github.com/astral-sh/ruff/blob/d81ba585267adda7328b9c8ea28b4a815b9c398d/crates/ruff_python_ast/src/parenthesize.rs#L58-L61


---

_Comment by @CodeMan62 on 2025-07-18 15:40_

Sure we can and i am adding it!

---

_Review requested from @ntBre by @CodeMan62 on 2025-07-19 15:35_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:420 on 2025-07-23 15:35_

I forgot that generators have a `parenthesized` field already. What do you think about this?

```suggestion
    let generator_str = if to_append
        .as_generator_expr()
        .is_some_and(|generator| !generator.parenthesized)
    {
        format!("({elt_str}) {for_type} {target_str} in {for_iter_str}{if_str}")
    } else {
        format!("{elt_str} {for_type} {target_str} in {for_iter_str}{if_str}")
    };
```

---

_@ntBre approved on 2025-07-23 15:36_

Thanks! Just one more minor simplification suggestion.

---

_@CodeMan62 reviewed on 2025-07-23 15:56_

---

_Review comment by @CodeMan62 on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:420 on 2025-07-23 15:56_

huh it looks good let's see how CI goes 

---

_@ntBre approved on 2025-07-23 15:57_

---

_Renamed from "[`perflint`] Parenthesize generator expressions (PERF401)" to "[`perflint`] Parenthesize generator expressions (`PERF401`)" by @ntBre on 2025-07-23 16:08_

---

_Merged by @ntBre on 2025-07-23 16:08_

---

_Closed by @ntBre on 2025-07-23 16:08_

---
