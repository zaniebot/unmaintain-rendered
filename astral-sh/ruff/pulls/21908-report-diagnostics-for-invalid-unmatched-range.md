```yaml
number: 21908
title: Report diagnostics for invalid/unmatched range suppression comments
type: pull_request
state: merged
author: amyreese
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: amy/unmatched-suppressions
created_at: 2025-12-10T23:21:31Z
updated_at: 2025-12-18T20:58:59Z
url: https://github.com/astral-sh/ruff/pull/21908
synced_at: 2026-01-12T15:57:36Z
```

# Report diagnostics for invalid/unmatched range suppression comments

---

_@amyreese_

## Summary

- Adds new RUF103 and RUF104 diagnostics for invalid and unmatched suppression comments
- Reports RUF104 diagnostics for any unmatched suppression comment

## Test Plan

Updated snapshots from test cases with unmatched suppression comments

Issue #3711
Fixes #21878
Fixes #21875 


---

_Comment by @astral-sh-bot[bot] on 2025-12-10 23:30_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Renamed from "Report diagnostics for unmatched range suppression comments" to "Report diagnostics for invalid/unmatched range suppression comments" by @amyreese on 2025-12-11 01:17_

---

_Comment by @MichaReiser on 2025-12-11 08:40_

Uff, `ruff: isort` 😆 

---

_Marked ready for review by @amyreese on 2025-12-12 01:26_

---

_Review requested from @MichaReiser by @amyreese on 2025-12-12 01:26_

---

_Review requested from @ntBre by @amyreese on 2025-12-12 01:26_

---

_Label `rule` added by @MichaReiser on 2025-12-12 09:00_

---

_Label `preview` added by @MichaReiser on 2025-12-12 09:00_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/ruff/suppressions.py`:94 on 2025-12-12 09:01_

Can you add a test that verifies we don't flag rules defined using [`external`](https://docs.astral.sh/ruff/settings/#lint_external) (might have to be a CLI test)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/invalid_suppression_comment.rs`:30 on 2025-12-12 09:03_

```suggestion
    pub(crate) kind: InvalidSuppressionCommentKind,
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unmatched_suppression_comment.rs`:16 on 2025-12-12 09:05_

I suggest adding a few statements so that you can place the `ruff: enable` comment before the end of the block (which is sort of the main reason for the rule)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__range_suppressions.snap`:264 on 2025-12-12 09:07_

Maybe something like: Suppression comment without matching `ruff: enable` comment

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__range_suppressions.snap`:299 on 2025-12-12 09:10_

We actually do the opposite in ty:

* We highlight the entire comment if all codes are unused (and the message is: *unused ignore comment*)
* We highlight the unused codes if not all of them are unused

This has the advantage that IDEs render the entire comment or just the unused codes as "unnecessary". 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__range_suppressions.snap`:314 on 2025-12-12 09:11_

I didn't notice this when reviewing your previous PR. 

I think we should create a single diagnostic for a single `disable/enable` pair. It''s okay to address this in a separate PR

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:266 on 2025-12-12 09:15_

I'm not sure we should offer a fix for invalid suppression comments. At least not a safe fix. We don't really know what the user wants to do in this case. Fix the comment? Remove it? 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:235 on 2025-12-12 09:16_

Does `noqa` offer a fix if the rule code is invalid? Removing is one possible solution but the user might also have misspelled the rule name and the proper fix is to fix the rule name instead.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:276 on 2025-12-12 09:18_

I think this is another case where I'd prefer to have dedicated `enable` and `disable` fields on `Suppression`. It would remove the need for code like this to "poke" into the internals.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:271 on 2025-12-12 09:25_

We should probably avoid flagging valid comments with invalid rule names. It might be best to unify the two checks into a single loop (and add a test)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__range_suppressions.snap`:597 on 2025-12-12 09:26_

The message here needs updating (it mentions `noqa`)

---

_@MichaReiser reviewed on 2025-12-12 09:26_

Overall this looks good. A few more smaller suggestions

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/invalid_suppression_comment.rs`:28 on 2025-12-12 19:37_

We'll have to bump these after the release this week.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/invalid_suppression_comment.rs`:24 on 2025-12-12 19:39_

It might be worth linking to the related rules like the other one in this PR and RUF100. Sometimes we have a `## See also` section for this.

---

_Review comment by @ntBre on `crates/ruff_linter/src/suppression.rs`:199 on 2025-12-12 19:46_

nit: I often avoid the binding if I can:


```suggestion
                    context.report_diagnostic(
                        UnusedNOQA {
                            codes: Some(codes),
                            kind: UnusedNOQAKind::Suppression,
                        },
                        range,
                    ).set_fix(Fix::safe_edit(edit));
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/suppression.rs`:279 on 2025-12-12 19:53_

