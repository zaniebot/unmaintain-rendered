```yaml
number: 20777
title: Render unsupported syntax errors in formatter tests
type: pull_request
state: merged
author: ntBre
labels:
  - formatter
  - testing
assignees: []
merged: true
base: main
head: brent/render-syntax-errors
created_at: 2025-10-08T22:00:21Z
updated_at: 2025-10-13T14:11:59Z
url: https://github.com/astral-sh/ruff/pull/20777
synced_at: 2026-01-12T15:57:09Z
```

# Render unsupported syntax errors in formatter tests

---

_@ntBre_

## Summary

Based on the suggestion in https://github.com/astral-sh/ruff/issues/20774#issuecomment-3383153511, I added rendering of unsupported syntax errors in our `format` test. 

In support of this, I added a `DummyFileResolver` type to `ruff_db` to pass to `DisplayDiagnostics::new` (first commit). Another option would obviously be implementing this directly in the fixtures, but we'd have to import a `NotebookIndex` somehow; either by depending directly on `ruff_notebook` or re-exporting it from `ruff_db`. I thought it might be convenient elsewhere to have a dummy resolver, for example in the parser, where we currently have a separate rendering pipeline [copied](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_parser/tests/fixtures.rs#L321) from our old rendering code in `ruff_linter`. I also briefly tried implementing a `TestDb` in the formatter since I noticed the `ruff_python_formatter::db` module, but that was turning into a lot more code than the dummy resolver.

We could also push this a bit further if we wanted. I didn't add the new snapshots to the black compatibility tests or to the preview snapshots, for example. I thought it was kind of noisy enough (and helpful enough) already, though. We could also use a shorter diagnostic format, but the full output seems most useful once we accept this initial large batch of changes.

## Test Plan

I went through the baseline snapshots pretty quickly, but they all looked reasonable to me, with one exception I noted below. I also tested that the case from #20774 produces a new unsupported syntax error.

---

_Label `formatter` added by @ntBre on 2025-10-08 22:00_

---

_Label `testing` added by @ntBre on 2025-10-08 22:00_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/format@expression__binary.py.snap`:982 on 2025-10-08 22:03_

I think this might be an unrelated bug. I would expect this range/underline to look more like:
```
invalid-syntax: Cannot use t-strings on Python 3.13 (syntax was added in Python 3.14)
 --> try.py:1:15
  |
1 |   aaaaaaaaaaa = t"hellooooooooooooooooooooooo \
  |  _______________^
2 | |         worlddddddddddddddddddddddddddddddddd" + (aaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbb)
  | |______________________________________________^
  |
```

which is what we produce for the unformatted file when I tried it separately.

---

_@ntBre reviewed on 2025-10-08 22:08_

---

_Comment by @github-actions[bot] on 2025-10-08 22:14_

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

_Marked ready for review by @ntBre on 2025-10-08 22:16_

---

_Review requested from @carljm by @ntBre on 2025-10-08 22:16_

---

_Review requested from @MichaReiser by @ntBre on 2025-10-08 22:16_

---

_Review requested from @sharkdp by @ntBre on 2025-10-08 22:16_

---

_Review requested from @dcreager by @ntBre on 2025-10-08 22:16_

---

_Review request for @dcreager removed by @ntBre on 2025-10-08 22:17_

---

_Review request for @carljm removed by @ntBre on 2025-10-08 22:17_

---

_Review request for @sharkdp removed by @ntBre on 2025-10-08 22:17_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:1196 on 2025-10-09 06:08_

I don't think this is necessary. You can just return `Path::new(".")`

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@expression__binary.py.snap`:965 on 2025-10-09 06:27_

It took me a while to find the message and it seems to be different from what I see in the playground where the primary annotation message comes right after the [invalid-syntax]. 

I haven't checked the CLI but ideally, the rendering would be the same.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@expression__binary.py.snap`:982 on 2025-10-09 06:31_

This looks the same when using `ruff check`. 

```
invalid-syntax: Cannot use t-strings on Python 3.10 (syntax was added in Python 3.14)
   --> scratch.py:496:5
    |
494 |   # wrapped in parentheses.
495 |   (
496 | /     t"hellooooooooooooooooooooooo \
497 | |         worlddddddddddddddddddddddddddddddddd"
    | |______________________________________________^
498 |       + (aaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbb)
499 |   )
    |
```

