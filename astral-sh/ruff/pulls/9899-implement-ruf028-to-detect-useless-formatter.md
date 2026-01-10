```yaml
number: 9899
title: Implement RUF028 to detect useless formatter suppression comments
type: pull_request
state: merged
author: snowsignal
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: jane/formatter/useless-noqa
created_at: 2024-02-08T19:25:00Z
updated_at: 2024-02-28T19:33:14Z
url: https://github.com/astral-sh/ruff/pull/9899
synced_at: 2026-01-10T22:57:09Z
```

# Implement RUF028 to detect useless formatter suppression comments

---

_Pull request opened by @snowsignal on 2024-02-08 19:25_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->
Fixes #6611

## Summary

This lint rule spots comments that are _intended_ to suppress or enable the formatter, but will be ignored by the Ruff formatter. 

We borrow some functions the formatter uses for determining comment placement / putting them in context within an AST.

The analysis function uses an AST visitor to visit each comment and attach it to the AST. It then uses that context to check:
1. Is this comment in an expression?
2. Does this comment have bad placement? (e.g. a `# fmt: skip` above a function instead of at the end of a line)
3. Is this comment redundant?
4. Does this comment actually suppress any code?
5. Does this comment have ambiguous placement? (e.g. a `# fmt: off` above an `else:` block)

If any of these are true, a violation is thrown. The reported reason depends on the order of the above check-list: in other words, a `# fmt: skip` comment on its own line within a list expression will be reported as being in an expression, since that reason takes priority.

The lint suggests removing the comment as an unsafe fix, regardless of the reason.

## Test Plan

A snapshot test has been created.

---

_Review requested from @MichaReiser by @snowsignal on 2024-02-08 19:25_

---

_Comment by @snowsignal on 2024-02-08 19:28_

### Draft Checklist
- [x] Snapshot test output isn't totally correct yet. We need to correctly resolving dangling comments at the end of indented bodies.
- [x] A few more edge cases need to be added to ensure that all permutations are accounted for.
- [x] We need to check if there's any other scenarios to report, and whether those need new error messages.

---

_Comment by @codspeed-hq[bot] on 2024-02-14 20:22_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/jane/formatter/useless-noqa)

### Merging #9899 will **not alter performance**

<sub>Comparing <code>jane/formatter/useless-noqa</code> (8fc186e) with <code>main</code> (36bc725)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Marked ready for review by @snowsignal on 2024-02-14 22:53_

---

_@MichaReiser reviewed on 2024-02-15 13:01_

Uhh, it seems there are many failing tests. I'm not sure what happened. It doesn't seem like you changed much on the formatter side.

---

_Label `rule` added by @MichaReiser on 2024-02-15 13:09_

---

_Label `preview` added by @MichaReiser on 2024-02-15 13:09_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/module.rs`:12 on 2024-02-15 13:12_

I would avoid calling it `noqa` because the formatter suppression comments are not `noqa` comments (they're `fmt: off`, `fmt: skip`, `yapf: disable` etc but not `noqa: RUF001`). 

I would also prefer the word invalid over ignored. Maybe `InvalidFormatterSuppressionComment`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/noqa.rs`:203 on 2024-02-15 13:13_

Nit: Maybe move out of the `noqa` module?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/lib.rs`:119 on 2024-02-15 14:15_

Nit: I would move this into `comments/mod` 

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/suppression.rs`:17 on 2024-02-15 14:17_

Nit: I would call this `from_str` because I wouldn't think of a `str` as a slice (yes, it is a byte slice, but that's somewhat hidden away). 

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/suppression.rs`:16 on 2024-02-15 14:17_

The documentation looks incomplete. I don't know if that was already the case before.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/mod.rs`:179 on 2024-02-15 14:19_

Nit: Maybe just `text`?

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/suppression.rs`:92 on 2024-02-15 14:21_

`CommentLinePosition` is conceptually independent of suppression comments (it's used for all sort of comments in the formatter). Maybe rename the module to `comments` instead?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/lib.rs`:123 on 2024-02-15 14:23_