This probably doesn't help much since I think it collects something internally, but if this is just for getting the unique values, you can use the [unique](https://docs.rs/itertools/latest/itertools/trait.Itertools.html#method.unique) adaptor from itertools.

(mostly an excuse to show you itertools, if you haven't seen it yet)

---

_@ntBre reviewed on 2025-12-12 19:58_

Nice! Just a couple of small things from me beyond Micha's comments.

This is a bit off topic, and I'm not sure if y'all have discussed this (or already implemented it?), but I was thinking it would be nice to have a code action to wrap the selected region with these comments, like we have for adding a noqa comment.

---

_Comment by @MichaReiser on 2025-12-12 20:06_

> This is a bit off topic, and I'm not sure if y'all have discussed this (or already implemented it?), but I was thinking it would be nice to have a code action to wrap the selected region with these comments, like we have for adding a noqa comment.

I think this is trickier because the main use case is to suppress multiple statements. It still recommended to use noqa for local suppressions. 

Showing too many code actions is also noisy, which is why I wouldn't implement this for now

---

_@amyreese reviewed on 2025-12-16 23:09_

---

_Review comment by @amyreese on `crates/ruff_linter/resources/test/fixtures/ruff/suppressions.py`:94 on 2025-12-16 23:09_

Should we consider external rule codes "valid" outside of a `#noqa` context? Ie, do we actually expect external tools/linters to support `#ruff:disable` style suppressions?

---

_@amyreese reviewed on 2025-12-16 23:37_

---

_Review comment by @amyreese on `crates/ruff_linter/src/suppression.rs`:235 on 2025-12-16 23:37_

Yes, the noqa implementation does a range deletion of either the entire comment or the individual invalid code, and also marks those as safe edits.

---

_@amyreese reviewed on 2025-12-17 02:18_

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__range_suppressions.snap`:314 on 2025-12-17 02:18_

I think I need to learn/understand how to build diagnostics with multiple fixes.

---

_Comment by @amyreese on 2025-12-17 02:24_

Checkpointing the feedback I got to today.

---

_Comment by @amyreese on 2025-12-18 02:12_

Reorganized the logic to reduce repeat iterations and only consider each suppression for one type of violation at a time (I think that's what Micha was getting at?). I'd like to leave consolidating the diagnostics and any other refactors/structural changes to follow-up PRs.

---

_Review requested from @MichaReiser by @amyreese on 2025-12-18 02:12_

---

_Review requested from @ntBre by @amyreese on 2025-12-18 02:12_

---

_@MichaReiser reviewed on 2025-12-18 10:12_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/ruff/suppressions.py`:94 on 2025-12-18 10:12_

I guess that makes sense

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__range_suppressions.snap`:562 on 2025-12-18 10:15_

It feels a bit off that we highlight the entire suppression here, given that the suppression itself is valid, it's just the rule code that isn't. 

I'm inclined to change the range here just to the code.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__range_suppressions.snap`:567 on 2025-12-18 10:15_

Maybe: *Remove the suppression comment* if all codes are invalid

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:234 on 2025-12-18 10:18_

I wonder if this is confusing for users that expect `ruff: disable` to work without rule codes, and having it report a invalid syntax error would be more helpful in that case (we could even special case it with a help text, saying they should add a rule code)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:199 on 2025-12-18 10:20_

```suggestion
                    let rule = Rule::from_code(
                        get_redirect_target(&suppression.code).unwrap_or(&suppression.code),
                    ).expect("Should trigger `InvalidRuleCode`);
```

---

_@MichaReiser approved on 2025-12-18 10:22_

A few more nits about which diagnostics we emit but this otherwise looks good.

---

_@amyreese reviewed on 2025-12-18 16:59_

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__range_suppressions.snap`:567 on 2025-12-18 16:59_

Changing this would also change the help text for all `#noqa` directives as well. Should that happen in a separate PR?

---

_@MichaReiser reviewed on 2025-12-18 18:04_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__range_suppressions.snap`:567 on 2025-12-18 18:04_

Yeah, maybe

---

_@amyreese reviewed on 2025-12-18 19:50_

---

_Review comment by @amyreese on `crates/ruff_linter/src/suppression.rs`:199 on 2025-12-18 19:50_

Turns out I can't do this because configured external rules are "valid" from the previous clause, but can't be unwrapped to a `Rule` here, and we don't want to treat those as unused either.

---

_Merged by @amyreese on 2025-12-18 20:58_

---

_Closed by @amyreese on 2025-12-18 20:58_

---

_Branch deleted on 2025-12-18 20:58_

---
