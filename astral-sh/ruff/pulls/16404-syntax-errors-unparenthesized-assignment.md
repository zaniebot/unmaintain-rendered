```yaml
number: 16404
title: "[syntax-errors] Unparenthesized assignment expressions in sets and indexes"
type: pull_request
state: merged
author: ntBre
labels:
  - parser
  - preview
assignees: []
merged: true
base: main
head: brent/syntax-assign-sequence-310
created_at: 2025-02-26T19:20:27Z
updated_at: 2025-03-14T19:06:44Z
url: https://github.com/astral-sh/ruff/pull/16404
synced_at: 2026-01-12T15:55:54Z
```

# [syntax-errors] Unparenthesized assignment expressions in sets and indexes

---

_@ntBre_

## Summary
This PR detects unparenthesized assignment expressions used in set literals and comprehensions and in sequence indexes. The link to the release notes in https://github.com/astral-sh/ruff/issues/6591 just has this entry:
> * Assignment expressions can now be used unparenthesized within set literals and set comprehensions, as well as in sequence indexes (but not slices).

with no other information, so hopefully the test cases I came up with cover all of the changes. I also tested these out in the Python REPL and they actually worked in Python 3.9 too. I'm guessing this may be another case that was "formally made part of the language spec in Python 3.10, but usable -- and commonly used -- in Python >=3.9" as @AlexWaygood added to the body of #6591 for context managers. So we may want to change the version cutoff, but I've gone along with the release notes for now.

## Test Plan

New inline parser tests and linter CLI tests.


---

_Comment by @github-actions[bot] on 2025-02-26 19:30_

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

_Label `preview` added by @ntBre on 2025-02-28 22:39_

---

_Renamed from "[syntax-errors] Detect unparenthesized assignment expressions in sets and indexes" to "[syntax-errors] Unparenthesized assignment expressions in sets and indexes" by @ntBre on 2025-02-28 23:02_

---

_Label `parser` added by @ntBre on 2025-03-03 17:44_

---

_@ntBre reviewed on 2025-03-03 18:37_

---

_Review comment by @ntBre on `crates/ruff/tests/lint.rs`:2671 on 2025-03-03 18:37_

I left the linter test this time because it caught a case where I emitted an `UnsupportedSyntaxError` when we already had a `ParseError`.

---

_Marked ready for review by @ntBre on 2025-03-03 18:45_

---

_Review requested from @MichaReiser by @ntBre on 2025-03-03 18:45_

---

_Review requested from @dhruvmanila by @ntBre on 2025-03-03 18:45_

---

_Review comment by @MichaReiser on `crates/ruff/tests/lint.rs`:2671 on 2025-03-04 08:37_

What's the reason this can't be a parser test? Is it a limitation of our parser test framework? I'd prefer to improve the parser test framework over having a CLI test for this.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:461 on 2025-03-04 08:38_

We use `NamedExpr` for Walrus expressions internally. 
```suggestion
            UnsupportedSyntaxErrorKind::UnparenthesizedNamedExprInSubscript => "unparenthesized assignment expression",
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:461 on 2025-03-04 08:40_

Or should this allow all assignment expressions, e.g. even `a[x=10]`?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:912 on 2025-03-04 08:44_

Can we also add a test for when the named expression isn't at the `lower` position

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:878 on 2025-03-04 08:49_

I don't understand this comment. Or rather, I think I find it more confusing than helpful. Maybe we could change the comment. What I understand is that the parsed expression is a subscript if we reach 878?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@unparenthesized_walrus_set_comp_py39.py.snap`:90 on 2025-03-04 08:52_

I think the message needs to be more explicit and include sequence index or similar because it otherwise suggests that `if x:= 10:` wouldn't be allowed

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1659 on 2025-03-04 08:56_

To make it clear that this error can only be added if the error above isn't added