The ranges look correct in the playground. So this might be diagnostic rendering specific

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@statement__type_alias.py.snap`:408 on 2025-10-09 06:34_

Let's bump the python version for this test to be at least 3.10. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@statement__with.py.snap`:811 on 2025-10-09 06:44_

Hmm, this is somewhat annoying. What we really want here is to not have new syntax errors. Showing all makes it sort of difficult to spot any accidential syntax errors (when it introduced a new syntax error).

That made me think about fingerprinting again and maybe there's a way to improve the fingerprint. 

The formatter shouldn't insert or remove statements. That means, we should be able to match diagnostics to their corresponding statement (include that in the fingerprint). Maybe something like this: 

* If there are any syntax errors
* Traverse the entire AST and build a Vec that contains the range of every statement (in sorted order)
* For each syntax error, search for the statement (index) with the smallest range that fully contains the syntax error's range
* Use that index as part of the fingerprint

We could also just traverse the AST for every syntax error instead of building a vec first

---

_@MichaReiser reviewed on 2025-10-09 06:44_

---

_@ntBre reviewed on 2025-10-09 13:00_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render.rs`:1196 on 2025-10-09 13:00_

Ah wow 🤦 It looks like I can simplify the version in the linter too:

https://github.com/astral-sh/ruff/blob/4917e080ef9bd13d4e72aec68d8f1c2d37088aa6/crates/ruff_linter/src/fs.rs#L12-L15

I assumed `Path::new` returned a `Path` and not a `&Path`.

---

_@ntBre reviewed on 2025-10-09 13:06_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/format@expression__binary.py.snap`:965 on 2025-10-09 13:06_

Oh right, that's the difference between `Diagnostic::invalid_syntax` and the linter's `create_syntax_error_diagnostic`. I can just copy the latter over here. 

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/format@statement__with.py.snap`:811 on 2025-10-09 13:18_

I guess my thinking was that this looks a lot more annoying here than it will in the future, unless we often add many new inputs with unsupported syntax at once. And we can avoid some of those by splitting up the test cases into version-specific groups as you mention above, but that might be bad for other reasons I haven't run into yet.

I can definitely look more into the fingerprinting though, that was my first thought when running into this yesterday. I think I may have briefly explored something vaguely along these lines even. I was hoping that we could attach the `node_index` for the AST node to the syntax error when constructing it, at least in tests, but it looks like we just use `NONE` for the node index in the parser (and often emit syntax errors before we have an AST node anyway).

---

_@ntBre reviewed on 2025-10-09 13:18_

---

_@MichaReiser reviewed on 2025-10-09 13:24_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@statement__with.py.snap`:811 on 2025-10-09 13:24_

Yeah, `node_index` is only initialized in ty. But it's roughly the same as just walking the tree in source order (`SourceOrderVisitor`) and assigning ids. 

---

_@ntBre reviewed on 2025-10-09 14:59_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/format@statement__with.py.snap`:811 on 2025-10-09 14:59_

The good news is that this seems to work quite well. I think we can probably revert the unsupported syntax error snapshots? It still seems useful to render the new syntax errors as full diagnostics in the `Formatted code introduced new unsupported syntax errors` message, though, so I think we can keep the resolver.

The bad news is that this uncovered a few more issues in the f-string tests. Not sure about others since that panic aborts the test run.

---

_@ntBre reviewed on 2025-10-09 15:47_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/format@statement__with.py.snap`:811 on 2025-10-09 15:47_

Actually I guess I looked at this too quickly right before standup. All but one of the new panics is kind of a false positive. In cases like this:

```py
f"foo {'\'bar\''}"
```

we know it's okay to reuse a quote since that's allowed on the same Python version as escapes. But the fingerprints are different: we lose a "Cannot use an escape sequence" error and gain a "Cannot reuse outer quote character" error.

So I think we should keep the snapshots but restrict them to the new errors detected by the new fingerprinting scheme and remove this hard panic. Maybe that's what you had in mind initially.

---

