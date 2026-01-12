```yaml
number: 7927
title: Update string nodes for implicit concatenation
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/implicit-str-concat-node
created_at: 2023-10-12T12:50:32Z
updated_at: 2023-12-05T20:33:47Z
url: https://github.com/astral-sh/ruff/pull/7927
synced_at: 2026-01-12T15:55:25Z
```

# Update string nodes for implicit concatenation

---

_@dhruvmanila_

## Summary

This PR updates the string nodes (`ExprStringLiteral`, `ExprBytesLiteral`, and `ExprFString`) to account for implicit string concatenation.

### Motivation

In Python, implicit string concatenation are joined while parsing because the interpreter doesn't require the information for each part. While that's feasible for an interpreter, it falls short for a static analysis tool where having such information is more useful. Currently, various parts of the code uses the lexer to get the individual string parts.

One of the main challenge this solves is that of string formatting. Currently, the formatter relies on the lexer to get the individual string parts, and formats them including the comments accordingly. But, with PEP 701, f-string can also contain comments. Without this change, it becomes very difficult to add support for f-string formatting.

### Implementation

The initial proposal was made in this discussion: https://github.com/astral-sh/ruff/discussions/6183#discussioncomment-6591993. There were various AST designs which were explored for this task which are available in the linked internal document[^1].

The selected variant was the one where the nodes were kept as it is except that the `implicit_concatenated` field was removed and instead a new struct was added to the `Expr*` struct. This would be a private struct would contain the actual implementation of how the AST is designed for both single and implicitly concatenated strings.

This implementation is achieved through an enum with two variants: `Single` and `Concatenated` to avoid allocating a vector even for single strings. There are various public methods available on the value struct to query certain information regarding the node.

The nodes are structured in the following way:

```
ExprStringLiteral - "foo" "bar"
|- StringLiteral - "foo"
|- StringLiteral - "bar"

ExprBytesLiteral - b"foo" b"bar"
|- BytesLiteral - b"foo"
|- BytesLiteral - b"bar"

ExprFString - "foo" f"bar {x}"
|- FStringPart::Literal - "foo"
|- FStringPart::FString - f"bar {x}"
  |- StringLiteral - "bar "
  |- FormattedValue - "x"
```

[^1]: Internal document: https://www.notion.so/astral-sh/Implicit-String-Concatenation-e036345dc48943f89e416c087bf6f6d9?pvs=4

#### Visitor

The way the nodes are structured is that the entire string, including all the parts that are implicitly concatenation, is a single node containing individual nodes for the parts. The previous section has a representation of that tree for all the string nodes. This means that new visitor methods are added to visit the individual parts of string, bytes, and f-strings for `Visitor`, `PreorderVisitor`, and `Transformer`.

## Test Plan

- `cargo insta test --workspace --all-features --unreferenced reject`
- Verify that the ecosystem results are unchanged


---

_Comment by @dhruvmanila on 2023-10-12 12:50_

Current dependencies on/for this PR:
* main
  * **PR #8062** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8062?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #8063** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8063?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #8064** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8064?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
        * **PR #7927** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7927?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
          * **PR #8826** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8826?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
            * **PR #8828** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8828?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
              * **PR #8835** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8835?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
                * **PR #9016** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9016?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/7927?utm_source=stack-comment).

---

_Label `parser` added by @dhruvmanila on 2023-10-12 12:51_

---

_Label `parser` removed by @dhruvmanila on 2023-10-12 12:51_

---

_Label `internal` added by @dhruvmanila on 2023-10-12 12:51_

---

_Renamed from "POC: AST node for implicit string concatenation" to "New AST node for implicit string concatenation" by @dhruvmanila on 2023-10-19 13:48_

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/pyupgrade/rules/use_pep604_annotation.rs`:212 on 2023-10-23 08:47_

We should pick a different name than string list (`StringConcatenation` maybe?), because string list sounds like `["1", "2"]` rather than `"1" "2"`

---

_@konstin reviewed on 2023-10-23 08:48_

---

_@dhruvmanila reviewed on 2023-10-23 14:00_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pyupgrade/rules/use_pep604_annotation.rs`:212 on 2023-10-23 14:00_

