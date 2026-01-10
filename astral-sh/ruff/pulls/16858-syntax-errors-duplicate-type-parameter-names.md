```yaml
number: 16858
title: "[syntax-errors] Duplicate type parameter names"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: brent/syn-duplicate-type-params
created_at: 2025-03-19T21:05:00Z
updated_at: 2025-03-21T19:06:23Z
url: https://github.com/astral-sh/ruff/pull/16858
synced_at: 2026-01-10T19:40:36Z
```

# [syntax-errors] Duplicate type parameter names

---

_Pull request opened by @ntBre on 2025-03-19 21:05_

Summary
--

Detects duplicate type parameter names in function definitions, class
definitions, and type alias statements.

I also boxed the `type_params` field on `StmtTypeAlias` to make it easier to
`match` with functions and classes. (That's the reason for the red-knot code 
owner review requests, sorry!)

Test Plan
--

New `ruff_python_syntax_errors` unit tests.

Fixes #11119.

---

_Label `rule` added by @ntBre on 2025-03-19 21:05_

---

_Label `preview` added by @ntBre on 2025-03-19 21:05_

---

_Review requested from @carljm by @ntBre on 2025-03-19 21:05_

---

_Review requested from @AlexWaygood by @ntBre on 2025-03-19 21:05_

---

_Review requested from @sharkdp by @ntBre on 2025-03-19 21:05_

---

_Review requested from @dcreager by @ntBre on 2025-03-19 21:05_

---

_Review requested from @MichaReiser by @ntBre on 2025-03-19 21:05_

---

_Review requested from @dhruvmanila by @ntBre on 2025-03-19 21:05_

---

_Comment by @github-actions[bot] on 2025-03-19 21:11_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-03-19 21:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @carljm on 2025-03-19 23:21_

> (That's the reason for the red-knot code
> owner review requests, sorry!)

No worries! Please don't ever hesitate to change red-knot code for this reason, it's up to us to manage our CODEOWNERS setup and handle the consequences.

---

_Review request for @carljm removed by @carljm on 2025-03-19 23:21_

---

_Review comment by @MichaReiser on `crates/ruff_python_syntax_errors/src/lib.rs`:136 on 2025-03-20 08:08_

Not saying that we shouldn't change the definition but you could have written this as
```suggestion
        let type_params = match stmt {
            Stmt::FunctionDef(ast::StmtFunctionDef {
                type_params: Some(type_params),
                ..
            })
            | Stmt::ClassDef(ast::StmtClassDef {
                type_params: Some(type_params),
                ..
            }) => &**type_params,
            Stmt::TypeAlias(ast::StmtTypeAlias {
                type_params: Some(type_params),
                ..
            }) => type_params,
            _ => return,
        };
```

---

_Review comment by @MichaReiser on `crates/ruff_python_syntax_errors/src/lib.rs`:142 on 2025-03-20 08:12_

From the documentation, it's not clear to me which element `dedup_by_with_count` keeps if multiple elements map to the same key: Is it the first, the last, or any? 

We would want to flag all except the first, which I believe the implementation currently isn't handling (it only flags one).

In which case we probably can't use `dedup_by_with_count` anymore, in which case I would suggest writing this entire check without using itertools (I don't mind it but I also don't love it because it hides allocations)


```rust
if type_params.len() > 1 {
	let mut sorted = type_params.clone();
	sorted.sort_by_key(|t| &*t.name());

	let last_name = None;

	for type_param in sorted {
		if last_name.is_some_and(|last| last == type_param.name()) {
			// push diagnostic
		}
		last_name = Some(type_param.name());
	}
}
```

---

_Review comment by @MichaReiser on `crates/ruff_python_syntax_errors/src/lib.rs`:141 on 2025-03-20 08:13_

Should we return early if `type_params.len < 2` to avoid the "hidden" allocation in `sorted_by_key`. That should allow us to skip this check for most type params.

---

_Review comment by @MichaReiser on `crates/ruff_python_syntax_errors/src/lib.rs`:308 on 2025-03-20 08:18_

Let's add a few examples where:

* With more than two duplicate type params
* With non duplicate type params

---

_@MichaReiser approved on 2025-03-20 08:18_

---

_@dhruvmanila reviewed on 2025-03-20 11:29_

---

_Review comment by @dhruvmanila on `crates/ruff_python_syntax_errors/src/lib.rs`:126 on 2025-03-20 11:29_

We might also want to move the duplicate parameter check to the `SyntaxChecker` as that's also a compiler error. Currently, it's done by the parser and raises `ParseError` but the CPython parser does not raise it while parsing:

https://github.com/astral-sh/ruff/blob/c95ecde61ef2cdb85e86de38963326162d004ada/crates/ruff_python_parser/src/parser/statement.rs#L3622-L3639

Maybe we can just use a similar implementation here to avoid the `itertools` usage and can avoid allocation if the number of parameters is < 2 (from Micha's review comment).

---

_@dhruvmanila approved on 2025-03-20 11:32_

Looks good

---

_Review comment by @ntBre on `crates/ruff_python_syntax_errors/src/lib.rs`:142 on 2025-03-20 13:35_

Ah that's a good point. I assumed it was taking the first, which is why I reversed the list, but that doesn't handle the case with more than two duplicates. I also felt like I was going overboard with itertools here, so your implementation makes sense to me.

---

_@ntBre reviewed on 2025-03-20 13:35_

---

_@ntBre reviewed on 2025-03-21 14:32_

---

_Review comment by @ntBre on `crates/ruff_python_syntax_errors/src/lib.rs`:136 on 2025-03-21 14:32_

For sure, I'm happy with that too if we'd rather not change the generated code.

---

_@ntBre reviewed on 2025-03-21 14:53_

---

_Review comment by @ntBre on `crates/ruff_python_syntax_errors/src/lib.rs`:126 on 2025-03-21 14:53_

Added it to #11934!

---

_@ntBre reviewed on 2025-03-21 15:10_

---

_Review comment by @ntBre on `crates/ruff_python_syntax_errors/src/lib.rs`:142 on 2025-03-21 15:10_

I went with an inner linear search instead of sorting at all, but I'm happy to go with your approach if you prefer it.

---

_Comment by @ntBre on 2025-03-21 15:15_

I rebased this onto #16106, added more test cases (and converted them to inline tests), returned early with <2 parameters, and removed itertools.

---

_@MichaReiser approved on 2025-03-21 15:49_

---

_Merged by @ntBre on 2025-03-21 19:06_

---

_Closed by @ntBre on 2025-03-21 19:06_

---

_Branch deleted on 2025-03-21 19:06_

---