_@ntBre reviewed on 2025-10-09 15:55_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/format@statement__with.py.snap`:831 on 2025-10-09 15:55_

These also appear to be false positives. They seem somewhat related to our discussion [here](https://github.com/astral-sh/ruff/pull/16523#discussion_r1984657793), but I thought that only applied to tuples. These cases are fine on 3.8 besides the runtime errors:

```pycon
Python 3.8.20 (default, Oct  2 2024, 16:34:12)
[Clang 18.1.8 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> with (
...   1 + 2
... ): ...
...
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: __enter__
>>> if True:
...   with (
...     1
...     if True
...     else 2
...   ):
...     pass
...
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
AttributeError: __enter__
```

---

_@MichaReiser reviewed on 2025-10-09 17:01_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@statement__with.py.snap`:811 on 2025-10-09 17:01_

Can you say why the fingerprints are different? I would have expected them to be the same because the syntax error belongs to the same statement.

If the fingerprints are different, we could then consider a `# invalid-syntax: allow` pragma comment to allowlist the few cases where we intentionally want to allow them.

---

_@ntBre reviewed on 2025-10-09 17:25_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/format@statement__with.py.snap`:811 on 2025-10-09 17:25_

We hash the syntax error kind, which has the same variant (`Pep701FString`) but a different inner variant (`Pep701FString(Backslash)` before vs `Pep701FString(NestedQuote)` after).

I can try playing with the pragma comment.

---

_Comment by @github-actions[bot] on 2025-10-09 18:54_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-09 18:55_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@ntBre reviewed on 2025-10-09 18:56_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/format@statement__with.py.snap`:811 on 2025-10-09 18:56_

I feel like there may be a nicer approach here, but I got something working by checking the preceding line for a pragma comment. It works well for the f-string cases. The remaining syntax error snapshots look like bugs that I plan to follow up on.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/fixtures.rs`:624 on 2025-10-10 05:58_

You can use `CommentRanges::from(parsed.tokens())` instead of `Indexer` since you don't need any of the other `Indexer` metadata.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/fixtures.rs`:637 on 2025-10-10 06:02_

I don't think this is correct, or at least, it's not as narrow as it could be

```py
if True:
	a = 10
```

The range of the `if` statement contains both the range of `True` but also of `a = 10`. 

What we want is to find the node with the narrowest range that fully encloses `range`. Roughly:

* Find the first range that contains `range.start` (you could even use a binary search)
* Now find the last range that still contains `range.end`, this is the most narrow range



---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/fixtures.rs`:664 on 2025-10-10 06:03_

Let's only build this when `parsed.unsupported_syntax_errors` isn't empty

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/fixtures.rs`:670 on 2025-10-10 06:04_

I think we should panic here or at least use `None` as node id instead of omitting the syntax error.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/fixtures.rs`:489 on 2025-10-10 06:07_

Should we add a note saying that, if expected, they can be suppressed with `invalid-syntax: allow`?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@expression__fstring.py.snap`:2421 on 2025-10-10 06:10_

Are these only the syntax errors that weren't suppressed with `invalid-syntax` allowed? I'm asking because it's not entirely clear to me when I'm supposed to use `invalid-syntax` vs when it's okay to just tolerate the invalid syntax errors. 

I think I'd either keep panicking in the presence of an invalid syntax error that isn't explicitly suppressed with an `invalid-syntax` pragma comment or render them:

The risk I see with the pragma comments is that it might unintentionally suppress more syntax errors than I intended (e.g. in the `with` test case, I might allow `invalid-syntax` errors to suppress the diagnostics for 3.8, but then introduce an error for 3.9). On the other hand, it makes it very clear that unsupported syntax errors need to be fixed before a PR is merged (or they need to be explicitly suppressed).

If we go with rendering the errors, then I think we should add a short paragraph right below Unsupported syntax errors, saying that these need to be fixed unless they're pre-existing syntax errors already present in the source (and not introduced by the formatter).

---

_@MichaReiser reviewed on 2025-10-10 06:19_

Nice, I like this a lot more. I think there's a question whether we want to render the unsupported syntax errors (I'm leaning towards that) or have the pragma comment. I do find having both a bit confusing because it's unclear when I'm supposed to use which.

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/format@expression__fstring.py.snap`:2421 on 2025-10-10 13:20_

Yes, we only render the leftover syntax errors that weren't present initially and weren't suppressed with the pragma comment. With the pragma comments, I was thinking we ideally wouldn't tolerate any snapshots, but I didn't want to fix these bugs in the same PR :sweat_smile: So I see what you mean, we should go back to panicking instead of snapshotting once these are resolved.

I think we may also be able to print the fingerprint for new errors so that you could include that in the pragma comment, to make them a bit more specific. Or maybe part of the error message would be easier to use, like in mdtests.

But I think I would still lean toward just rendering all of the new errors too and not having the pragma comments. That's a bit simpler and we didn't have many errors to render once we updated the fingerprint filtering anyway.

---

_@ntBre reviewed on 2025-10-10 13:20_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/fixtures.rs`:670 on 2025-10-10 13:22_

