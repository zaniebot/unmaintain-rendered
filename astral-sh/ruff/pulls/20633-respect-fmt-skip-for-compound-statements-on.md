```yaml
number: 20633
title: "Respect `fmt: skip` for compound statements on single line"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: fmt-skip-one-liner
created_at: 2025-09-29T17:20:12Z
updated_at: 2025-11-18T18:02:16Z
url: https://github.com/astral-sh/ruff/pull/20633
synced_at: 2026-01-12T15:57:06Z
```

# Respect `fmt: skip` for compound statements on single line

---

_@dylwil3_

Closes #11216

Essentially the approach is to implement `Format` for a new struct `FormatClause` which is just a clause header _and_ its body. We then have the information we need to see whether there is a skip suppression comment on the last child in the body and it all fits on one line.

The `ruff` test fixture (credit: mostly created by Claude) contains several lines that seem to crash `black`, see:

- https://github.com/psf/black/issues/4782
- https://github.com/psf/black/issues/4781

and another line containing a deviation that I _think_ is unintentional for `black`, see: 
- https://github.com/psf/black/issues/4783

However, it's not clear whether we want the same behavior here? In this PR we have:

```python
if True: print("this"); print("that") # fmt: skip
```

unchanged, but we _do_ add a newline between the statements here (unlike Black):

```python
print("this"); print("that") # fmt: skip
```