Nit: 
```suggestion
                SuppressionKind::from_slice(comment.slice_source(source))),
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/visitor.rs`:96 on 2024-02-15 14:28_

Nit: The name `text_position` stands in contrast with `line_position` and raises the question how  `line_position` is different or the same as `text_position` (The old function name is an old relict before a rename). Maybe name the method `from_text` or `from_str`?
```suggestion
                line_position: CommentLinePosition::from_str(
                    *comment_range,
                    self.source_code.as_str(),
                ),
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/ignored_formatter_noqa.rs`:50 on 2024-02-15 15:21_

Reading the rule documentation helped me understand why `InvalidSuppressionComment` isn't a good name because it also recognise comments that are sementically valid but are useless. We could split the rule into multiple rules as I remember you suggested when starting to work on the rule:

* `UslessFormatterSuppressionComment`: Warns about sementically correct suppression comments that are useless: Disabling formatting at the end of a block, enabling formatting when not inside a suppression, disabling formatting in an already suppressed range. 
* `InvalidFormatterSuppressionComment`: Errors for comments that are sementically invalid: Suppression comments in expression positions, skip comments that aren't at the end of the line etc.
* `UnclosedFormatterSuppressionComment`: Warns about `fmt: off` comments without a matching `fmt: on` comment

The benefit of splitting the rules is that `InvalidFormatterSuppressionComment` warns about a correctness issue where the other two are more opinionated (it's suspicious but not necessarily incorrect). Users may also want to use different severity levels for the rules.

 I think you proposed this once. Maybe `UselessFormatterSuppressionComment`? 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/ignored_formatter_noqa.rs`:87 on 2024-02-15 15:23_

Would it be worth to filter `comment_ranges` to ranges that are suppression comments and exit early if there are no suppression comments in the document? We could even consider collecting the ranges with their suppression kind into a vec (or SmallVec) because most files don't contain suppression comments and it's most important to exit early if a file contains no suppression comments.

Edit: I can see now that the visitor does it to some extend. I suspect that the visitor's handling is still more expensive because it already allocates a bunch of data structures ahead of time AND visits every statement at the root level, but skips traversing into them. 

Moving the logic out might also help with the `filter_map` in the `SuppressionCommentVisitor` because the visitor can then just become generic over the `Iterator`. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/suppression_comment_visitor.rs`:44 on 2024-02-15 15:33_

Nit: I think I would prefer defining our own `Iterator` here over using a `Box ed function

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/suppression_comment_visitor.rs`:42 on 2024-02-15 15:42_

Nit: The reason why the `CommentsVisitor` in the formatter accepts a `builder` is because we want to display the comments in the playground and that requires us having a way to collect them. I wonder if it would be easier if the logic would be merged, or at least the `check_comment` functionality

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/suppression_comment_visitor.rs`:162 on 2024-02-15 15:48_

Comparing nodes performs a deep comparision which is fairly expensive. You want to use `AnyNodeRef::ptr_eq()` instead.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/suppression_comment_visitor.rs`:21 on 2024-02-15 15:49_

I think it would be helpful to explain the visitor in more detail, especially because it captures additional state (`comments_in_scope`) that isn't explained and I struggle understanding what it is used for.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/suppression_comment_visitor.rs`:195 on 2024-02-15 15:57_

Delete the `allow(dead_code)`

```suggestion
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/suppression_comment_visitor.rs`:195 on 2024-02-15 15:57_


```suggestion
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/suppression_comment_visitor.rs`:198 on 2024-02-15 15:59_

It might be worthwile to explain this struct and it's equality constraints 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/suppression_comment_visitor.rs`:135 on 2024-02-15 15:59_

Could you add a comment explaining why we break for comments that are not own line?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/suppression_comment_visitor.rs`:202 on 2024-02-15 16:18_

Nit: `previous_state` is a very generic name, not telling me much about what it is without reading the code. 

What I understand from the code is that this is the suppression kind of the previous suppression comment that has a shared enclosing node? Can we refine the name and maybe document the field to explain what it is?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/ignored_formatter_noqa.rs`:90 on 2024-02-15 16:27_

It seems the main reason to use the `BTreeMap` here is so that we can emit the Diagnostics in order. I don't think we're making use of the key's uniquness to deduplicate data, because we should capture each comment only once. 

That's why I would prefer using a simple `Vec` and do a manual sorting before emitting the diagnostic. It makes the sorting more explicit (and removes the need for an `Eq`, `Ord` of `SuppressionCommentData`) and has the advantage that a `Vec` is cheaper to construct or resize.

Related. It seems that we only use the `TextRange` from `SuppressionCommentData` and no other fields. So it should be possible to change this to `Vec<(TextRange, IgnoredReason)>` which avoids writting a lot of unnecessary data. It also shows that `parent` is actually unused on `SuppressionCommentData`, maybe there's more code that can be deleted

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/ignored_formatter_noqa.rs`:128 on 2024-02-15 16:30_

Would this return a false negative if you have

```python
if True:
    # fmt: off
    test # fmt: skip
    # fmt: off
```

or 

```python
if True:
    # fmt: off
    test 
    # fmt: skip
    # fmt: off
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/ignored_formatter_noqa.rs`:99 on 2024-02-15 16:31_

`comments_in_scope` doesn't seem to be used

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/ignored_formatter_noqa.rs`:1 on 2024-02-15 16:41_

Can we delete this?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/suppression_comment_visitor.rs`:52 on 2024-02-15 16:56_

Using `comment_ranges.len()` will almost always allocate a too large `Vec` because most of the comments in a file are not suppression comments. Keeping it as is makes sense if we only pass the suppression comments to `new` instead of all comments.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/suppression_comment_visitor.rs`:30 on 2024-02-15 17:00_

It seems we always push `Some(node)`, so using `Option<AnyNodeRef>` here is unnecessary.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/ignored_formatter_noqa.rs`:58 on 2024-02-15 17:09_


```suggestion
            "This suppression comment will be ignored by the formatter because {}",
```

---

_@MichaReiser requested changes on 2024-02-15 17:48_

Nice work. You make it look less complicated than it is. There are some perf opportunities and simplifications that we can do to the code. But there are also still a few cases that are not handled correctly which make it difficult to assess if other cases are handled correctly. We should have a look at those. 

I played around with it in the playground and found a few cases that need improvement:

```python
def body():
    # fmt: off
    test
    # fmt: on

    test2
```

The rule reports that the last `fmt: on` comment is unnecessary because formatting is already enabled. This is not the case, formatting was disabled

```python
The rule reports that the `fmt: off` in the `if True` body is unused but that's a valid use. 
```

One check the rule doesn't cover today but I'm okay leaving to an extension is missing `fmt: on` comments to re-enable formatting without relying on the automatic re-enabling at the end of a block. 


---

_Review comment by @snowsignal on `crates/ruff_linter/src/rules/ruff/rules/suppression_comment_visitor.rs`:44 on 2024-02-17 05:43_

I feel like this gets complicated because of the closure that `FilterMap` needs, though. There's no easy way to mask the closure type without boxing.

---

_Review comment by @snowsignal on `crates/ruff_linter/src/rules/ruff/rules/suppression_comment_visitor.rs`:42 on 2024-02-17 05:48_

I personally like this pattern because it decouples the context generation from analysis. It makes things easier to understand IMO.

---

_Review comment by @snowsignal on `crates/ruff_linter/src/rules/ruff/rules/ignored_formatter_noqa.rs`:50 on 2024-02-17 05:58_

I think one of the reasons I wanted these scenarios under one rule is because invalid comments actually affect how we handle comment state. Like, for example, if a comment is between a decorator node and a function definition, we shouldn't 'count' that as a valid comment. Here's an example of what I mean:
```python
@decorator
# fmt: off
def func():
    # fmt: on
    pass
```
In this case, I think we can be smart and warn about _both_ comments on the first pass, since the first comment will be ignored, meaning the second comment is useless here.

However I understand that users might want to disable/enable some subset of errors. I think one way we could approach this would be to visit the AST once, collect all the errors, and then have each rule filter by the errors associated with it.

---

_@snowsignal reviewed on 2024-02-17 08:52_

---

_@MichaReiser reviewed on 2024-02-19 08:02_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/ignored_formatter_noqa.rs`:50 on 2024-02-19 08:02_

That makes sense. Yeah, we have other rules where we have a single "implementation" and then branch on the error case to decide which diagnostic to create. That way we avoid code-duplication (and runtime overhead) as well as duplicated warnings.

---

_@snowsignal reviewed on 2024-02-22 20:27_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/rules/ruff/rules/ignored_formatter_noqa.rs`:128 on 2024-02-22 20:27_

The first case seems to be a false negative, at least.

---

_Review requested from @MichaReiser by @snowsignal on 2024-02-23 19:24_

---

_Comment by @github-actions[bot] on 2024-02-23 21:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (error)</summary>
<p>

```
ruff failed
  Cause: Rule `S410` was removed and cannot be selected.
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 1 project error; 41 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/cc1776a76788ac684f7f03bde720b5e22b21606c/disnake/ext/commands/params.py#L481'>disnake/ext/commands/params.py:481:9:</a> RUF028 This suppression comment is invalid because it cannot be in an expression, pattern, argument list, or other non-statement
+ <a href='https://github.com/DisnakeDev/disnake/blob/cc1776a76788ac684f7f03bde720b5e22b21606c/disnake/ext/commands/params.py#L498'>disnake/ext/commands/params.py:498:9:</a> RUF028 This suppression comment is invalid because it cannot be in an expression, pattern, argument list, or other non-statement
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Rule `S410` was removed and cannot be selected.
```

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF028 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 2 project errors)

<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (error)</summary>
<p>

```
ruff failed
  Cause: Rule `S410` was removed and cannot be selected.
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 2 project errors)