```suggestion
                else if key_or_element.is_unparenthesized_named_expr() {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1714 on 2025-03-04 08:58_

Nit: I'm somewhat inclined to move this check into `parse_set_expression`. That would also move the tests closer to the actual logic (for the tail elements)

---

_@MichaReiser approved on 2025-03-04 09:00_

I'd prefer to wait for @AlexWaygood  to confirm the exact details of this change in CPython (and when it was chagned). We otherwise risk flagging code that works perfectly fine on Python 3.9

---

_Review comment by @dhruvmanila on `crates/ruff/tests/lint.rs`:2684 on 2025-03-04 09:47_

If it's not already covered, it might be useful to have tests that contains named expression in other positions in the set (parenthesized and unparenthesized) because the parser will parse the first element, do some checks and then parse the remaining elements.

https://github.com/astral-sh/ruff/blob/cb362f4b0a9116867e60e3f08d185ea73c40e452/crates/ruff_python_parser/src/parser/expression.rs#L1866-L1866

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/error.rs`:461 on 2025-03-04 09:48_

Unless you meant something else and this is a typo, that's an assignment _statement_ which isn't allowed here.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/error.rs`:461 on 2025-03-04 09:49_

I think we should just rename it to `UnparenthesizedNamedExpr` as it's relevant in other context as well other than subscript (set expression, set comprehension).

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:869 on 2025-03-04 09:53_

I don't think there's anything to change here but just wanted to point out that we test some of these in the filesystem based tests:
* https://github.com/astral-sh/ruff/blob/cb362f4b0a9116867e60e3f08d185ea73c40e452/crates/ruff_python_parser/resources/valid/expressions/subscript.py
* https://github.com/astral-sh/ruff/tree/cb362f4b0a9116867e60e3f08d185ea73c40e452/crates/ruff_python_parser/resources/invalid/expressions/subscript

These are fine here as they're using different parser options.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:884 on 2025-03-04 09:55_

Can we move the relevant test cases closer to the code that's checking the condition? I'm referring to the ones above that contains single element in the subscript expression.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1714 on 2025-03-04 10:09_

I think I'd recommend moving the check to `parse_set_expression` where we have access to both the first and the rest of the set elements.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1644 on 2025-03-04 10:11_

Thanks for adding these comments. Would it be more useful to move them as rustdoc on the corresponding `UnsupportedSyntaxKind` variant instead? That way, it can include context for both set (including comprehensions) and sequence indexes.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:855 on 2025-03-04 10:13_

Can we use "named_expr" instead of "walrus"? I think it'll be useful to maintain consistency in the terminology used internally. What do you think?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:878 on 2025-03-04 10:17_

I think it might be related to the fact that `parse_slice` is used for both sequence indexes (`data[x := 1]`) and slices (`data[x := 1: 2]`) but the unsupported syntax kind is only to be raised for the former. We have the same check for the latter case down below on 893 which raises a syntax error.


---

_@dhruvmanila reviewed on 2025-03-04 10:17_

---

_@dhruvmanila reviewed on 2025-03-04 10:17_

---

_Review comment by @dhruvmanila on `crates/ruff/tests/lint.rs`:2684 on 2025-03-04 10:17_

Just noticed that they're covered, ignore this comment.

---

_Comment by @dhruvmanila on 2025-03-04 10:21_

I think it's https://github.com/python/cpython/commit/87c87b5bd6f6a5924b485398f353308410f9d8c1 that added it and it seems that the change was added in **3.9.1** so it doesn't exists in **3.9.0** (looking at the tags that commit is included in) which is why I guess it's not in the 3.9 release notes?

So, it was fixed in https://github.com/python/cpython/pull/23332 and backported to 3.9 in https://github.com/python/cpython/pull/23333.

---

_@dhruvmanila approved on 2025-03-04 10:26_

Thank you. We just need to make sure about the version cutoff but otherwise this looks good minor some nit comments.

---

_Comment by @AlexWaygood on 2025-03-04 10:30_

> I'd prefer to wait for @AlexWaygood to confirm the exact details of this change in CPython (and when it was chagned). We otherwise risk flagging code that works perfectly fine on Python 3.9

I'm on PTO this week and don't have my laptop on me. But I hear that we have other CPython core devs on the team who might be able to help with this kind of question ;)

In general, I think we should definitely follow CPython's implementation rather than CPython's documentation when it comes to the Python version that things like this became legal syntax. That seems like it should avoid false positives. In cases where the syntax was added unofficially for one or two versions prior to it being added to the language spec, it's very possible that people started using the new syntax accidentally even if it wasn't "officially" supported by the language

---

_Comment by @MichaReiser on 2025-03-04 10:31_

> I'm on PTO this week and don't have my laptop on me. But I hear that we have other core devs on the team who might be able to help with this kind of question ;)

I think this PR can wait till you're back ;) Enjoy your PTO

---

_@ntBre reviewed on 2025-03-04 13:50_

---

_Review comment by @ntBre on `crates/ruff/tests/lint.rs`:2671 on 2025-03-04 13:50_

No I think you're right. I remembered later last night that the parser tests have separate `## Error` and `## Unsupported Syntax Error` sections. I think I just didn't notice it until I wrote the linter test. I'll double check that the parser test would have caught it and then delete this.

