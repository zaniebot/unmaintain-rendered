```yaml
number: 22119
title: "Respect `fmt: skip` for multiple statements on same logical line"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: fmt-skip-suite
created_at: 2025-12-20T21:44:08Z
updated_at: 2026-01-11T21:46:27Z
url: https://github.com/astral-sh/ruff/pull/22119
synced_at: 2026-01-12T15:57:42Z
```

# Respect `fmt: skip` for multiple statements on same logical line

---

_@dylwil3_

This PR adjusts the logic for skipping formatting so that a `fmt: skip` can affect multiple statements if they lie on the same line.

Specifically, a `fmt: skip` comment will now suppress all the statements in the suite in which it appears whose range intersects the line containing the skip directive. For example:

```python
x=[
'1'
];x=2 # fmt: skip
```

remains unchanged after formatting.

(Note that compound statements are somewhat special and were handled in a previous PR - see #20633).


Closes #17331 and #11430.

Simplest to review commit by commit - the key diffs of interest are the commit introducing the core logic, and the diff between the snapshots introduced in the last commit (compared to the second commit).

# Implementation

On `main` we format a suite of statements by iterating through them. If we meet a statement with a leading or trailing (own-line)`fmt: off` comment, then we suppress formatting until we meet a `fmt: on` comment. Otherwise we format the statement using its own formatting rule.

How are `fmt: skip` comments handled then? They are handled internally to the formatting of each statement. Specifically, calling `.fmt` on a statement node will first check to see if there is a trailing, end-of-line `fmt: skip` (or `fmt: off`/`yapf: off`), and if so then write the node with suppressed formatting.

In this PR we move the responsibility for handling `fmt: skip` into the formatting logic of the suite itself. This is done as follows:

- Before beginning to format the suite, we do a pass through the statements and collect the data of ranges with skipped formatting. More specifically, we create a map with key given by the _first_ skipped statement in a block and value a pair consisting of the _last_ skipped statement and the _range_ to write verbatim.
- We iterate as before, but if we meet a statement that is a key in the map constructed above, we pause to write the associated range verbatim. We then advance the iterator to the last statement in the block and proceed as before.

## Addendum on range formatting

We also had to make some changes to range formatting in order to support this new behavior. For example, we want to make sure that

```python
<RANGE_START>x=1<RANGE_END>;x=2 # fmt: skip
```

formats verbatim, rather than becoming 

```python
x = 1;x=2 # fmt: skip
```

Recall that range formatting proceeds in two steps:
1. Find the smallest enclosing node containing the range AND that has enough info to format the range (so it may be larger than you think, e.g. a docstring has enclosing node given by the suite, not the string itself.)
2. Carve out the formatted range from the result of formatting that enclosing node.

We had to modify (1), since the suite knows how to format skipped nodes, but nodes may not "know" they are skipped. To do this we altered the `visit_body` bethod of the `FindEnclosingNode` visitor: now we iterate through the statements and check for skipped ranges intersecting the format range. If we find them, we return without descending. The result is to consider the statement containing the suite as the enclosing node in this case.

---

_Label `bug` added by @dylwil3 on 2025-12-20 21:44_

---

_Label `formatter` added by @dylwil3 on 2025-12-20 21:44_

---

_Comment by @astral-sh-bot[bot] on 2025-12-20 21:53_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/statement/suite.rs`:1027 on 2025-12-22 16:14_

If you don't like increasing the visibility of `NodeRefEqualityKey` there are other things we could do here. For example, we could enumerate with the indices of the statements within the suite - this has the downside of requiring lots of changes to code that expects to receive an `iter` over statements to instead receive an iter over pairs of indices and statements.

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/statement/suite.rs`:1042 on 2025-12-22 16:14_

Copying `NodeRefEqualityKey` is cheap right?

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/other/decorator.rs`:16 on 2025-12-22 16:16_

This is now the only place `suppressed_node` is used. Should we inline it?

---

_@dylwil3 reviewed on 2025-12-22 16:18_

---

_Marked ready for review by @dylwil3 on 2025-12-22 16:19_

---

_Review requested from @MichaReiser by @dylwil3 on 2025-12-22 16:19_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/lib.rs`:63 on 2025-12-22 16:41_

Let's add some new tests for range formatting a `fmt: skip`ed statement list 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:1071 on 2025-12-22 16:48_

Could we extract this data in `Comments::from_ast` instead? That would fit better into the overall formatter design where there's one pre-processing pass to extract comments (which then are stored on `Comments`). 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:1042 on 2025-12-22 16:48_

It's as cheap as copying a reference

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:200 on 2025-12-22 16:51_

Let's move this into a helper function similar to how we handle `fmt_off` comments and mark that function as cold. The function is already rather big

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:449 on 2025-12-22 16:52_

Is this whole block the same as above? Can we avoid duplicating the logic?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:1031 on 2025-12-22 16:53_

What's the reason we need the last statement too? Would it be enough to test whether the next statement is covered by `verbatim_range`?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:1027 on 2025-12-22 16:55_

Would it be possible to use an interval map (`Vec<TextRange>`) over an `FxHashMap` keyed by `NodeRefEqualityKey`. A node is suppressed if its range intersects with any range in the interval map.

(Similar to `ExpressionsScopeMap` in ty but for ranges instead of node ids https://github.com/astral-sh/ruff/blob/b4c2825afdd8c1010c3a5859521629d2d1e0a6df/crates/ty_python_semantic/src/semantic_index.rs#L734)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:1079 on 2025-12-22 16:56_

It's rather unfortunate that this is `O(statements)` rather than `O(comments)`

---

_@MichaReiser reviewed on 2025-12-22 16:57_

Thanks for looking into this long standing issues. 

I haven't done an in-depth review but I left some initial questions. We can also use our 1:1 to discuss some of them

---

_@MichaReiser reviewed on 2025-12-22 18:17_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:1106 on 2025-12-22 18:17_

Looking at this code makes me wonder whether we need the pre-processing at all. 

Given the current statement and an `std::slice::Iter` (that you can cheaply clone) over the remaining statements: Can't we implement this roughly like this:


```rust
let Some(mut trailing_semicolon) = trailing_semicolon(stmt.into(), source) else {
	return; 
}


let mut lookahead = statements_iter.clone();
while let Some(next_statement) = lookahead.next() {
	if has_skip_comment(next_statement) {
		break;
	} else if let Some(trailing) = trailing_semicolon(next_statement.into()) {
		trailing_semicolon = trailing;
		// Keep going
	} else {
		// Has no skip comment
		return false;
}

// Call fmt:skip suppressed formatting, passing in the original `statements_iter` (and move it forward
```

The downside of this approach is that it's `O(n^2)` where `n` is the number of statements on the same line. But I'm not sure this is a big issue because a) it would only be slow on the first format call and b) it's unlikely anyone has that many statements on a single line for this to be an issue. 