<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Rule `S410` was removed and cannot be selected.
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
```

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/invalid_formatter_suppression_comment.rs`:141 on 2024-02-26 09:49_

I think this is the same condition, right?
```suggestion
        if comment.kind == SuppressionKind::Skip && !comment.line_position.is_end_of_line() {
            return Err(IgnoredReason::SkipHasToBeTrailing);
        }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/suppression_comment_visitor.rs`:141 on 2024-02-26 09:56_

Can we add some inline documentation why we're doing this check? It seems we're testing if the comment and node start on the same line. Why do we stop as soon as this is no longer the case? What about comments that are on the same line as the node's end? 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/suppression_comment_visitor.rs`:252 on 2024-02-26 09:58_

Nit: Maybe move to `ruff_python_trivia::comments` (preferred because it's also used in the formatter) or to `invalid_formatter_suppression_comments` because it isn't used in this visitor.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/suppression_comment_visitor.rs`:130 on 2024-02-26 09:58_

Should this use `own_line_comment_indentation`? Or do we don't have to because we reimplement the logic but do it comment by comment?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/suppression_comment_visitor.rs`:200 on 2024-02-26 10:03_

Nit: I think it would be helpful to explain the fields in more detail or refer to the formatter struct.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/invalid_formatter_suppression_comment.rs`:241 on 2024-02-26 10:08_

Nit: Can we make this a method on `AnyNodeRef`and reuse it between the formatter and this rule? 


```suggestion
/// Returns `true` if `statement` is the first statement in an alternate `body` (e.g. the else of an if statement)
fn is_first_statement_in_alternate_body(&self, statement: AnyNodeRef) -> bool {
    match self {
        AnyNodeRef::StmtFor(ast::StmtFor { orelse, .. })
        | AnyNodeRef::StmtWhile(ast::StmtWhile { orelse, .. }) => {
            are_same_optional(statement, orelse.first())
        }

        AnyNodeRef::StmtTry(ast::StmtTry {
            handlers,
            orelse,
            finalbody,
            ..
        }) => {
            are_same_optional(statement, handlers.first())
                || are_same_optional(statement, orelse.first())
                || are_same_optional(statement, finalbody.first())
        }

        AnyNodeRef::StmtIf(ast::StmtIf {
            elif_else_clauses, ..
        }) => are_same_optional(statement, elif_else_clauses.first()),
        _ => false,
    }
}
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/invalid_formatter_suppression_comment.rs`:165 on 2024-02-26 10:15_

The following isn't supported by the formatter but doesn't get flagged

```python
class Test:
    @classmethod
    # fmt: off
    def cls_method_a(    cls) -> None:
        pass
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/invalid_formatter_suppression_comment.rs`:135 on 2024-02-26 10:16_

We may need to change the condition to whether the enclosing statement is not a statement to also catch

```python
    def cls_method_a(
            # fmt: off
            cls) -> None:
        pass
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/invalid_formatter_suppression_comment.rs`:274 on 2024-02-26 10:18_

Is this the terminology we use in the documentation too? `ambigious` region kind of makes sense but is hard to understand. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/ruff/RUF028.py`:63 on 2024-02-26 10:21_

Can we add an example for a `fmt: skip` that is combined with another suppression comment (it works ;), just to have this tested:

```python
    def cls_method_a(
            # fmt: off
            cls) -> None: 
        # noqa: test # fmt: skip
        pass
```

---

_@MichaReiser approved on 2024-02-26 10:22_

I really like how you narrowed down the scope of the rule. This looks good to me. There's one issue around suppression comments in nodes that aren't expressions that we need solving but this is ready otherwise

---

_Added to milestone `v0.3.0` by @MichaReiser on 2024-02-27 13:56_

---

_@snowsignal reviewed on 2024-02-27 16:14_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/rules/ruff/rules/invalid_formatter_suppression_comment.rs`:141 on 2024-02-27 16:14_

Oops, my bad.

---

_@snowsignal reviewed on 2024-02-27 16:17_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/rules/ruff/rules/suppression_comment_visitor.rs`:141 on 2024-02-27 16:17_

Sure, I could definitely add some documentation here. This checks, after we've determined that this comment is at the end of a line, whether or not it's the same line as the node we're moving out of. For example:
```python
def empty():
    pass # fmt: skip
```

And if it's not, the `break` means "there are no more comments to check for this node."

---

_@snowsignal reviewed on 2024-02-27 16:48_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/rules/ruff/rules/suppression_comment_visitor.rs`:252 on 2024-02-27 16:48_

We can't move it to the trivia crate, unfortunately, because of a circular dependency with `ruff_python_ast`. `invalid_formatter_suppression_comments` could work, though.

---

_@snowsignal reviewed on 2024-02-27 17:02_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/rules/ruff/rules/suppression_comment_visitor.rs`:130 on 2024-02-27 17:02_

Right - since we process each comment in order, it's assumed that any previous comments would have had valid indentation as well.

---

_@snowsignal reviewed on 2024-02-27 18:33_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/rules/ruff/rules/suppression_comment_visitor.rs`:130 on 2024-02-27 18:33_

That does leave the edge case where an indented non-suppression comment wouldn't be properly accounted for - so on second thought, I'll re-introduce `own_line_comment_indentation` here.

---

_@snowsignal reviewed on 2024-02-27 18:59_

---

_Review comment by @snowsignal on `crates/ruff_linter/resources/test/fixtures/ruff/RUF028.py`:63 on 2024-02-27 18:59_

It seems like this fails because `# fmt: skip` is treated like its on its own line. Is that not supposed to be the case?

---

_@MichaReiser reviewed on 2024-02-28 08:00_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/ruff/RUF028.py`:63 on 2024-02-28 08:00_

Ah yeah sorry, The comment should be at the end of the function header

```python
    def cls_method_a(
            cls) -> None:  # noqa: test # fmt: skip
        pass

```

---

_Merged by @snowsignal on 2024-02-28 19:21_

---

_Closed by @snowsignal on 2024-02-28 19:21_

---

_Branch deleted on 2024-02-28 19:21_

---
