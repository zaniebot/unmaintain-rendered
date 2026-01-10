```yaml
number: 21783
title: New diagnostics for unused range suppressions
type: pull_request
state: merged
author: amyreese
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: amy/unused-suppressions
created_at: 2025-12-04T02:21:05Z
updated_at: 2025-12-09T16:43:56Z
url: https://github.com/astral-sh/ruff/pull/21783
synced_at: 2026-01-10T16:42:11Z
```

# New diagnostics for unused range suppressions

---

_Pull request opened by @amyreese on 2025-12-04 02:21_

Adds new diagnostics for reporting unused range suppressions.

- Updates RUF100 to accept an enum to determine whether the diagnostic mentions `noqa` or "suppression" in the description and fix title.
- Updates existing noqa logic to pass the appropriate enum when reporting RUF100 diagnostics.
- Adds new logic to the new ranged `Suppressions` system to track whether individual suppressions/codes have been "used", and then report appropriat RUF100 diagnostics for unused suppressions/codes.
- Adds new integration tests exercising a variety of unused suppression cases.

Issue #3711 

---

_Comment by @astral-sh-bot[bot] on 2025-12-04 02:28_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unused_suppression.rs`:37 on 2025-12-04 07:15_

Did you create a new rule because that was easier or do you forsee use cases where users want to disable unused block level suppression warnings but not warnings about unused `noqa` comments?

---

_@MichaReiser reviewed on 2025-12-04 07:15_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:349 on 2025-12-04 07:18_

Should we just pass `suppressions` here (the owned value), given that they aren't used afterwards?


Another alternative would be to store them on `LintContext` (I don't remember if that would be hard. It's okay to only look at this at the very end. Just floating this as an idea). Ultimately, I think, where we store it probably depends on where we want to use it, e.g. e might need to pass them to a new lint rule that checks for unclosed suppressions)

---

_@MichaReiser reviewed on 2025-12-04 07:18_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:191 on 2025-12-04 07:20_

Nit: I would probably use some early returns here to reduce the nesting
```suggestion
        if !context.is_rule_enabled(Rule::UnusedSuppression) {
            return;
        }

        let unused_suppressions = self.valid.iter().filter(|suppression| !suppression.used);

        for suppression in unused_suppressions {
            for comment in &suppression.comments {
                let edit = if comment.codes.len() == 1 {
                    delete_comment(comment.range, locator)
                } else {
                    let code_index = comment
                        .codes
                        .iter()
                        .position(|range| locator.slice(range) == suppression.code)
                        .unwrap();
                    let code_range = if code_index < (comment.codes.len() - 1) {
                        TextRange::new(
                            comment.codes[code_index].start(),
                            comment.codes[code_index + 1].start(),
                        )
                    } else {
                        comment.codes[code_index]
                    };
                    Edit::range_deletion(code_range)
                };

                let mut diagnostic = context
                    .report_diagnostic(UnusedSuppression, suppression.comments[0].range);
                diagnostic.set_fix(Fix::safe_edit(edit));
            }
        }
    }
```

---

_@MichaReiser reviewed on 2025-12-04 07:20_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:182 on 2025-12-04 07:22_

I think the logic here only works now because there are always at most 2 comments. This loop would, otherwise, delete the `+1` comment twice. 

I'd be leaning towards splitting `comments` into two fields: `disable_comment` (could also be an `ignore` in the future), `enable_comment: Option<...>`. This way, making the assumption here that we always have at most two comments is safe (and it also removes the need to find the comment again.)

---

_@MichaReiser reviewed on 2025-12-04 07:22_

---

_@amyreese reviewed on 2025-12-04 16:36_

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/ruff/rules/unused_suppression.rs`:37 on 2025-12-04 16:36_

Both 😅 

---

_@amyreese reviewed on 2025-12-04 16:44_

---

_Review comment by @amyreese on `crates/ruff_linter/src/suppression.rs`:182 on 2025-12-04 16:44_

> I think the logic here only works now because there are always at most 2 comments. This loop would, otherwise, delete the `+1` comment twice.

I'm not sure I understand what you mean by this.  This logic is deciding whether to remove the entire comment (because it is only suppressing one lint code), vs removing just the unused lint code if the comment has multiple codes. That logic shouldn't be affected by whether there's only a 'disable' comment or whether there's a matching 'enable' comment. In the latter case it would just generate two separate diagnostics, though I was originally hoping to build a single diagnostic with two fixes, but as far as I can tell that's not yet supported?

That said, the logic here is not finished, I just wanted to checkpoint what I had before I had to leave for an event. 

---

_@amyreese reviewed on 2025-12-04 16:49_

---

_Review comment by @amyreese on `crates/ruff_linter/src/linter.rs`:349 on 2025-12-04 16:49_

I went back and forth a couple times on what pieces should own the creation of the `Suppressions` object. The biggest problem is that the existing APIs are very inconsistent on what's available where, like how far context/locator/tokens get passed through. I like the idea of adding them to `LintContext` but that context isn't passed to `generate_noqa_edits` for example.

---

