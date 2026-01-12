```yaml
number: 16482
title: "[syntax-errors] Parenthesized keyword argument names after Python 3.8"
type: pull_request
state: merged
author: ntBre
labels:
  - parser
  - preview
assignees: []
merged: true
base: main
head: brent/syn-paren-kwarg
created_at: 2025-03-03T20:28:41Z
updated_at: 2025-03-06T17:18:15Z
url: https://github.com/astral-sh/ruff/pull/16482
synced_at: 2026-01-12T15:55:55Z
```

# [syntax-errors] Parenthesized keyword argument names after Python 3.8

---

_@ntBre_

Summary
--

Unlike the other syntax errors detected so far, parenthesized keyword arguments are only allowed *before* 3.8. It sounds like they were only accidentally allowed before that [^1].

As an aside, you get a pretty confusing error from Python for this, so it's nice that we can catch it:

```pycon
>>> def f(**kwargs): ...
... f((a)=1)
...
  File "<python-input-0>", line 2
    f((a)=1)
       ^^^
SyntaxError: expression cannot contain assignment, perhaps you meant "=="?
>>>
```
Test Plan
--
Inline tests.

[^1]: https://github.com/python/cpython/issues/78822


---

_Label `parser` added by @ntBre on 2025-03-03 20:28_

---

_Label `preview` added by @ntBre on 2025-03-03 20:28_

---

_Review requested from @MichaReiser by @ntBre on 2025-03-03 20:28_

---

_Review requested from @dhruvmanila by @ntBre on 2025-03-03 20:28_

---

_Comment by @github-actions[bot] on 2025-03-03 20:37_

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

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:452 on 2025-03-04 08:18_

```suggestion
    ParenthesizedKwargName,
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:485 on 2025-03-04 08:22_

This feels hacky and confusing. 

What you could do here is e.g change this to `changed_in` which returns a `ChangedIn` struct that has a `version` and `change` field where `change` is `added` or `removed`. 

I also suggest limiting the visibility of this method to `pub(crate)`. 

Another alternative is to remove the method and go for a bit more code repetition by:

* Using an explicit message for each kind
* Undo the changes that I proposed to inline the version check into `add_unsupported_syntax` and do the check at the call site. 

I'm sort of leaning for the second because it feels simpler. 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:710 on 2025-03-04 08:27_

Do we need to store the `parenthesized_range` (the downside of storing the range is that it probably made `ParsedExpr` larger?)? Isn't the range the same as using `start` and `


Instead, I think you can get the location by calling `self.node_range(start)`, but I'm not a 100% sure about that.

---

_@MichaReiser reviewed on 2025-03-04 08:28_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/error.rs`:452 on 2025-03-04 11:09_

I think we should use "Keyword" instead of "Kwarg" as the latter is a convention used to describe variadic keyword argument (`**kwarg`).

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/error.rs`:485 on 2025-03-04 11:13_

Yeah, I agree with that. Using an explicit message seems simpler and would also allow us to tailor each message. We do that for other parse errors as well https://github.com/astral-sh/ruff/blob/cb362f4b0a9116867e60e3f08d185ea73c40e452/crates/ruff_python_parser/src/error.rs#L210-L233

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:719 on 2025-03-04 11:16_

Can we add some test cases with random whitespaces like `f((  a ) = 1)` mainly to verify the diagnostic range?

---

_@dhruvmanila reviewed on 2025-03-04 11:17_

---

_@ntBre reviewed on 2025-03-04 20:10_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/error.rs`:452 on 2025-03-04 20:10_

I went for `ParenthesizedKeywordArgumentName`.

---

_@ntBre reviewed on 2025-03-04 20:42_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/error.rs`:485 on 2025-03-04 20:42_

I agree that this is hacky and confusing and also that it's looking more and more like we should just use custom messages for each variant as they grow more different. However, I think the `add_unsupported_syntax` change is really convenient. It feels a lot nicer to keep the version in one place, next to the variants themselves, and not have to include `PythonVersion`s throughout the parser. I'm going to try using something like the `ChangedIn` approach for now, but I definitely won't mind moving all the way to the simple approach if it still looks awkward.

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/error.rs`:485 on 2025-03-04 21:16_

I think it does still look awkward, but I'll let y'all have a look. If it doesn't look salvageable I'm happy to switch to the simple approach.

I'll admit I'm also starting to miss my macro because I think this would look pretty neat as

```rust
syntax_errors! {
    (Match, Added, PY310, "Cannot use `match` statement"),
    (Walrus, Added, PY38, "Cannot use named assignment expression (`:=`)"),
    (ExceptStar, Added, PY311, "Cannot use `except*`"),
    (ParenthesizedKeywordArgumentName, Removed, PY38, "Cannot use parenthesized keyword argument name"),
}
```

but I know this has the downsides we talked about before.

---

_@ntBre reviewed on 2025-03-04 21:16_

---

_@ntBre reviewed on 2025-03-04 21:42_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/expression.rs`:710 on 2025-03-04 21:42_

`self.node_range(start)` in the `parser.add_unsupported_syntax_error` call includes the `=`. I tried grabbing a second range before eating the `Token::Equal` and constructing it from there, but that includes a trailing space in a case like this:

```python
f((a) = 1)
  ~~~^   # tildes are expected, ^ is the extra space
```

that might be preferable to making `ParsedExpr` larger, though. As an intermediate step, I've just included the end of the range in `ParsedExpr` to make it half as much larger. At least according to my LSP, the `Option<TextSize>` and the `bool` make `ParsedExpr` the same size (72 vs 80 with `TextRange`).

I might be missing something here, though. It looks like part of your comment may have gotten cut off.
                                

---