Yeah, thanks! I'll propose a few names, we can finalize then.

---

_Renamed from "New AST node for implicit string concatenation" to "Update string nodes for implicit concatenation" by @dhruvmanila on 2023-11-14 13:38_

---

_Comment by @github-actions[bot] on 2023-11-18 17:24_

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

_Comment by @dhruvmanila on 2023-11-18 19:03_

The ecosystem checks are only for `F541` where earlier we _wouldn't_ highlight the individual f-string part if there's a placeholder in any other part. This means the following wouldn't be highlighted:

```python
(
    f"this is a f-string without any placeholders"
    f"while this contains a {placeholder}"
)
```

But now, we would highlight it:

```console
$ cargo run --bin ruff -- check --no-cache --isolated --select=F541 ~/playground/ruff/src/play.py --show-source --ecosystem-ci
/Users/dhruv/playground/ruff/src/play.py:2:5: F541 [*] f-string without any placeholders
  |
1 | (
2 |     f"this is a f-string without any placeholders"
  |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ F541
3 |     f"while this contains a {placeholder}"
4 | )
  |
  = help: Remove extraneous `f` prefix

ℹ Safe fix
1 1 | (
2   |-    f"this is a f-string without any placeholders"
  2 |+    "this is a f-string without any placeholders"
3 3 |     f"while this contains a {placeholder}"
4 4 | )

Found 1 error.
[*] 1 fixable with the `--fix` option.
```



---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:92 on 2023-11-20 17:30_

This is now a separate branch because there could be implicit concatenations on the left side of `%` operator.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:62 on 2023-11-20 17:30_

Escapes are required for multi-line strings to escape the newline characters.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pyupgrade/rules/native_literals.rs`:206 on 2023-11-20 17:33_

This change is mainly to narrow down the scope for `is_implicit_concatenated` method on literal expressions instead of all expressions (`Expr::is_implicit_concatenated_string`).

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/comparable.rs`:562 on 2023-11-20 17:35_

Sorry, the actual change here is the following:

```diff
-    Str(&'a str),
-    Bytes(&'a [u8]),
+    Str(Vec<ComparableStringLiteral<'a>>),
+    Bytes(Vec<ComparableBytesLiteral<'a>>),
```

And, the corresponding change in `impl<'a> From<ast::LiteralExpressionRef<'a>> for ComparableLiteral<'a> {`. I moved this part from down below which is why the diff is big.

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:1345 on 2023-11-20 17:41_

