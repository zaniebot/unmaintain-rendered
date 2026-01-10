```yaml
number: 15605
title: "feat: Update RUF055 to do var == value"
type: pull_request
state: merged
author: kiran-4444
labels:
  - fixes
assignees: []
merged: true
base: main
head: ruf055-fix
created_at: 2025-01-20T06:02:30Z
updated_at: 2025-01-21T13:47:07Z
url: https://github.com/astral-sh/ruff/pull/15605
synced_at: 2026-01-10T20:05:43Z
```

# feat: Update RUF055 to do var == value

---

_Pull request opened by @kiran-4444 on 2025-01-20 06:02_

This commit fixes RUF055 rule to format `re.fullmatch(pattern, var)` to `var == pattern` instead of the current `pattern == var` behaviour. This is more idiomatic and easy to understand.

## Summary

This changes the current formatting behaviour of `re.fullmatch(pattern, var)` to format it to `var == pattern` instead of `pattern == var`.

## Test Plan

I used a code file locally to see the updated formatting behaviour.

Fixes https://github.com/astral-sh/ruff/issues/14733


---

_Comment by @github-actions[bot] on 2025-01-20 06:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @MichaReiser on 2025-01-20 09:47_

---

_Label `fixes` added by @MichaReiser on 2025-01-20 09:47_

---

_Comment by @MichaReiser on 2025-01-20 09:47_

@ntBre would you mind reviewing this one? 

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:119 on 2025-01-20 17:37_

I think this can be deleted?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:265 on 2025-01-20 17:53_

I think you can just in-line `self.compare_expr` here and swap `left` and `comparators`. That seems easier than calling `compare_expr` and unpacking the `Expr` again. This is basically what you have in your `Some(Expr::Compare...` but saves you from having to call `self.compare_expr` and then `if let`.

After that, I think you could delete the comment too since the implementation will now be consistent with the `string == pattern` comment above. It looks like I had the right idea with that comment but implemented it backwards :sweat_smile: 

You may not want to apply this change through GitHub because I think the indentation is wrong, but I thought an example might help.

```suggestion
                    Some(Expr::Compare(ExprCompare {
                        range: TextRange::default(),
                        left: Box::new(self.string.clone()),
                        ops,
                        comparators: Box::new([self.pattern.clone()]),
                    }))
```

---

_@ntBre requested changes on 2025-01-20 17:54_

This looks great, thanks for fixing this! I had a couple of small suggestions, but otherwise this looks good to me.

---

_@kiran-4444 reviewed on 2025-01-21 09:32_

---

_Review comment by @kiran-4444 on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:119 on 2025-01-21 09:32_

oh yes, missed it.

---

_@kiran-4444 reviewed on 2025-01-21 09:32_

---

_Review comment by @kiran-4444 on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:265 on 2025-01-21 09:32_

This is nice. I overlooked this way of implementation. Corrected in my recent commit.

---

_@ntBre approved on 2025-01-21 13:16_

This looks great, thank you again!

---

_Merged by @ntBre on 2025-01-21 13:47_

---

_Closed by @ntBre on 2025-01-21 13:47_

---
