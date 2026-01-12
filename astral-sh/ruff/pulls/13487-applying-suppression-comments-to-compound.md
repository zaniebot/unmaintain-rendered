```yaml
number: 13487
title: Applying Suppression Comments to Compound Statements
type: pull_request
state: closed
author: bnkc
labels:
  - formatter
assignees: []
draft: true
base: main
head: suppression_on_clause_header
created_at: 2024-09-23T20:40:13Z
updated_at: 2024-10-14T20:23:46Z
url: https://github.com/astral-sh/ruff/pull/13487
synced_at: 2026-01-12T15:55:44Z
```

# Applying Suppression Comments to Compound Statements

---

_@bnkc_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR addresses [issue #11216](https://github.com/astral-sh/ruff/issues/11216). It introduces a first attempt at implementing special handling for `# fmt: skip` and `# fmt: off`, so that they apply to entire compound statements. There are still some failing tests to resolve, but I’d appreciate feedback on whether the general approach is sound, as well as any suggestions for improvement.

I’m also curious if this change might require reworking existing tests.

## Test Plan

A new test has been added in `crates/ruff_python_formatter/src/comments/mod.rs`:

```rust
#[test]
fn fmt_skip_applies_to_entire_compound_statement() {
    let source = r#"
if True: x = 0  # fmt: skip
def foo(self, lorem, ipsum, dolor, sit, amet, consectetur, adipiscing, elit, sed, do): ...  # fmt: skip
"#;

    let test_case = CommentsTestCase::from_code(source);
    let comments = test_case.to_comments();

    assert_debug_snapshot!(comments.debug(test_case.source_code));
}
```

The test essentially reproduces the reported issue. There are still edge cases to address, and I’m continuing to refine the solution. I’ve been diving into this codebase for a few days, so apologies for any oversights. Looking forward to your feedback!

---

_Review requested from @MichaReiser by @bnkc on 2024-09-23 20:40_

---

_Renamed from "Resolving Bug https://github.com/astral-sh/ruff/issues/11216" to "Resolving Bug #11216" by @bnkc on 2024-09-23 20:40_

---

_Renamed from "Resolving Bug #11216" to "Resolving Bug " by @bnkc on 2024-09-23 20:40_

---

_Renamed from "Resolving Bug " to "Resolving Bug #11216" by @bnkc on 2024-09-23 20:40_

---

_Renamed from "Resolving Bug #11216" to "Applying Suppression Commments to Compound Statements" by @bnkc on 2024-09-23 20:41_

---

_Converted to draft by @bnkc on 2024-09-23 20:41_

---

_Renamed from "Applying Suppression Commments to Compound Statements" to "Applying Suppression Comments to Compound Statements" by @bnkc on 2024-09-23 20:51_

---

_@MichaReiser reviewed on 2024-09-24 10:04_

Solving this inside the comment placement is clever. I like it. 

My only concern is that not all clause headers have their own node. For example, the `else` header of many statements is just a `Vec<Node>`:

```
while test:
    pass
else:  ... # fmt: skip
```

I recommend to look into how to support these clause headers as a next step.

---

_Label `formatter` added by @MichaReiser on 2024-09-24 10:04_

---

_Comment by @bnkc on 2024-09-26 16:08_

> Solving this inside the comment placement is clever. I like it.
> 
> My only concern is that not all clause headers have their own node. For example, the `else` header of many statements is just a `Vec<Node>`:
> 
> ```
> while test:
>     pass
> else:  ... # fmt: skip
> ```
> 
> I recommend to look into how to support these clause headers as a next step.

Hey made a small change. Let me know if this is what we are looking for

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:477 on 2024-09-27 15:43_

I think this is an over-simplification. E.g. 

```python
def test(
	a, 
	b, 
): ... # fmt: skip

def test() -> List[
	str,
	int,
]: ... # fmt: skip

if (
	a and b and c and [
		1, 
		2,
): ... # fmt: skip 
```

There's `ClauseHeader` that has some fairly complicated logic to find the exact range of the header

https://github.com/astral-sh/ruff/blob/138e70bd5c01d61061247c43b1bbb33d0290a18f/crates/ruff_python_formatter/src/statement/clause.rs#L40-L70

maybe we can reuse some of the logic here

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:480 on 2024-09-27 15:47_

I'm not sure if making the comment a trailing comment is correct. Doesn't it suppress formatting for the entire node? I think the comment should be a dangling comment, see 

https://github.com/astral-sh/ruff/blob/6ac61d7b89b2ac3789c2d8ecde505f8c739c6f35/crates/ruff_python_formatter/src/comments/placement.rs#L373-L392

---

_@MichaReiser reviewed on 2024-09-27 15:47_

---

_Comment by @MichaReiser on 2024-10-07 06:58_

I looked at this a bit more closely and I don't think a `placement.rs` only solution is going to work. What makes me belive so is that I changed the source in `quick_test` to 

```rust
let source = r#"
def foo(self, lorem, ipsum, dolor, sit, amet, consectetur, adipiscing, elit, sed, do): print( " teest ")  # fmt: skip
"#;
```

The test then fails with:

```
The node has dangling comments that need to be formatted manually. Add the special dangling comments handling to `fmt_fields`.
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

Which makes sense when we take a look at the `FormatClauseHeader` implementation

https://github.com/astral-sh/ruff/blob/a0afe7857eb26554da2694422d536b14479b115a/crates/ruff_python_formatter/src/statement/clause.rs#L368-L395

Specifically at how it formats a suppressed clause header

https://github.com/astral-sh/ruff/blob/a0afe7857eb26554da2694422d536b14479b115a/crates/ruff_python_formatter/src/verbatim.rs#L954-L980

You can see how it only formats the clause header in verbatim mode but not the body. We need to extend the clause handling with a branch that skips both the clause header and body formatting when the sinlge-body statement is suppressed

---

_Comment by @bnkc on 2024-10-10 16:12_

@MichaReiser Thanks for the feedback! I'll get to this over the weekend :)

---

_Review requested from @MichaReiser by @bnkc on 2024-10-14 20:23_

---

_Closed by @bnkc on 2024-10-14 20:23_

---

_Branch deleted on 2024-10-14 20:23_

---