If this indeed becomes a problem, we can then add a caching similar to your `FxHashMap` where we store the already checked ranges, so that we skip the same work on the next call.

---

_Comment by @MichaReiser on 2025-12-22 20:28_

Regarding the benchmark. I don't think using hyperfine will give you meaningful numbers. There's just too much overhead in ruff's cli (config discovery, directory walking, io) in addition to the noise of measuring a cli tool.

If you want to get meaningful numbers, I suggest adding a benchmark to ruff_benchmark. Even if it's just locally

---

_Converted to draft by @dylwil3 on 2025-12-26 21:34_

---

_Renamed from "Respect `fmt: skip` for multiple statements on same line" to "Respect `fmt: skip` for multiple statements on same logical line" by @dylwil3 on 2026-01-05 17:34_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/statement/suite.rs`:976 on 2026-01-05 20:35_

I experimented with computing the tokens within the suite range just once, then passing the slice along to this method and mutating it like

```
tokens = tokens[last_statement.end()..]
```

as I went along. I thought this could help performance since then the binary search should terminate more quickly, but this did not lead to an improvement compared to the present approach.

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/tests/snapshots/format@fmt_skip__semicolons.py.snap`:178 on 2026-01-05 20:37_

See #22406

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/statement/suite.rs`:983 on 2026-01-05 22:02_

It is tempting to do something like check the source code directly for a newline. This does not work because there may be a line continuation character. So maybe you could get around that, but how do you know it's a line continuation character instead of something like:

```python
x=1 # a comment with _not_ a line continuation \
x=2
```
At this point it seemed like we were re-inventing lexing, so why not use the tokens instead? There is the cost of the binary searches, but in the most common case we only do it once per statement in the suite, and it appears to not affect performance very much.

---

_@dylwil3 reviewed on 2026-01-05 22:03_

---

_Marked ready for review by @dylwil3 on 2026-01-05 22:03_

---

_@MichaReiser reviewed on 2026-01-05 22:12_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:976 on 2026-01-05 22:12_

Could we use the simple tokenizer instead (it has the advantage that it doesn't require O(n) scanning, instead, it's a single lookahead at the current text position)

---

_@MichaReiser reviewed on 2026-01-05 22:13_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:983 on 2026-01-05 22:13_

It seems `SimpleTokenizer` does support `Continuation`

---

_@dylwil3 reviewed on 2026-01-05 23:26_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/statement/suite.rs`:976 on 2026-01-05 23:26_

