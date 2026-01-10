```yaml
number: 13158
title: "Add a method to `Checker` for cached parsing of stringified type annotations"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - performance
assignees: []
merged: true
base: main
head: alex/cached-annotation-parsing
created_at: 2024-08-30T10:20:49Z
updated_at: 2024-09-05T15:26:31Z
url: https://github.com/astral-sh/ruff/pull/13158
synced_at: 2026-01-10T21:38:32Z
```

# Add a method to `Checker` for cached parsing of stringified type annotations

---

_Pull request opened by @AlexWaygood on 2024-08-30 10:20_

## Summary

This PR adds a method to `ruff_linter::checkers::ast::Checker` for cached parsing of stringified type annotations. It was decided in review of #12951 that this would be desirable, since `ruff_python_parser::typing::parse_type_annotation` can be quite expensive. However, since we already have a number of linter rules that call `ruff_python_parser::typing::parse_type_annotation`, it seemed to me like this would make sense as a standalone PR. (Adding caching was also more complicated than I expected, so separating this into its own PR should make life easier for reviewers.) If this is accepted, I'll rebase #12951 on top of it.

The PR should be easiest to review commit-by-commit. Each commit on its own passes the entire test suite. The first two commits do some refactoring to lay the groundwork for adding the new method. The third commit adds the new method and makes use of it in `crates/ruff_linter/src/checkers/ast/mod.rs`; the final commit makes use of the new method in various linter rules that currently use `ruff_python_parser::typing::parse_type_annotation`.

## Test Plan

`cargo test`


---

_Label `performance` added by @AlexWaygood on 2024-08-30 10:20_

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-08-30 10:20_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-30 10:20_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-08-30 10:20_

---

_Comment by @AlexWaygood on 2024-08-30 10:31_

This doesn't seem to have any impact on the Codspeed benchmarks one way or another as a standalone PR (but I don't know how many stringified annotations we have in those benchmarks?)

---

_Comment by @github-actions[bot] on 2024-08-30 10:39_

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

_@charliermarsh reviewed on 2024-08-30 16:42_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/mod.rs`:267 on 2024-08-30 16:42_

Why is the arena necessary here? Doesn't the cache store owned data?

---

_@AlexWaygood reviewed on 2024-09-01 00:18_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/mod.rs`:267 on 2024-09-01 00:18_

I just pushed https://github.com/astral-sh/ruff/pull/13158/commits/8ff61ad8f16aa9b88f45f653b170acd711769aac to clarify the design of the cache a little bit.

The `HashMap` in the cache has to be behind a `RefCell` or we can't call `Checker::parse_type_annotation` with an `&self` reference, and having it take an `&mut self` reference causes all sorts of borrow checker complaints. If the `HashMap` behind the `RefCell` owns the data, however, it's impossible to return a reference to the data from the cache's `lookup_or_parse()` method, or the borrow checker complains that the data returned by `lookup_or_parse()` borrows data owned by the current function. So the parsed annotations have to be stored in some kind of external storage outside of the `RefCell<HashMap>`, with the `RefCell<HashMap>` only storing references to the data.

If there's a better way of doing this, I'd love to know about it. But I've been puzzling over this for much of the afternoon, and can't really see it :(

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-09-01 10:21_

---

_Comment by @MichaReiser on 2024-09-02 10:23_

> This doesn't seem to have any impact on the Codspeed benchmarks one way or another as a standalone PR (but I don't know how many stringified annotations we have in those benchmarks?)

I don't think we have any benchmark containing stringified annotations. You could try to run some hyperfine benchmarks over a project that does use stringified annotations.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:182 on 2024-09-02 10:29_

Nit: I'm leaning towards caching only successfully parsed annotations, considering that annotations with syntax errors should be rare. This would help reduce the size of every element in the arena.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:183 on 2024-09-02 10:29_

Nit: Maybe `by_offset`? 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:2265 on 2024-09-02 10:32_

Can you tell me more about what is inside the cache that is no longer valid? I don't see that this loop modifies the AST in anyway.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:181 on 2024-09-02 10:35_