_Marked ready for review by @amyreese on 2025-12-05 16:20_

---

_Comment by @amyreese on 2025-12-05 16:22_

Not particularly happy with `&mut` everywhere, but not sure what other options I have, especially now that we're passing in suppressions objects from further up the stack.

---

_Comment by @MichaReiser on 2025-12-05 16:26_

> Not particularly happy with &mut everywhere, but not sure what other options I have, especially now that we're passing in suppressions objects from further up the stack.

One option you have is to use [interior mutability](https://doc.rust-lang.org/book/ch15-05-interior-mutability.html) by making `Suppression::used` a `Cell<bool>` or `AtomicBool`. 

I don't mind the `&mut` overly much, but this is also a case where using iterior mutability seems a great fit because the fact that marking suppressions as used requires mutating isn't something that API users should really be aware of and using `Cell<bool>` allows you to hide that. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:166 on 2025-12-05 16:28_

Nit: I would probably call this `check_suppressions` because it doesn't return a set of diagnostics.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:179 on 2025-12-05 16:29_

I think this is a divergence from unused `noqa` suppressions, and I think it's very desirable to flag unused suppressions after you disabled a rule. That's why I think we shouldn't skip here


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:184 on 2025-12-05 16:30_

I think we want to store the suppression range on `Suppression` to correctly handle:

```
# ruff:disable[RUF100, RUF100]
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unused_suppression.rs`:37 on 2025-12-05 16:34_

Having two rules might be confusing to users. I can see how the old rule name isn't great, but the name of this rule is generic enough that a user could think that it also handles `noqa` comments. 

Given that our long-term plan is to merge all suppression comments, I'd lean towards reusing the old rule and renaming it later. 



---

_@MichaReiser requested changes on 2025-12-05 16:36_

The implementation looks great. 

We should add some tests for the new rule that show that the fixes are working as expected. This is the reason why I'm requesting changes.

We should also add some tests demonstrating how you can use `ruff:disable` to suppress `unused-suppression` errors within a block.

I'm also leaning towards reusing the existing rules but I'm curious to learn about your motivation for having different rules (and how that expands to the other checks we want to add)

---

_Comment by @amyreese on 2025-12-06 01:11_

Updated to use `Cell<bool>` rather than `&mut`, to reuse the existing UnusedNOQA rule, as well as to report disabled codes and comments with no codes listed. Duplicate codes are not yet handled.

Currently reports a separate diagnostic for each code due to the way the `Suppression` struct was designed. I did not realize the current NOQA rules combine everything into a single diagnostic, and I did not like my initial attempt to try and create a single diagnostic from the current data model. That said, `ruff check --fix` does converge on deleting everything unused, including the entire comment if all of the codes are unused, though presumably requires more iterations which is undesirable. 

Not sure if it's worth reworking the data model before going further on this, or if I should worry about that later, or if you have a better idea of which direction to go.

---

_Comment by @MichaReiser on 2025-12-06 12:19_

> Not sure if it's worth reworking the data model before going further on this, or if I should worry about that later, or if you have a better idea of which direction to go.

That shouldn't be necessary (other than storing the range of the codes?). We use a similar data structure in ty, and we group consecutive unused codes together, which is perfectly fine (it can result in multiple diagnostics if most codes are unused but only creates one diagnostic if all codes are unused)

See https://github.com/astral-sh/ruff/blob/adf468beb53bfc0a06698d4e4f265a02545f37a1/crates/ty_python_semantic/src/suppression.rs#L260-L289


We can address this in a follow-up PR (which is also what I did in ty, the initial version didn't support grouping)


---

_Label `rule` added by @MichaReiser on 2025-12-06 12:19_

---

_Label `preview` added by @MichaReiser on 2025-12-06 12:19_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:78 on 2025-12-06 12:21_

We should double check if the unused are still necessary

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:223 on 2025-12-06 12:24_

Should this use `codes`?

```suggestion
                        codes: Some(codes),
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__range_suppressions.snap`:279 on 2025-12-06 12:26_

Let's add some more tests demonstrating some edge cases:

* Duplicate rule codes (it's okay to add a todo)
* Disabling `RUF100` 
* Overlapping ranges (`ruff: disable[E501` followed by another `ruff: disable[E501]`)
* Multiple codes where only one code is unused
* Multiple codes where only some codes are unused
* Multiple codes were all codes are unused

---

_@MichaReiser reviewed on 2025-12-06 12:26_

---

_@amyreese reviewed on 2025-12-09 00:22_

---

_Review comment by @amyreese on `crates/ruff_linter/src/suppression.rs`:223 on 2025-12-09 00:22_

yes 🙈

---

_@MichaReiser reviewed on 2025-12-09 15:40_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/ruff/suppressions.py`:60 on 2025-12-09 15:40_

It's fine if one of them counts as unused, for as long as we remove the right one.

---

_@MichaReiser approved on 2025-12-09 15:41_

---

_Merged by @amyreese on 2025-12-09 16:30_

---

_Closed by @amyreese on 2025-12-09 16:30_

---

_Branch deleted on 2025-12-09 16:30_

---