Nice, thanks! That's a little perf bump... although it's hard to tell whether it was just noise - the actual diffs in the perf comparison aren't very illuminating. In any case we can use the `SimpleTokenizer` if it's preferred!

---

_@MichaReiser reviewed on 2026-01-06 08:01_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:976 on 2026-01-06 08:01_

I would prefer it simply because it's what we use almost everywhere in the formatter. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:966 on 2026-01-06 08:11_

and use `iter.as_slice()` at the call site

```suggestion
    statements: &[Stmt],
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:1004 on 2026-01-06 08:25_

I always feel a bit uneasy about tokenizer calls where we have a blanket match arm that eats over any tokens. 

I would write this as

```rust
    while let Some(token) = tokenizer.next() {
        match token {
            SimpleTokenKind::Continuation => {
                // Skip over the newline
                tokenizer.next();
            }
            SimpleTokenKind::Newline => {
                return true;
            }
            kind => {
                if !kind.is_trivia() {
                    return false;
                }
            }
        }
    }
```

We could even consider making the `kind.is_trivia` a debug assertion

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/verbatim.rs`:466 on 2026-01-06 08:39_

I think you could use `statements.peeking_next` here instead of `statements.clone().next`
```suggestion
    while let Some(prec) = statements.peeking_next(|next| next.end() <= verbatim_range.end())
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:970 on 2026-01-06 08:42_

I think it would be ncie if we wouldn't need `new_logical_line_between_statements` and instead could use `trailing_semicolon` directly because both functions give you the same answer: Are these multiple statements on a single line. 

I did try to rewrite the function to only make use of `trailing_semicolon` but I messed up somewhere. Maybe you've an idea on how to make it work, this is what I ended up with:

```rust
    let source = f.context().source();
    let comments = f.context().comments();
    let start = first.start();

    // Handle the common case first where a statement has no trailing semicolon.
    let Some(trailing_semicolon) = trailing_semicolon(first.into(), source) else {
        return if has_skip_comment(comments.trailing(first), source) {
            Some(first.range())
        } else {
            None
        };
    };

    let mut last_statement = first;
    let mut end = trailing_semicolon.end();

    for statement in statements.clone() {
        last_statement = statement;

        if let Some(trailing_semicolon) =
            crate::statement::trailing_semicolon(statement.into(), source)
        {
            end = trailing_semicolon.end();
        } else {
            end = statement.end();
            break;
        }
    }

    if has_skip_comment(comments.trailing(last_statement), source) {
        Some(TextRange::new(start, end))
    } else {
        None
    }
```

And I changed `trailing_semicolon` to:

```rust
pub(super) fn trailing_semicolon(node: AnyNodeRef, source: &str) -> Option<TextRange> {
    debug_assert!(node.is_statement());

    let mut tokenizer = SimpleTokenizer::starts_at(node.end(), source);

    while let Some(token) = tokenizer.next() {
        match token.kind() {
            SimpleTokenKind::Continuation => {
                // Skip over the newline
                tokenizer.next();
            }
            SimpleTokenKind::Newline => {
                return None;
            }
            SimpleTokenKind::Semi => {
                return Some(token.range());
            }
            kind if kind.is_trivia() => {
                // continue
            }
            _ => {
                return None;
            }
        }
    }

    None
}