Nit: I would move this type to the end of the file. It seems less important than `Checker` (which is the main thing in this file)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:267 on 2024-09-02 10:41_

I don't think there's a better way. 

As you said, the value returned by `lookup_or_parse` must be copy to avoid borrowing a value that's behind a `RefCell`. 

This is accomplished by storing references that outlive the `Cache` (and `Checker`))

---

_@MichaReiser approved on 2024-09-02 10:42_

Nice. Looks good to me. 

The only part that's unclear to me is why we need to reset the cache. It would be nice if it could be avoided. If not, then it would help to extend the comment so that it explains what data gets invalidated.

---

_Comment by @Daverball on 2024-09-02 11:05_

This is more of a general comment, since I thought about adding something similar for my ongoing work on flake8-type-checking.

I am not sure to what degree this would be viable, but why not just eagerly parse string annotations, store them all on the checker as a struct that contains both the string and the parsed expression add that struct to the deferred string annotations vector instead of just the string and then visit the expression where we currently parse the annotation in the checker. That should preserve the same semantics for bindings and references.

I realize there are some corner cases with nested strings like `'list["str"]'` where parsing either would still need to be deferred or the nested case would need to be handled eagerly while parsing the annotation, adding an arbitrary number of string / expression pairs, rather than only a single one. But it still seems viable to me, unless I'm forgetting an important detail.

---

_Comment by @MichaReiser on 2024-09-02 11:15_

I thought about that too because we always end up parsing all type annotations. 

My conclusion was that doing it inside of `Checker` would require one additional AST pass, which is unfortunate. It would be nice if we could do this directly in the parser but that's probably more involved.

---

_@AlexWaygood reviewed on 2024-09-02 11:21_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/mod.rs`:2265 on 2024-09-02 11:21_

Sure. Consider the following annotation:

```py
x: "list['str']"
```

The first time we visit the AST, we see `"list['str']"` and identify it as a stringified annotation. We store it in `self.visit.string_type_definitions` to be analyzed later.

After the entire tree has been visited, we look through `self.visit.string_type_definitions` and find `"list['str']"`. We parse it, and it becomes `list['str']`. After parsing it, we call `self.visit_expr()` on the `list['str']` node, and that `visit_expr` call is going to find `'str'` inside that node and identify it as a string type definition, appending it to `self.visit.string_type_definitions`, ensuring that there will be one more iteration of this loop. Unfortunately, the `TextRange` of `'str'` here will be _relative to the parsed `list['str']` node_ rather than _relative to the original module_, meaning the cache (which uses `TextSize` as the key) becomes invalid on the second iteration of this loop.

Does that make sense? Can you see a better way round this?

---

_@AlexWaygood reviewed on 2024-09-02 11:23_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/mod.rs`:2265 on 2024-09-02 11:23_

(The effect of this is that without clearing the cache on each iteration of this loop, we can have an infinite loop on some snippets. I observed this when running `cargo test -p ruff_linter` on an early version of this PR, and it took a while to debug 😆)

---

_@MichaReiser reviewed on 2024-09-02 11:26_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:2265 on 2024-09-02 11:26_

Makes sense. It might make sense to paste this excellent explanation into the comment :)

No, I don't have another workaround other than using ptr-comparison and storing expression references similar to https://github.com/astral-sh/ruff/blob/d6851076388624d4c76c68d0cf8e72fd29e4a6e6/crates/ruff_python_formatter/src/comments/node_key.rs#L9-L12

---

_@AlexWaygood reviewed on 2024-09-02 11:30_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/mod.rs`:2265 on 2024-09-02 11:30_

> Makes sense. It might make sense to paste this excellent explanation into the comment :)

I'll do so!

---

_Merged by @AlexWaygood on 2024-09-02 12:44_

---

_Closed by @AlexWaygood on 2024-09-02 12:44_

---

_Branch deleted on 2024-09-02 12:44_

---

_Label `internal` added by @dhruvmanila on 2024-09-05 15:26_

---