(see https://github.com/astral-sh/ruff/issues/11430 and https://github.com/astral-sh/ruff/issues/17331 .)

The other deviation from `black` is that we format the placement of the skip comment itself.

For review, it is simplest to go commit by commit.

---

_Label `bug` added by @dylwil3 on 2025-09-29 17:20_

---

_Label `formatter` added by @dylwil3 on 2025-09-29 17:20_

---

_Label `preview` added by @dylwil3 on 2025-09-29 17:20_

---

_Comment by @github-actions[bot] on 2025-09-29 17:28_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_@dylwil3 reviewed on 2025-10-06 20:43_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/statement/clause.rs`:483 on 2025-10-06 20:43_

We no longer use `clause_body`, but we do use `clause_header` alone in one place - the formatting of `match` statements. Unlike match _cases_, I don't think it is legal to have an entire match statement lie on a single line, so it's okay that we don't handle that here.

---

_Marked ready for review by @dylwil3 on 2025-10-06 20:44_

---

_Review requested from @MichaReiser by @dylwil3 on 2025-10-06 20:44_

---

_Comment by @MichaReiser on 2025-10-07 07:59_

> unchanged, but we do add a newline between the statements here (unlike Black):
> ```py
> print("this"); print("that") # fmt: skip
> ```

This seems unrelated to this PR as it is a pre-existing issue, right?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/preview.rs`:43 on 2025-10-07 08:01_

Is the reason for gating this behind preview that we're concerned about bugs or are there cases where it can change existing formatted code?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/clause.rs`:41 on 2025-10-07 08:02_

Nit: I prefer to have those "helper" trait implementations after the types own `impl` block as the latter seems more important to the type

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/clause.rs`:70 on 2025-10-07 08:04_

I find this comment slightly confusing. Are you asking me to compare with `last_child_in_body` and spot the differences? 😆 

I assume this is the same as `last_child_in_body` but specific and limited to clause headers

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/clause.rs`:84 on 2025-10-07 08:05_

Nit, similar to `last_child_in_body`
```suggestion
            ClauseHeader::Class(StmtClassDef { body, .. }) 
            | ClauseHeader::Function(StmtFunctionDef { body, .. }) 
            | ClauseHeader::If(StmtIf { body, .. }) 
            | ClauseHeader::ElifElse(ElifElseClause { body, .. }) 
            | ClauseHeader::Try(StmtTry { body, .. }) 
            | 
            ClauseHeader::ExceptHandler(ExceptHandlerExceptHandler { body, .. }) => {
                body.last().map(AnyNodeRef::from)
            }
```

Same for other branches that are over `&[Stmt]` and return the last element. It makes it easier for readers to see which branches are the same

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/clause.rs`:743 on 2025-10-07 08:09_

I was confused for a second because I wondered: what about multiline clause headers that are suppressed by a `fmt: skip`? But that's handled by `clause_header`.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/clause.rs`:683 on 2025-10-07 08:10_

```suggestion
    let clause_start = header.first_keyword_range(f.context().source())?.start();

    let comments = f.context().comments().clone();

    let last_child = header
        .last_child_in_clause()
        .expect("last child to exist if `should_suppress_clause` is `Ok(true)`");
    let clause_end = last_child.end();
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/clause.rs`:775 on 2025-10-07 08:13_

I think this is correct but an explanation why this is correct might be useful. 

* What happens with the leading comments of the clause headers?
* What about dangling comments (I think they're fine because a clause header with dangling comments is always guaranteed to be multiline
* What about trailing clause comments?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:136 on 2025-10-07 08:14_

Can we derive the `SuiteKind` from the `ClauseHeader` to reduce the number of arguments?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/clause.rs`:537 on 2025-10-07 08:16_

It seems that `trailing_colon_comment` and `trailing_comments` are the same for all call sites. 
```suggestion
    header_formatter: &'a Content,
    trailing_colon_comments: &'a [SourceComment],
    body: &'a Suite,
    kind: SuiteKind,
```

I'd also flip the `header` and `trailing_colon` arguments as it matches the formatting order more closely:

```
<clause_header>: <trailing_colon_comments>
	<body>
``

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@fmt_skip__compound_one_liners.py.snap`:26 on 2025-10-07 08:20_

Change it to something that would format differently to demonstrate that this doesn't break normal `fmt: skip` behavior
```suggestion
    y  =  2  # fmt: skip
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@fmt_skip__compound_one_liners.py.snap`:156 on 2025-10-07 08:22_

What about:

```py
if (
	long_condition
):  a     +     b # fmt: skip
```


I would expect that this doesn't get formatted according to how `fmt:skip`s semantic is defined.

I don't expect this to be hard to support. It might be as easy as only changing the `countains_line_break` check to only test from `:` to the end of the clause

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@fmt_skip__compound_one_liners.py.snap`:208 on 2025-10-07 08:24_

Let's add a few more test cases with different comment placements

```suggestion
try: risky_operation()  # fmt: skip
# leading 1
except ValueError: handle_error()  # fmt: skip
# leading 2
except: handle_any_error()  # fmt: skip
# leading 3
else: success_case()  # fmt: skip
# leading 4
finally: cleanup()  # fmt: skip
# trailing
```

---

_@MichaReiser reviewed on 2025-10-07 08:24_

This is great :) I suggest adding a few more tests with different comment placements. Just to make sure we handle them correctly. 

---

_@dylwil3 reviewed on 2025-10-20 20:03_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/statement/clause.rs`:70 on 2025-10-20 20:03_

Changed. (Maybe this is an academia thing but I'm used to "compare" sometimes meaning "see also".)

---

_@dylwil3 reviewed on 2025-10-20 20:07_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/tests/snapshots/format@fmt_skip__compound_one_liners.py.snap`:208 on 2025-10-20 20:07_

This was tricky! It has dangling comments. 

But I think I figured it out: `clause_header` has us pass in optional leading comments and I wasn't handling those in the suppressed version of writing `clause`. (The correct transformation of dangling comments to leading comments happens inside the implementation of formatting for `StmtTry`, where they are passed in this way to `clause`.)

---

_@dylwil3 reviewed on 2025-10-20 20:19_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:136 on 2025-10-20 20:19_

I don't think so because I think we have to be told whether we're the last suite in the statement (but I think that's the only missing info)

---

_@dylwil3 reviewed on 2025-10-20 20:29_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/preview.rs`:43 on 2025-10-20 20:29_

removed gating

---

_Comment by @dylwil3 on 2025-10-24 17:40_

The CI failure here https://github.com/astral-sh/ruff/actions/runs/18664070057/job/53211233008?pr=20633 was caused by this PR uncovering a pre-existing bug. See #21065 and #21067 . Will rebase once that PR is merged and then continue working on this!

---

_Converted to draft by @dylwil3 on 2025-10-27 16:19_

---

_Comment by @dylwil3 on 2025-10-27 20:56_

Cut down a comment-related hydra head and more grew in its place. This snippet causes a panic because the indented comment is not marked as formatted. Annoyingly it is attached to `bar()` (because there is no `orelse` node).

```python
for x in it: foo()
    # comment
else: bar() # fmt: skip
```

Converting to draft while I sort it out 😄 

---

_Label `preview` removed by @dylwil3 on 2025-11-10 22:36_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/statement/clause.rs`:775 on 2025-11-17 16:45_

Added handling for comments in headers (turns out they can happen now because of a change we made during the course of this code review - we allow multiline headers, it's only the range starting from the colon that has to be on one line). Also clarified in comment how leading and trailing comments are handled.

---

_@dylwil3 reviewed on 2025-11-17 16:45_

---

_Marked ready for review by @dylwil3 on 2025-11-17 16:48_

---

_Review requested from @MichaReiser by @dylwil3 on 2025-11-17 16:48_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/clause.rs`:537 on 2025-11-18 09:52_

Nit: I think I'd store the data here rather than the formatters and construct the formatters on the fly in `fmt`

```suggestion
    header: ClauseHeader<'a>,
    header_formatter: &'a Content,
    leading_comments: &'a [SourceComment],
    trailing_colon_comment: &'a [SourceComment],
    body: &'a Suite,
    kind: SuiteKind,
```

Mainly because it avoids storing the `trailing_comments` twice, we only construct the formatters when necessary, and expressions like `clause.format_header.header.last_child_in_clause()` require a little less drilling (becomes `clause.header.last_child_in_clause()`)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/clause.rs`:739 on 2025-11-18 09:56_

I don't understand this comment. What are we `recalling`?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/clause.rs`:759 on 2025-11-18 09:58_

You could consider changing `should_suppress_clause` to return an enum

```rust
enum SuppressClauseHeader<'a> {
	No,
	Yes { last_child: AnyNodeRef<'a> }
}
```

But I don't think either's fine really

---

_@MichaReiser approved on 2025-11-18 09:58_

Nice

---

_@dylwil3 reviewed on 2025-11-18 17:54_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/statement/clause.rs`:739 on 2025-11-18 17:54_

I meant the line above where we did an early return, but I think the comment didn't add much so I just removed it

---

_@dylwil3 reviewed on 2025-11-18 17:54_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/statement/clause.rs`:537 on 2025-11-18 17:54_

Done, good idea!

---

_@dylwil3 reviewed on 2025-11-18 17:54_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/statement/clause.rs`:759 on 2025-11-18 17:54_

Done!

---

_Merged by @dylwil3 on 2025-11-18 18:02_

---

_Closed by @dylwil3 on 2025-11-18 18:02_

---

_Branch deleted on 2025-11-18 18:02_

---