```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/fmt_skip/semicolons.py`:8 on 2026-01-06 08:45_

Let's add an example with a trailing semicolon

```suggestion
    x=2;x=3.  ; # fmt: skip
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@range_formatting__fmt_skip.py.snap`:80 on 2026-01-06 08:52_

The outputs here look wrong to me. I would expect `x=1` and `x=3` to be formatted. 

This may be a pre-existing bug, in which case we can fix it separately but we should verify that this is indeed the case (and isn't an issue with how we emit source positions in this new PR)

---

_@MichaReiser reviewed on 2026-01-06 08:53_

Nice, this looks pretty simple now. 

Do we still need the `is_suppressed` logic in the statement formatting?

---

_@dylwil3 reviewed on 2026-01-06 12:17_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/statement/suite.rs`:970 on 2026-01-06 12:17_

> and instead could use trailing_semicolon directly because both functions give you the same answer: Are these multiple statements on a single line.

I can try but is this really true?

```python
x = 1;
x = 2
```
The first statement has a trailing semicolon but they are on different logical lines.

---

_@MichaReiser reviewed on 2026-01-06 12:20_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:970 on 2026-01-06 12:20_

You're right, we need the newline handling

---

_@dylwil3 reviewed on 2026-01-06 13:58_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/statement/suite.rs`:1004 on 2026-01-06 13:58_

I'll add this but observe that, since we are specifically in a range between statements, I don't think any non-trivia tokens can actually occur.

---

_Comment by @dylwil3 on 2026-01-09 21:23_

While fixing the behavior for range formatting, I noticed that, unrelated to any suppression comments, we do the following:

```python
x=1;<RANGE_START>x=2<RANGE_END>
```

becomes:

```python
x=1;    x = 2
```

Is this expected or should I make an issue for it?

The enclosing node of the range here is correctly determined to be the full suite (because it can't find the indentation for `x=2`), but then something must go wrong when narrowing from the formatted

```python
x=1
x=2
```

back down. My guess is that this comes down to the `is_logical_line` helper being not quite right, or else maybe we aren't emitting/interpreting source positions correctly here? I haven't looked closely.

---

_@dylwil3 reviewed on 2026-01-09 21:35_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/statement/suite.rs`:1004 on 2026-01-09 21:35_

I ended up keeping this as is but adding a comment with the justification I gave above - let me know if you feel strongly and I'll commit your suggestion!

---

_Review requested from @MichaReiser by @dylwil3 on 2026-01-09 21:35_

---

_Comment by @dylwil3 on 2026-01-09 21:38_

As you predicted, removing `is_suppressed` got rid of what little perf regression was left: https://codspeed.io/astral-sh/ruff/runs/compare/69615827861fdc44a0a9aaea..6961718d1af8d4524db090e5

(except for a teensy one on `numpy/globals.py`)

---

_Comment by @MichaReiser on 2026-01-10 09:44_

> Is this expected or should I make an issue for it?

Yeah, this looks very wrong. Opening an issue for it would be great

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/decorator.rs`:15 on 2026-01-10 09:46_

I don't think it's necessary to clone `comments` here because we don't borrow them across a `write!( or `fmt` call

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/decorator.rs`:18 on 2026-01-10 09:48_

We only need the trailing comments

```rust
        let trailing_comments = comments.trailing(item);

        if has_skip_comment(trailing_comments, f.context().source()) {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/range.rs`:273 on 2026-01-10 09:50_

Do we need to reset `self.suppressed` even when early exiting? Maybe just break here?

---

_@MichaReiser approved on 2026-01-10 09:52_

---

_Merged by @dylwil3 on 2026-01-10 14:56_

---

_Closed by @dylwil3 on 2026-01-10 14:56_

---

_Comment by @madduck on 2026-01-11 21:46_

Thanks a lot for your work on this @dylwil3, and everyone who was also involved! ♥

---