This one needs to be optional because `node_id` also filters out the pragma comments. Maybe that's a strange API in itself. But I can also restore the `unwrap` inside of `node_id` for finding the enclosing node. That's what I had before I added in the comment check.

---

_@ntBre reviewed on 2025-10-10 13:22_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/fixtures.rs`:637 on 2025-10-10 14:10_

I was having some trouble with the binary search, so I took a more literal approach:

```rust
        let (position, node_range) = self
            .nodes
            .iter()
            .enumerate()
            .filter(|(_, node)| node.contains_range(range))
            .min_by_key(|(_, node)| node.len())
            .unwrap();
```

---

_@ntBre reviewed on 2025-10-10 14:10_

---

_@MichaReiser reviewed on 2025-10-10 14:27_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@expression__fstring.py.snap`:2421 on 2025-10-10 14:27_

> But I think I would still lean toward just rendering all of the new errors too and not having the pragma comments. That's a bit simpler and we didn't have many errors to render once we updated the fingerprint filtering anyway.

Sounds good to me. Sorry for misdirecting you on the pragma comments

---

_@ntBre reviewed on 2025-10-10 15:33_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/format@expression__fstring.py.snap`:2421 on 2025-10-10 15:33_

No worries, it was interesting to try out! I'll keep my cleanup commits from this morning and then revert them all, just in case we ever want to resurrect them.

---

_@ntBre reviewed on 2025-10-10 18:21_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/format@statement__with.py.snap`:831 on 2025-10-10 18:21_

I've looked into this a bit, and I actually don't think we can fix this easily. As noted in the thread linked above, we decided to be a bit more aggressive here and flag tuple cases as syntax errors since they will always cause an error at runtime. But since we parse these two cases the same way:

```py
with (a): ...  # ok, even at runtime, if `a` implements the protocol
with (a,): ... # runtime error on 3.8, tuple definitely doesn't implement the protocol
```

we can't differentiate between a single `WithItem` passed to the context manager, which is okay on 3.8, and a single-item tuple, which we intentionally wanted to flag.

Since 3.8 is EOL and we haven't gotten any user reports, I think we should just accept these snapshots in line with our previous discussion.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/fixtures.rs`:617 on 2025-10-12 05:49_

I'm fine with this as I don't think it matters much. But if you're interested. Here's an example using `partition_point` to find the first node that starts at or before a given offset.

https://github.com/astral-sh/ruff/blob/6ef72e71edab6436626bff527d2e69a022978c7b/crates/ruff_python_trivia/src/comment_ranges.rs#L39-L41

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@expression__fstring.py.snap`:2421 on 2025-10-12 05:51_

I'm not sure if all those errors are now okay or if some of those are new errors introduced by the formatter. If any of them falls into the latter category, could you add a `FIXME` to the fixture file above the relevant line?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@statement__with.py.snap`:831 on 2025-10-12 05:56_

I think we could fix it by tracking in `try_parse_parenthesized_with_items` whether there's a trailing comma (peek for a trailing comma inside the `parse_comma_separated_list` callback) and then return it as part of `try_parse_parenthesized_with_items`'s result? 

But I also agree, I don't think this is worth spending much time on.

---

_@MichaReiser approved on 2025-10-12 05:57_

Very nice find and thanks for taking the time to make our formatter test suite more robust!

---

_@ntBre reviewed on 2025-10-13 13:30_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/fixtures.rs`:617 on 2025-10-13 13:30_

Ah okay, thank you! I found the examples above that using `binary_search_by`, but `partition_point`  is good to know about.

I think I'll just leave it as-is for now if it doesn't matter too much.

---

_@ntBre reviewed on 2025-10-13 13:40_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/format@expression__fstring.py.snap`:2421 on 2025-10-13 13:40_

I thought the errors for line 762 were related to https://github.com/astral-sh/ruff/issues/20774, but it turns out that they're false positives with our syntax error detection instead. I'll add the FIXME and open a separate issue.

---

_Merged by @ntBre on 2025-10-13 14:00_

---

_Closed by @ntBre on 2025-10-13 14:00_

---

_Branch deleted on 2025-10-13 14:00_

---

_@ntBre reviewed on 2025-10-13 14:11_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/format@statement__with.py.snap`:831 on 2025-10-13 14:11_

I'm looking at this quickly now. `parse_comma_separated_list` already returns whether there was a trailing comma, so this might be an easy fix.

---
