```yaml
number: 17410
title: "[`pyflakes`] Add fix safety section (`F841`)"
type: pull_request
state: merged
author: Kalmaegi
labels:
  - documentation
assignees: []
merged: true
base: main
head: document_fix_safety
created_at: 2025-04-15T15:59:23Z
updated_at: 2025-04-17T15:07:11Z
url: https://github.com/astral-sh/ruff/pull/17410
synced_at: 2026-01-12T15:56:02Z
```

# [`pyflakes`] Add fix safety section (`F841`)

---

_@Kalmaegi_

add fix safety section to docs for #15584, I'm new to ruff and not sure if the content of this PR is correct, but I hope it can be helpful.

---

_Label `documentation` added by @AlexWaygood on 2025-04-15 15:59_

---

_Comment by @Kalmaegi on 2025-04-15 16:17_

A small question: Do we need to list all possible cases that may lead to modifications in the comments, and ensure that there are test cases for all of them?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyflakes/rules/unused_variable.rs`:47 on 2025-04-15 18:50_

I think we can leave this out. This is pretty well covered by our general docs on [fix safety](https://docs.astral.sh/ruff/linter/#fix-safety).

```suggestion
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyflakes/rules/unused_variable.rs`:45 on 2025-04-15 18:53_

I'm not sure if this second point is entirely true. We at least have a heuristic for preserving the right-hand side:

https://github.com/astral-sh/ruff/blob/431f10e7885a1a9057fb40f67d4df73c28650e32/crates/ruff_linter/src/rules/pyflakes/rules/unused_variable.rs#L209-L212

Are there test cases showing a false positive with a function? If not, maybe we could find another example here.

---

_@ntBre reviewed on 2025-04-15 19:08_

Thanks for working on this!

This rule is a little tricky since it's always unsafe and there's not much commentary in the file. Did you try finding the original PR adding the rule? That could be a good place to find more discussion. I tried a bit and didn't find it, though.

If we can't find any further discussion, I think what you have is fine, despite my comment on the side effects point below.

> A small question: Do we need to list all possible cases that may lead to modifications in the comments, and ensure that there are test cases for all of them?

I think just a couple of reasons for the unsafety is good, like you have here. I think I'd also lean away from modifying code or tests in these PRs and just add docs for what's there now. On the other hand, if you add tests that help you determine when the fix is safe or not, we might as well include them! (That may be more applicable for rules that are _sometimes_ safe, unlike this one)

---

_@Kalmaegi reviewed on 2025-04-16 02:14_

---

_Review comment by @Kalmaegi on `crates/ruff_linter/src/rules/pyflakes/rules/unused_variable.rs`:45 on 2025-04-16 02:14_

You're absolutely right, I got some details wrong. Thank you very much for your help! Next, I'll check the logic of the other rules and conduct some thorough testing.

---

_Comment by @Kalmaegi on 2025-04-16 09:07_

> Thanks for working on this!
> 
> This rule is a little tricky since it's always unsafe and there's not much commentary in the file. Did you try finding the original PR adding the rule? That could be a good place to find more discussion. I tried a bit and didn't find it, though.
> 
> If we can't find any further discussion, I think what you have is fine, despite my comment on the side effects point below.
> 
> > A small question: Do we need to list all possible cases that may lead to modifications in the comments, and ensure that there are test cases for all of them?
> 
> I think just a couple of reasons for the unsafety is good, like you have here. I think I'd also lean away from modifying code or tests in these PRs and just add docs for what's there now. On the other hand, if you add tests that help you determine when the fix is safe or not, we might as well include them! (That may be more applicable for rules that are _sometimes_ safe, unlike this one)

The only potential insecurity of this rule seems to be that it deletes comments (if any) when removing content. I've reviewed several other rules, and they all appear to behave similarly. For such cases, do we just need to list the unique associations related to the comments?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyflakes/rules/unused_variable.rs`:44 on 2025-04-16 17:21_

```suggestion
/// This rule's fix is marked as unsafe because removing an unused variable assignment may
/// delete comments that are attached to the assignment.
```

---

_@ntBre approved on 2025-04-16 17:22_

Thank you! One more tiny stylistic nit. I think the list format might be good for rules with many possible unsafe conditions, but we typically keep these as regular paragraphs.

---

_Comment by @ntBre on 2025-04-16 17:30_

> The only potential insecurity of this rule seems to be that it deletes comments (if any) when removing content. I've reviewed several other rules, and they all appear to behave similarly. For such cases, do we just need to list the unique associations related to the comments?

I think what you have here is great. For some other rules, you could list a couple of examples of places where comments could be deleted if you want, but most existing rules I found just say that comments could be deleted without more explanation, so that's fine to do too.

---

_Comment by @Kalmaegi on 2025-04-17 00:56_

@ntBre Thank you very much for your help! It has been incredibly helpful in deepening my understanding of these matters. Initially, I tried to identify all possible points of impact, but apart from the deletion of comments, I didn’t find any others—the same goes for the other files. Moving forward, I will strive to conduct more thorough checks and carefully review the other rules. I believe this is a good start. Once again, I truly appreciate your efforts!

---

_Comment by @ntBre on 2025-04-17 13:55_

No problem, thanks for working on this! I'll give your other PRs a look soon.

I pushed one small commit just updating the test snapshot that shows the rule documentation. Then I'll merge right after the checks finish!

---

_Comment by @github-actions[bot] on 2025-04-17 13:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "add fix safety section to docs" to "[`pyflakes`] Add fix safety section (`F841`)" by @ntBre on 2025-04-17 13:57_

---

_Merged by @ntBre on 2025-04-17 13:58_

---

_Closed by @ntBre on 2025-04-17 13:58_

---

_Comment by @Kalmaegi on 2025-04-17 15:07_

> No problem, thanks for working on this! I'll give your other PRs a look soon.
> 
> I pushed one small commit just updating the test snapshot that shows the rule documentation. Then I'll merge right after the checks finish!

Thank you for your help! I'm not sure if I checked everything thoroughly, but most of them don't have side effects.

---
