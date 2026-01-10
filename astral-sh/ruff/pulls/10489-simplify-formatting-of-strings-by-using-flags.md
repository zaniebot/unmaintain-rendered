```yaml
number: 10489
title: Simplify formatting of strings by using flags from the AST nodes
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: formatter-string-flags-2
created_at: 2024-03-20T12:29:21Z
updated_at: 2024-03-20T16:16:56Z
url: https://github.com/astral-sh/ruff/pull/10489
synced_at: 2026-01-10T22:47:02Z
```

# Simplify formatting of strings by using flags from the AST nodes

---

_Pull request opened by @AlexWaygood on 2024-03-20 12:29_

## Summary

This PR simplifies the formatter by using the flags available on AST nodes, which were added in https://github.com/astral-sh/ruff/pull/10298 and refined in https://github.com/astral-sh/ruff/pull/10314. These can be used to query the quoting style and prefix used by a string. (Currently, we compute these by reparsing them from the source code.)

I was hoping this might lead to a speedup -- locally for me, it seems to be performance-neutral. It's (in my opinion) a nice simplification, anyway.

Each commit is atomic; the full test suite passes on each commit.

## Test Plan

`cargo test`


---

_Label `internal` added by @AlexWaygood on 2024-03-20 12:29_

---

_Label `formatter` added by @AlexWaygood on 2024-03-20 12:29_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-03-20 12:29_

---

_Comment by @github-actions[bot] on 2024-03-20 12:48_

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

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1250 on 2024-03-20 12:58_

Nit. `self.0.set(FStringFlagsInner::Double, quote_style.is_double())`

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/mod.rs`:58 on 2024-03-20 13:04_

Nice

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/normalize.rs`:79 on 2024-03-20 13:09_

Using `impl` here means that all these methods get monomorphized for each `StringPart` type. It's not a ton of code but still considerable. 


The other downside is that `StringPart` is a type from the `ruff_python_parser` crate. Ideally, the formatter would only depend on the AST, but not the parser and would instead be given a parsed AST already (the formatter doesn't really care about parsing). 



That's why I think I would prefer keeping the `StringPart` enum (or you can have another struct that holds the relevant field and implements `From` methods for all string types). E.g. all we need is the `kind`, and `possibly` the range. So you could define a `StringPart` struct that stores `kind` and `range` and implements `From` for all possible string part values. This way you avoid monomorphization and depending on a rust parser crate type.





---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/any.rs`:9 on 2024-03-20 13:14_

We should move `StringKind` to `ruff_python_ast` if it is something that other crates should depend on. Ideally, the formatter only depends on `ruff_python_ast` and gets passed an AST, instead of calling parsing itself (because it doesn't really care about how the file was parsed, all it needs is the ast)

---

_@MichaReiser requested changes on 2024-03-20 13:16_

Nice. It allows us to delete quiet a lot of code. 

There are two concerns:

* Using `impl StringPart` leads to monomorphization of not entirely trivial functions -> More generated code. I recommend extracting a struct that holds just the information we need to avoid this
* We should move `StringKind` out of the parser crate because I want to make `ruff_python_formatter` independent of the parser crate eventually. It should only depend on the `ruff_python_ast` and not be concerned about how the source file was parsed (that will happen in a caller crate). 



---

_@AlexWaygood reviewed on 2024-03-20 15:24_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:2109 on 2024-03-20 15:24_

Not sure if all this belongs in `nodes.rs`. `ruff_python_ast::str.rs` didn't feel right, though, since `nodes.rs` depends on `str.rs` (it would have created a circular dependency between the two files.

---

_@AlexWaygood reviewed on 2024-03-20 15:25_

---

_Review comment by @AlexWaygood on `crates/ruff_python_formatter/src/string/mod.rs`:124 on 2024-03-20 15:25_

This feels like it could be quite useful generally, so maybe it should live in `ruff_python_ast` as well? Or maybe we should leave it where it is for now and only move it when we actually do feel the need for it elsewhere?

---

_Comment by @AlexWaygood on 2024-03-20 15:26_

Thanks! I added a commit to move the relevant abstractions from `ruff_python_parser` to `ruff_python_ast`, and addressed your other comments

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-03-20 15:26_

---

_@MichaReiser reviewed on 2024-03-20 16:08_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/mod.rs`:124 on 2024-03-20 16:08_

Let's move it when we have the need for it.

---

_@MichaReiser approved on 2024-03-20 16:12_

Thank you

---

_Merged by @AlexWaygood on 2024-03-20 16:16_

---

_Closed by @AlexWaygood on 2024-03-20 16:16_

---

_Branch deleted on 2024-03-20 16:16_

---