_@MichaReiser reviewed on 2025-03-05 07:32_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:524 on 2025-03-05 07:32_

Wouldn't it be possible to implement this based on `changed_version`?

```rust
let (change, version) = self.changed_version();

match change {
	Change::Added => target_version < version,
	Change::Removed => target_version >= version
}
```

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/error.rs`:484 on 2025-03-05 10:42_

We could possibly include the `PythonVersion` where this change was added or removed:
```rs
enum Change {
    Added(PythonVersion),  // added in Python ...
    Removed(PythonVersion),  // removed in Python ...
}
```

---

_@dhruvmanila reviewed on 2025-03-05 10:42_

---

_@ntBre reviewed on 2025-03-05 13:20_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/error.rs`:524 on 2025-03-05 13:20_

Oh yes, thanks!

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/error.rs`:484 on 2025-03-05 13:27_

Nice, that is a little neater.

---

_@ntBre reviewed on 2025-03-05 13:27_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:569 on 2025-03-06 07:31_

```suggestion
```

Future readers don't have the context on in which order you added those, so I'm not sure this paragraph is useful to them. 

We could document `Added` and `Removed` but I think it's mostly self explanatory when looking at the `Display` implementation

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:710 on 2025-03-06 07:38_

This seems to work but I'm not sure if it's okay to access `prev_token_end` directly. 

@dhruvmanila do you have a preference here?

```patch
Subject: [PATCH] Change `iterate` to return a `Result`
---
Index: crates/ruff_python_parser/src/parser/expression.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_python_parser/src/parser/expression.rs b/crates/ruff_python_parser/src/parser/expression.rs
--- a/crates/ruff_python_parser/src/parser/expression.rs	(revision c73a6a15f7b161d1bfa6ad512cad70b49265a4b2)
+++ b/crates/ruff_python_parser/src/parser/expression.rs	(date 1741246585551)
@@ -672,6 +672,8 @@
                 seen_keyword_unpacking = true;
             } else {
                 let start = parser.node_start();
+
+                let paren_start = parser.current_token_range().start();
                 let mut parsed_expr = parser
                     .parse_named_expression_or_higher(ExpressionContext::starred_conditional());
 
@@ -701,6 +703,7 @@
                     }
                 }
 
+                let parens_end = parser.prev_token_end;
+                // or
+                let parens_end = parser.node_range(paren_start).end();
                 if parser.eat(TokenKind::Equal) {
                     seen_keyword_argument = true;
                     let arg = if let ParsedExpr {
@@ -721,7 +724,7 @@
                         if let Some(end) = end_parenthesis {
                             parser.add_unsupported_syntax_error(
                                 UnsupportedSyntaxErrorKind::ParenthesizedKeywordArgumentName,
-                                TextRange::new(start, end),
+                                TextRange::new(paren_start, parens_end),
                             );
                         }
 

```

(The variable naming could be better)

---

_@MichaReiser approved on 2025-03-06 07:40_

I still slightly prefer to not store the parenthesized range to make this PR smaller and avoid storing information that can be retrieved from the parser already but I'm happy to go with what you / @dhruvmanila think is best

---

_@dhruvmanila reviewed on 2025-03-06 11:31_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:710 on 2025-03-06 11:31_

We can just use `start` which is defined above `paren_start` in your diff as both are equivalent.

I played around with using `prev_token_end` and couldn't really find any issues with it. I'd say it's fine to use `prev_token_end` or `node_range`.  

I think I'd just use something like `let arg_range = self.node_range(start)` before consuming the `=` token and then check if the parsed expression is parenthesized or not and use the `arg_range` for the error range.

---

_@dhruvmanila approved on 2025-03-06 11:33_

Nice.

As suggested https://github.com/astral-sh/ruff/pull/16482#discussion_r1983191200, I'd get the argument range locally using `node_range` and use that instead.

---

_Review comment by @T-256 on `crates/ruff_python_parser/resources/inline/err/parenthesized_kwarg_py38.py`:1 on 2025-03-06 13:11_

```suggestion
# parse_options: {"target-version": "3.9"}
```
It makes bellow tests better, and prevents show duplicated Python version on diagnostics message:
`Syntax Error: Cannot use parenthesized keyword argument name on Python 3.8 (syntax was removed in Python 3.8)`

When I looked at it, I thought we could de-duplicate message to make it simple, but then I realized first `on Python 3.8` is actually for selected `target-version` Python.

---

_@T-256 reviewed on 2025-03-06 13:11_

---

_@ntBre reviewed on 2025-03-06 13:19_

---

_Review comment by @ntBre on `crates/ruff_python_parser/resources/inline/err/parenthesized_kwarg_py38.py`:1 on 2025-03-06 13:19_

I think it's important to test that we get the error on the earliest version for which we expect the error, which is 3.8.

I agree the error looks redundant at first glance, but both 3.8s are conveying different information, like I think you said. The first one is the target version and the second is the time the error was added or removed. You need both if you want to avoid the error by updating your target Python version.

---

_@ntBre reviewed on 2025-03-06 13:22_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/expression.rs`:710 on 2025-03-06 13:22_

I'm not sure what I was messing up before to get the extra space before `=` :facepalm:

`let arg_range = parser.node_range(start)` right before the `=` works perfectly. Thanks for the suggestions!

---

_Renamed from "[syntax-errors] Parenthesized keyword arguments after Python 3.8" to "[syntax-errors] Parenthesized keyword argument names after Python 3.8" by @ntBre on 2025-03-06 17:17_

---

_Merged by @ntBre on 2025-03-06 17:18_

---

_Closed by @ntBre on 2025-03-06 17:18_

---

_Branch deleted on 2025-03-06 17:18_

---
