```yaml
number: 21079
title: "[ruff] Implement union-with-annotated-type (RUF066)"
type: pull_request
state: open
author: wilt00
labels:
  - rule
  - preview
assignees: []
base: main
head: RUF066-implement
created_at: 2025-10-26T03:52:10Z
updated_at: 2025-12-16T16:43:46Z
url: https://github.com/astral-sh/ruff/pull/21079
synced_at: 2026-01-12T15:57:15Z
```

# [ruff] Implement union-with-annotated-type (RUF066)

---

_@wilt00_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR implements a new rule, nested-annotated-type, as discussed in #21037 . This rule fails for type definitions including `Annotated` nested within some other type.

## Test Plan

New snapshosts in `RUF066.py` with `cargo insta test`.

---

_Label `rule` added by @ntBre on 2025-10-27 16:33_

---

_Label `needs-decision` added by @ntBre on 2025-10-27 16:33_

---

_Comment by @MichaReiser on 2025-10-30 13:50_

I think this rule makes sense to me (catching `Annotated[...] | ...`) and I'd be in favor adding it because it catches a very likely mistake. But I'd be curious to hear @amyreese's opinion, too, just to ensure I'm not overlooking any valid use cases for this.


I do think we should come up with a less cryptic name :). We certainly can't use the nested terminology because the annotated documentation uses that to refer to `Annotated[Annotated]` nesting

> Nested Annotated types are flattened. The order of the metadata elements starts with the innermost annotation:
> https://docs.python.org/3/library/typing.html#typing.Annotated

Maybe something like `type-operation-with-annotated` or `mixed-annotated-regular-type`?



---

_Comment by @amyreese on 2025-10-30 22:26_

I agree that it's a useful rule — I'm not aware of any case where you would want to use an `Annotated` type with anything, let alone nest `Annotated` inside of anything other than another `Annotated` type.

For names I prefer `mixed-annotated-regular-type` over the other suggestion, but I might instead name it `union-with-annotated-type`

---

_@MichaReiser reviewed on 2025-10-30 22:32_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/ruff/RUF066.py`:6 on 2025-10-30 22:32_

What's the reason why this is an error? 

I'm asking because I like the name `union-with-annotated-type` but the name requires that it only detects usages of `Annotated` with `Union`

---

_Review comment by @amyreese on `crates/ruff_linter/resources/test/fixtures/ruff/RUF066.py`:6 on 2025-10-30 22:43_

> What's the reason why this is an error?
> 
> I'm asking because I like the name `union-with-annotated-type` but the name requires that it only detects usages of `Annotated` with `Union`

Assuming you're asking about the `Optional` example, that's functionally equivalent to `Union[Annotated[...], None]`, and it's still treated as a union in Python types. It's just a convenience because `Optional[X]` is more readable than `Union[X, None]`, though once `|` was allowed, everything converged on `X | None` rather than a dedicated syntax for optionals.

---

_@amyreese reviewed on 2025-10-30 22:43_

---

_@wilt00 reviewed on 2025-10-30 22:52_

---

_Review comment by @wilt00 on `crates/ruff_linter/resources/test/fixtures/ruff/RUF066.py`:6 on 2025-10-30 22:52_

Exactly, yes. `union-with-annotated-type` is my favorite of the suggested names as well - do you think it's common enough knowledge that optional is a subtype of union for that name to work? I can also clarify that in the diagnostic text.

---

_@amyreese reviewed on 2025-10-30 22:55_

---

_Review comment by @amyreese on `crates/ruff_linter/resources/test/fixtures/ruff/RUF066.py`:6 on 2025-10-30 22:55_

I do think it's common enough knowledge — and the context of the lint rule IMO helps bridge the gap for anyone unfamiliar — and as more projects move from `Optional[X]` to `X | None`, it becomes less and less of a concern.

---

_@wilt00 reviewed on 2025-10-30 23:18_

---

_Review comment by @wilt00 on `crates/ruff_linter/resources/test/fixtures/ruff/RUF066.py`:6 on 2025-10-30 23:18_

Sounds good. Are there other generic types that don't derive from Union that it makes sense to flag in this rule? Looking down the list of members of typing, Final is the only one that I feel could possibly fit and that's a stretch.

---

_@MichaReiser reviewed on 2025-10-30 23:19_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/ruff/RUF066.py`:6 on 2025-10-30 23:19_

Ah right, thanks

---

_Label `needs-decision` removed by @MichaReiser on 2025-10-30 23:19_

---

_@amyreese reviewed on 2025-10-30 23:33_

---

_Review comment by @amyreese on `crates/ruff_linter/resources/test/fixtures/ruff/RUF066.py`:6 on 2025-10-30 23:33_

I think it's reasonable to keep the rule relatively small in scope. I also think it's unlikely that someone would add `Final` or other type composition "by accident" the way they might with unions/optionals, not to mention being harder to reason about whether that's intended behavior or a potential bug.

---

_Renamed from "[ruff] Implement nested-annotated-type (RUF066)" to "[ruff] Implement union-with-annotated-type (RUF066)" by @wilt00 on 2025-11-01 21:06_

---

_Review requested from @amyreese by @MichaReiser on 2025-11-04 21:13_

---

_Comment by @ntBre on 2025-11-05 22:25_

I'll leave the main review to @amyreese, but this looks reasonable overall to me. Would you mind re-numbering the rule to RUF068? We have existing PRs for [RUF066](https://github.com/astral-sh/ruff/pull/9911) and [RUF067](https://github.com/astral-sh/ruff/pull/20585).

---

_Label `preview` added by @ntBre on 2025-11-05 22:25_

---

_Review comment by @amyreese on `crates/ruff_linter/resources/test/fixtures/ruff/RUF066.py`:18 on 2025-11-13 22:39_

This being an error implies that `list[Annotated[...]]` or similar would also produce an error. That part seems fine to me, but I wonder if it would be worth having a separate error message for these cases since the mention of union/optional doesn't make sense here? Would also like to have test cases showing that behavior one way or the other.

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/ruff/rules/union_with_annotated_type.rs`:49 on 2025-11-13 22:39_

Needs a bump as well 😅

---

_@amyreese requested changes on 2025-11-13 22:44_

---

_Comment by @CoderJoshDK on 2025-12-16 16:43_

Hey, I see this PR has not had any movement in the last month. If you don't mind, I think I am going to pick it up. I will include the original commits (rebased with main) and update based on the included comments. I hope I am not stepping on your toes with this.

---