For test snapshots as the `value` field is a `OnceCell<String>`.

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/expression/string.rs`:262 on 2023-11-20 17:47_

I'm thinking of removing `FormatString` altogether and move the formatting logic in the respective node. The highlighted part is for implicit string concatenation and the `self.string.parts` is allocating a new vector because of type differences in each match blocks. This would also help in isolating docstring formatting only in `ExprStringLiteral`. Also, the f-string formatting would mean that the logic will be shifted so we might as well do it for string and bytes literal.

I'll probably do it in a follow-up PR.

---

_@dhruvmanila reviewed on 2023-11-20 17:51_

The f-strings nesting is a bit too much but it'll be reduced with https://github.com/astral-sh/ruff/pull/6365

---

_Marked ready for review by @dhruvmanila on 2023-11-20 17:52_

---

_Review requested from @BurntSushi by @dhruvmanila on 2023-11-20 17:52_

---

_Review requested from @charliermarsh by @dhruvmanila on 2023-11-20 17:52_

---

_Comment by @BurntSushi on 2023-11-21 19:14_

@dhruvmanila What would you say is the best way to review this? I first looked at doing this commit-by-commit, but the individual commits themselves are quite large. One thing that could help is if the commits were split in a way where one commit might have a change to a data structure (like a new AST node or something), and then a commit separate from that updated the rest of the code to be in sync with that change. The split makes it easier to scrutinize the "substance" of the change (the data structure) separately from the less interesting refactor bits that can easily drown out the substance. Splitting them this way will mean that your commits won't build individually, but we can squash them on merge.

I don't know how much effort it would be to split this up at this point though. It might not be worth it. It might also just be harder for me because I'm less familiar with the code.

---

_Comment by @dhruvmanila on 2023-11-21 19:26_

> One thing that could help is if the commits were split in a way where one commit might have a change to a data structure (like a new AST node or something), and then a commit separate from that updated the rest of the code to be in sync with that change.

I think it can be done, let me spend some time on it.

(Rebased on main, will be re-arranging the commits now.)

---

_Comment by @dhruvmanila on 2023-11-21 19:59_

@BurntSushi I've re-arranged the commits, let me know if it helps. I'm happy to provide any further information as required.

The commit flow is as follows:
- Update the AST nodes
- Update the parser
	- Update snapshots for parser tests
- Update visitor implementation and related code
- Update the code generator
- Update the linter
- Update the formatter

---

_Review comment by @BurntSushi on `crates/ruff_python_ast/src/nodes.rs`:1090 on 2023-11-22 12:41_

My first thought here is to wonder why, based on the comment, this isn't `Single(FString)` in order to better encode the invariant.

---

_Review comment by @BurntSushi on `crates/ruff_python_ast/src/nodes.rs`:1090 on 2023-11-22 12:44_

Hmmm yes, I see, the `FStringValue::parts` method makes it rather annoying to do that without extra type machinery that probably isn't worth doing.

---

_Review comment by @BurntSushi on `crates/ruff_python_ast/src/nodes.rs`:1326 on 2023-11-22 12:50_

I wonder if you want to implement `PartialEq` manually? Namely, if you have two `ConcatenatedStringLiteral` values where both have equivalent values for `strings` but where one has `value` initialized and the other does not, would you expect them to compare equal? Semantically I think I would, since the alternative is that equality is dependent on whether `as_str()` has been called, which seems incidental.

---

_Review comment by @BurntSushi on `crates/ruff_python_ast/src/visitor.rs`:782 on 2023-11-22 13:08_

What about using `_visitor` and `_alias` instead of the `allow(unused_variables)`? (Not a big deal either way.)

---

_@BurntSushi approved on 2023-11-22 13:15_

Ah this is much better! Thank you for breaking it up. It made it oodles easier to review. :-)

Overall great work. I especially found the new fstring/string/byte-string types pretty easy to follow and understand. Their structure makes sense to me.

---

_@dhruvmanila reviewed on 2023-11-22 15:38_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:1090 on 2023-11-22 15:38_

Yeah, I tried adding a `FStringPartRef` enum but then we can't get the mutable reference for it which is required in the `Transformer`.

```rs
enum FStringPartRef<'a> {
	Literal(&'a StringLiteral),
	FString(&'a FString),
}
```

---

_@dhruvmanila reviewed on 2023-11-22 15:43_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:1326 on 2023-11-22 15:43_

Oh, that's a good point, thanks for noticing it. I'll add the custom implementation for `PartialEq` which compares the `strings` field only:

```rs
impl PartialEq for ConcatenatedStringLiteral {
    fn eq(&self, other: &Self) -> bool {
        if self.strings.len() != other.strings.len() {
            return false;
        }
        // The `zip` here is safe because we have checked the length of both parts.
        self.strings
            .iter()
            .zip(other.strings.iter())
            .all(|(s1, s2)| s1 == s2)
    }
}
```

---

_@dhruvmanila reviewed on 2023-11-22 15:44_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/visitor.rs`:782 on 2023-11-22 15:44_

I did it mainly for consistency with other methods 😅 

I'll probably change it in a follow-up PR instead.

---

_@charliermarsh reviewed on 2023-11-22 17:36_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/comparable.rs`:562 on 2023-11-22 17:36_

Should this be a `Vec` though? Does this still consider `"foobar"` and `"foo" "bar"` to be equivalent?

---

_@charliermarsh reviewed on 2023-11-22 17:44_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/bad_string_format_type.rs`:128 on 2023-11-22 17:44_

I suspect that you could implement `.chars()` without needing to allocate the string.

---

_@charliermarsh reviewed on 2023-11-22 17:45_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:151 on 2023-11-22 17:45_

Interesting, how did this get so much simpler?

---

_@charliermarsh reviewed on 2023-11-22 17:51_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/nodes.rs`:1241 on 2023-11-22 17:51_

I don't know that I'm a huge fan of `deref` here since this can actually be expensive and require an allocation. How much trouble would it be to remove this and require explicit `as_str()`?

---

_@charliermarsh approved on 2023-11-22 17:54_

This is impressive work. There's a huge surface area here. You did a great job of breaking down the individual PRs and walking us through it.

I left a few questions, which I'm happy to continued discussing, but erring on the side of approving.

---

_@charliermarsh reviewed on 2023-11-22 17:55_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/nodes.rs`:1116 on 2023-11-22 17:55_

I wonder if these should be `smallvec`... It's probably worth benchmarking in a separate PR. In practice, I'd bet the vast majority of concatenations are <= 5 elements.

---

_@charliermarsh reviewed on 2023-11-22 17:57_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/unrecognized_platform.rs`:130 on 2023-11-22 17:57_

This is an example where... we can probably do this without the concatenation? Imagine if we had a custom matcher that worked on implicit concatenations (i.e., a vector of strings). \cc @burntsushi in case you have obvious ideas here.

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/flake8_pyi/rules/unrecognized_platform.rs`:130 on 2023-11-22 18:28_

I think the closest "out of the box" experience you can get is [stream searching with `aho-corasick`](https://docs.rs/aho-corasick/latest/aho_corasick/struct.AhoCorasick.html#method.stream_find_iter), but you need to provide a `std::io::Read` impl. Mildly annoying but doable in this context.

But I would say to write the simple/obvious code for now, and if this ends up popping up on a profile then we can revisit it and do something bespoke here or use `aho-corasick`.

---

_@BurntSushi reviewed on 2023-11-22 18:28_

---

_@dhruvmanila reviewed on 2023-11-22 19:00_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:1241 on 2023-11-22 19:00_

It's not too much as I just need to go through the references and add the method call which should be simple with `rust-analyzer`.

I get your point and I think having an explicit call is worth the small effort.

---

_@dhruvmanila reviewed on 2023-11-22 19:04_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/bad_string_format_type.rs`:128 on 2023-11-22 19:04_

Yes, we can.

Thinking about this with your previous comment, I think we should implement some other methods as well like `is_empty`, `len`, etc. which means that these APIs will guaranteed be free of allocation and, we would be providing `as_str` to get the concatenated source code to use methods not implemented on the abstract type.

---

_@dhruvmanila reviewed on 2023-11-22 19:10_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:151 on 2023-11-22 19:10_

Ah, that's the magic of this refactor 😉 

Jokes aside, this basically reverts this commit https://github.com/astral-sh/ruff/commit/ac4a4da50ed524b45f162acf2726aea902b2026f which fixed the bug where this rule wasn't producing the correct fix for an implicitly concatenated string. But, now that we work on individual parts, that isn't required.

---

_@dhruvmanila reviewed on 2023-11-22 19:15_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_pyi/rules/unrecognized_platform.rs`:130 on 2023-11-22 19:15_

I think I would prefer to avoid the complexity for such simple cases but I do understand that this could be useful in similar cases where the strings are long.

---

_@dhruvmanila reviewed on 2023-11-23 14:57_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:1241 on 2023-11-23 14:57_

#8826 

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/bad_string_format_type.rs`:128 on 2023-11-23 14:57_

#8826 

---

_@dhruvmanila reviewed on 2023-11-23 14:57_

---

_Merged by @dhruvmanila on 2023-11-24 23:55_

---

_Closed by @dhruvmanila on 2023-11-24 23:55_

---

_Branch deleted on 2023-11-24 23:55_

---
