```yaml
number: 22099
title: Consolidate diagnostics for matched disable/enable suppression comments
type: pull_request
state: merged
author: amyreese
labels:
  - rule
  - suppression
  - preview
assignees: []
merged: true
base: main
head: amy/group-diagnostics
created_at: 2025-12-19T21:58:36Z
updated_at: 2026-01-07T02:42:54Z
url: https://github.com/astral-sh/ruff/pull/22099
synced_at: 2026-01-12T15:57:41Z
```

# Consolidate diagnostics for matched disable/enable suppression comments

---

_@amyreese_

## Summary

Combines diagnostics for matched suppression comments, so that ranges and autofixes for both
the `#ruff:disable` and `#ruff:enable` comments will be reported as a single diagnostic.

## Test Plan

Snapshot changes, added new snapshot for full output from preview mode rather than just a diff.

Issue #3711

---

_Label `rule` added by @amyreese on 2025-12-19 21:59_

---

_Label `suppression` added by @amyreese on 2025-12-19 21:59_

---

_Label `preview` added by @amyreese on 2025-12-19 21:59_

---

_Review requested from @MichaReiser by @amyreese on 2025-12-19 21:59_

---

_Review requested from @ntBre by @amyreese on 2025-12-19 21:59_

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 22:06_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/mod.rs`:340 on 2025-12-22 10:29_

Can you say what the reason is for adding this test? How is it different from `range_suppressions` (I see that it isn't a diff but it otherwise seems to capture the same)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__range_suppressions.snap`:555 on 2025-12-22 10:30_

I think the help text here is a bit misleading. It should probably be: Remove the invalid suppression

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:116 on 2025-12-22 10:38_

You can simplify your code by encoding the invariant "that there's always at least a disable-comment" into the struct:

```suggestion

    comments: DisableEnableComments,
}

#[derive(Debug)]
pub(crate) enum DisableEnableComments {
    /// An implicitly closed disable comment without a matching enable comment.
    Disable(SuppressionComment),

    /// A matching pair of disable and enable comments.
    Pair(SuppressionComment, SuppressionComment),
}

impl DisableEnableComments {
    pub(crate) fn disable_comment(&self) -> &SuppressionComment {
        match self {
            DisableEnableComments::Disable(comment) => comment,
            DisableEnableComments::Pair(disable, _) => disable,
        }
    }

    pub(crate) fn enable_comment(&self) -> Option<&SuppressionComment> {
        match self {
            DisableEnableComments::Disable(_) => None,
            DisableEnableComments::Pair(_, enable) => Some(enable),
        }
    }
}
```

It then allows you to write:


```rust
            } else if let DisableEnableComments::Disable(disable_comment) = &suppression.comments
            {
                // UnmatchedSuppressionComment
                let range = disable_comment.range;
                if unmatched_ranges.insert(range) {
                    context.report_diagnostic_if_enabled(UnmatchedSuppressionComment {}, range);
                }
            }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:1705 on 2025-12-22 10:41_

We don't need an owned comment here (this shoudl allow you to remove most `.clone` calls in debug

```suggestion
        comment: Option<&'a SuppressionComment>,
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:286 on 2025-12-22 10:43_

You want to use a `if let Some(..)` here to avoid the unwrap (similar to a `if enable_comment := suppression.enable_comment` in Python)
```suggestion
            if let Some(enable_comment) = suppression.enable_comment.as_ref() {
                let (range2, edit2) = Suppressions::delete_code_or_comment(
                    locator,
                    suppression,
                    enable_comment,
                    highlight_only_code,
                );
                diagnostic.secondary_annotation("", range2);
                diagnostic.set_fix(Fix::applicable_edits(edit, vec![edit2], applicability));
            } else {
                diagnostic.set_fix(Fix::applicable_edit(edit, applicability));
            }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:276 on 2025-12-22 10:44_

Nit: `enable_range` and `remove_enable_comment_edit` would be more meaningful names

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:179 on 2025-12-22 10:49_

Both fixes are safe, so I think we can avoid passing the fix safety here

```suggestion
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:283 on 2025-12-22 10:51_

Nit: We don't need to allocate a vector here, `applicable_edits` takes any type that implements `IntoIter`, like a stack allocated array
```suggestion
                diagnostic.set_fix(Fix::applicable_edits(edit, [edit2], applicability));
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:309 on 2025-12-22 10:54_

Let's create an issue to track fixing `delete_code_or_comment` to correctly handle the cases where a suppression comment contains repeated codes (in which case `code_index` will always delete the first unused code but never the second).

The `unwrap` call in `delete_code_or_comment` also deserves a commetn why it is okay to call `unwrap` there OR, even better, use `expect`

---

_@MichaReiser approved on 2025-12-22 10:57_

I've a few nit comments and a suggestion on how to encode the invariant that a suppression always has at least a `disable` comment. 

I also think we should delete the `_full` test. It's not clear what value it adds over the existing test

---

_@amyreese reviewed on 2026-01-05 22:39_

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/ruff/mod.rs`:340 on 2026-01-05 22:39_

It was a lot easier to analyze and debug the changes to the test cases when only seeing the raw output/diff of changes, rather than trying to decipher the diff of diffs. We don't need to keep it, but it was extremely helpful when working on this PR.

---

_@amyreese reviewed on 2026-01-05 23:56_

---

_Review comment by @amyreese on `crates/ruff_linter/src/suppression.rs`:1705 on 2026-01-05 23:56_

How do I then deal with the ownership of the `SuppressionComment` created in `parse_suppression_comment()`?  https://github.com/astral-sh/ruff/pull/22099/changes#diff-35bdff8d5bced4b2e861a0c49467cce54db87355ba881a3e7482e936fead6750L1561

With your suggested change, I get `cannot return value referencing local variable comment returns a value referencing data owned by the current function`.

---

_@MichaReiser reviewed on 2026-01-06 07:45_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/mod.rs`:340 on 2026-01-06 07:45_

I'm leaning towards removing it as it's now mainly creating noise by duplicating all changes and it seems easy enough to re-add when debugging

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:1571 on 2026-01-06 07:51_

```suggestion
            TextRange::new(offset, source.text_len()),
```

---

_@MichaReiser reviewed on 2026-01-06 07:51_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:1705 on 2026-01-06 08:00_

Oh, I didn't see that use. Yeah, this doesn't work. You could use a `Cow` which is an enum over an owned value or a reference. But it's not worth it given that this ist test only.

The other solution is to move the ownership one level up, but this will make your tests more verbose (as in, there's more repeated code).

```rust
    #[test]
    fn enable_single_code() -> Result<(), ParseError> {
        let source = "# ruff: enable[some-thing]";
        let comment = parse_suppression_comment(source)?;
        
        assert_debug_snapshot!(
            DebugSuppressionComment::new(comment, source),
            @r##"
            SuppressionComment {
                text: "# ruff: enable[some-thing]",
                action: Enable,
                codes: [
                    "some-thing",
                ],
                reason: "",
            },
        "##,
        );

				Ok(())
    }
```

So, I think keeping what you have is fine. 

(Changing your test function to return a `Result` might be worth it (or always calling `unwrap` on parse_suppression_comment` to avoid the noisy `Ok` wrapping in the snapshots

---

_@MichaReiser reviewed on 2026-01-06 08:00_

---

_Merged by @amyreese on 2026-01-07 02:42_

---

_Closed by @amyreese on 2026-01-07 02:42_

---

_Branch deleted on 2026-01-07 02:42_

---