---

_@ntBre reviewed on 2025-03-04 13:59_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/error.rs`:461 on 2025-03-04 13:59_

Yeah sorry about the names, that seems like a running theme in these reviews. I was trying to make them shorter, but it does seem better just to use explanatory names. I will also add documentation on each of the variants, I should have been doing that as I went too.

---

_@ntBre reviewed on 2025-03-04 14:32_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/expression.rs`:912 on 2025-03-04 14:32_

That was a good idea. We actually don't have a nice error message for these like when it's in the first position:

```
1 | # even after 3.10, an unparenthesized walrus is not allowed in a slice
2 | lst[x:=1:-1]
  |     ^^^^ Syntax Error: Unparenthesized named expression cannot be used here
3 | lst[1:x:=1]
4 | lst[1:3:x:=1]
  |


  |
1 | # even after 3.10, an unparenthesized walrus is not allowed in a slice
2 | lst[x:=1:-1]
3 | lst[1:x:=1]
  |        ^^ Syntax Error: Expected ']', found ':='
4 | lst[1:3:x:=1]
  |


  |
1 | # even after 3.10, an unparenthesized walrus is not allowed in a slice
2 | lst[x:=1:-1]
3 | lst[1:x:=1]
  |           ^ Syntax Error: Expected a statement
4 | lst[1:3:x:=1]
  |


  |
1 | # even after 3.10, an unparenthesized walrus is not allowed in a slice
2 | lst[x:=1:-1]
3 | lst[1:x:=1]
  |            ^ Syntax Error: Expected a statement
4 | lst[1:3:x:=1]
  |
```

Those are regular `ParseError`s though, so I might leave that for a follow-up.

---

_@ntBre reviewed on 2025-03-04 14:42_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/expression.rs`:878 on 2025-03-04 14:42_

Yes, @dhruvmanila is right, that's what I meant. I initially checked it right after `let lower = ...` and then had to move it into this `if` to avoid a duplicate with the `ParseError`, so I thought it was worth noting, but I see how it could be confusing. I think I'll just delete this comment because it's pretty clear in hindsight what's going on.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:878 on 2025-03-04 15:07_

Or we could change it to saying that we have a sequence index here and add a comment to the other branch saying that it's a slice. 

---

_@MichaReiser reviewed on 2025-03-04 15:07_

---

_@ntBre reviewed on 2025-03-04 15:43_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/expression.rs`:1644 on 2025-03-04 15:43_

Great idea. I may have gone slightly overboard on the variant docs, but I'm thinking these might be a good start if we end up wanting to show these to users eventually.

---

_@ntBre reviewed on 2025-03-04 15:56_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/expression.rs`:855 on 2025-03-04 15:56_

I think that totally makes sense. I left the existing `Walrus` variant alone for now but cleaned up all of the walruses in this PR. I'll do another cleanup pass for the already-merged variants separately.

---

_Comment by @ntBre on 2025-03-04 16:36_

Thanks for the reviews! I think I have handled all of the comments, but I'm definitely happy to wait for Alex to come back to discuss the version. 3.9 sounds like a safer way to go than 3.10, but there's no rush.

---

_Comment by @ntBre on 2025-03-14 17:37_

I handled the merge conflicts and changed the version cutoff to 3.9, so I think this should be good to go.

---

_Merged by @ntBre on 2025-03-14 19:06_

---

_Closed by @ntBre on 2025-03-14 19:06_

---

_Branch deleted on 2025-03-14 19:06_

---
