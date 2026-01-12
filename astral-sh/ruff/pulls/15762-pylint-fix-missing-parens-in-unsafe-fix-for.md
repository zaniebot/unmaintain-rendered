```yaml
number: 15762
title: "[`pylint`] Fix missing parens in unsafe fix for `unnecessary-dunder-call` (`PLC2801`)"
type: pull_request
state: merged
author: mishamsk
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: 15745-fix-PLC2801-missing-parens
created_at: 2025-01-27T03:07:10Z
updated_at: 2025-02-04T23:16:46Z
url: https://github.com/astral-sh/ruff/pull/15762
synced_at: 2026-01-12T15:55:52Z
```

# [`pylint`] Fix missing parens in unsafe fix for `unnecessary-dunder-call` (`PLC2801`)

---

_@mishamsk_

## Summary

This fixes #15745

Also, I haven't found a trait/helper to get expression precedence. Even though at least 3 rules check precedence when generating fixes. Generally speaking, unless/until fixes are done via AST generation - many (all?) fixes involving expression rewrites would need to consult precedence to handle parens. Unless I am mistaken.

Thus, I've implemented an enum and a trait. But I didn't dare doing that in semantic/AST crates, although I believe it would be helpful to move them there.

@AlexWaygood @dylwil3 you'll know the codebase better, let me know if you think it should be elevated to some core base crate. If you do, I am happy to implement this, but will need your guidance on where you'd like it to reside.

## Test Plan

* Updated test case with 2 examples from the issue + a couple of mine, including one that would generate invalid python program before this fix
* cargo nextest run (check updated snapshot)


---

_Comment by @github-actions[bot] on 2025-01-27 03:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Assigned to @dylwil3 by @dylwil3 on 2025-01-29 17:30_

---

_Renamed from "[pylint] Fix missing parens in unsafe fix (PLC2801)" to "[`pylint`] Fix missing parens in unsafe fix for `unnecessary-dunder-call` (`PLC2801`)" by @dylwil3 on 2025-01-30 14:01_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs`:599 on 2025-02-03 07:37_

What's the motivation for assigning fixed numbers to the different precedence values? I don't this is necessary because `ExprPrecedence` is an internal enum -- changing the values isn't user visible.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs`:610 on 2025-02-03 07:40_

I'd avoid a custom trait here and instead implement `From<&Expr>` for ExprPrecedence`, as well as `From<Operator>`, `From<UnaryOp>` and `From<BoolOp>`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs`:599 on 2025-02-03 07:43_

Could we align the names (enum and variants) with what we have in the [formatter](https://github.com/astral-sh/ruff/blob/ef85c682bdd76ed63457bdfbec72e09424ac20ab/crates/ruff_python_formatter/src/expression/mod.rs#L1141-L1155) and [parser](https://github.com/astral-sh/ruff/blob/138e70bd5c01d61061247c43b1bbb33d0290a18f/crates/ruff_python_parser/src/parser/expression.rs#L2366-L2397). It should simplify unifying the enums long-term

---

_@MichaReiser approved on 2025-02-03 07:44_

Thanks. This overall looks good. Extracting a common `OperatorPrecedence` enum into the `ast` crate seems like a good idea because this PR now adds the third version of `OperatorPrecedence` enum but I suggest that we do this as a follow up PR. For now, I recommend aligning the enum and variant names with the other `OperatorPrecedence` enums

---

_@mishamsk reviewed on 2025-02-04 02:43_

---

_Review comment by @mishamsk on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs`:599 on 2025-02-04 02:43_

Prefer being explicit (value) over implicit (position) when it doesn't have major downsides. THe thinking was - since Python precedence levels aren't likely to change these numbers would hold, and on the upside - reordering variants would break anything.

But that's speculative. I removed the values and repr, added a comment similar to the parser code too.

---

_@mishamsk reviewed on 2025-02-04 02:58_

---

_Review comment by @mishamsk on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs`:610 on 2025-02-04 02:58_

I thought about it. The reason I went a custom trait route is that `From` either encourages to either use `into()` which is less readable outside of IDE (when types are not apparent), or would force writing `ExprPrecedence::from(some_node)` which is readable but longer. I expect compiler to emit exact same code for both.

Interestingly enough, both parser and formatter code include the same method for one specific expression too

https://github.com/astral-sh/ruff/blob/138e70bd5c01d61061247c43b1bbb33d0290a18f/crates/ruff_python_parser/src/parser/expression.rs#L2427-L2435

but rely on `OperatorPrecedence::from(some_node)` in all other cases.

I am not married to the custom trait idea, if you have a strong opinion here - I can easily change that. Either in this PR, or as a follow-up while moving the whole thing to AST module.

---

_@mishamsk reviewed on 2025-02-04 03:03_

---

_Review comment by @mishamsk on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs`:599 on 2025-02-04 03:03_

First of all, I do not think that having even 3 enums is necessary bad. At least two separate ones make sense:

* Formatter enum is indeed an operator precedence one, as it is purpose is to split operators
* The one I've created and the parser one are both expression precedence and I would keep that naming. 
  * But unifying the parser one would go against @dhruvmanila comment, that explicitly excludes some expressions. I didn't investigate why, but I trust there were good reasons to do so

As for variant names - tbh. I like the shorter naming that aligns with Python docs better (formatter & mine), but since my enum logically closer to parser I changed names to mostly match parser version, where possible.

---

_@dhruvmanila reviewed on 2025-02-04 08:47_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs`:599 on 2025-02-04 08:47_

For the parser, the enum only contains precedence levels for the operators and not for all expressions. The main reason is that the parser utilizes the Pratt parsing algorithm to parse binary expressions which is the main use case for this enum. While, the other expressions are parsed normally and implemented as separate methods. But, that doesn't necessarily mean that it can't be unified to include other variants as well. We'll just have to be careful that the parser starts the binary expression parsing from the correct precedence level.

---

_Label `bug` added by @dhruvmanila on 2025-02-04 08:48_

---

_Label `preview` added by @dhruvmanila on 2025-02-04 08:48_

---

_@MichaReiser reviewed on 2025-02-04 09:47_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs`:610 on 2025-02-04 09:47_

I removed the trait for now as it isn't needed when we move to the `ruff_python_ast` crate (because we can directly implement it on `Expr`). 

I also renamed the enum to `OperatorPrecedence` to better match what we have in other crates.

---

_@MichaReiser approved on 2025-02-04 09:49_

---

_Merged by @MichaReiser on 2025-02-04 09:54_

---

_Closed by @MichaReiser on 2025-02-04 09:54_

---

_Branch deleted on 2025-02-04 23:16_

---
