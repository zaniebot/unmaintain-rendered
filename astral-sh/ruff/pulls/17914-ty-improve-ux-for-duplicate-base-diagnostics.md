```yaml
number: 17914
title: "[ty] Improve UX for `[duplicate-base]` diagnostics"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/dup-bases-diagnostic
created_at: 2025-05-07T12:47:22Z
updated_at: 2025-05-07T15:27:46Z
url: https://github.com/astral-sh/ruff/pull/17914
synced_at: 2026-01-12T15:56:08Z
```

# [ty] Improve UX for `[duplicate-base]` diagnostics

---

_@AlexWaygood_

## Summary

This PR improves the UX of our `duplicate-base` diagnostics so that we:
- Say explicitly what's wrong with including a duplicate base (it's going to raise `TypeError` at runtime)
- Highlight the original occurence of the base in the bases list as well as its second occurence
- If a base is duplicated multiple times, only report a single diagnostic for all occurences rather than multiple diagnostics

<details>
<summary>Screenshot of what the new diagnostics look like in the terminal</summary>

![image](https://github.com/user-attachments/assets/6d8a7bd5-0e03-418a-8c82-8db5212aeabc)

</details>

## Test Plan

Snapshots added


---

_Review requested from @carljm by @AlexWaygood on 2025-05-07 12:47_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-07 12:47_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-07 12:47_

---

_Label `ty` added by @AlexWaygood on 2025-05-07 12:47_

---

_Comment by @github-actions[bot] on 2025-05-07 12:50_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @MichaReiser on 2025-05-07 12:53_

Nice. 

I think this does have the downside that it's now only possible to suppress the error at the first or last line of the class declaration but not where the base class is repeated (because suppressions use the primary range).

Have you considered changing the primary range to the arguments range instead of the entire class header?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/mro.rs`:336 on 2025-05-07 12:56_

```suggestion
    /// the second index is the base's invalid later occurrence.
```

There can be more than two occurences. Which I think also shows a shortcoming of this implementation because we would still emit three diagnostics for `class Bar(Foo, Foo, Foo, Foo)`. 

Ideally, we would group all repetitive bases together

---

_@AlexWaygood reviewed on 2025-05-07 12:57_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:781 on 2025-05-07 12:57_

Adding snapshots revealed that we weren't reporting all diagnostics in source order if there were multiple `[duplicate-base]` diagnostics in the same file. This fixes that.

---

_Comment by @AlexWaygood on 2025-05-07 12:58_

> I think this does have the downside that it's now only possible to suppress the error at the first or last line of the class declaration but not where the base class is repeated (because suppressions use the primary range).

Yeah. I think this is justifiable, though, given that it's the class definition as a whole that will fail and raise an exception at runtime, not any of the sub-expressions inside the argument list.

> Have you considered changing the primary range to the arguments range instead of the entire class header?

I could do that, but again, it's the class definition as a whole that will fail; the name of the class feels like relevant information that's useful to have highlighted

---

_@MichaReiser reviewed on 2025-05-07 12:58_

---

_@AlexWaygood reviewed on 2025-05-07 13:00_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/mro.rs`:336 on 2025-05-07 13:00_

hmm, true

---

_@AlexWaygood reviewed on 2025-05-07 13:27_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/mro.rs`:336 on 2025-05-07 13:27_

working on this

---

_@AlexWaygood reviewed on 2025-05-07 13:45_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/mro.rs`:336 on 2025-05-07 13:45_

Fixed in https://github.com/astral-sh/ruff/pull/17914/commits/1ec9b0ee8588dbe7247177fefd41a8dfa5c9a9c1

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-05-07 13:46_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/mro.md`:316 on 2025-05-07 13:48_

Could you add an example where we suppress the diagnostic. Just to make sure it works as expected when suppressing it on line 309 or 316

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer.rs`:781 on 2025-05-07 13:49_

We should fix this in the mdtest framework. We already sort diagnostics in the CLI

---

_@MichaReiser approved on 2025-05-07 13:50_

---

_@carljm approved on 2025-05-07 13:51_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:781 on 2025-05-07 15:05_

I'll revert this for now and accept a snapshot with the diagnostics in an odd order. Will tackle the mdtest sorting in a followup.

---

_@AlexWaygood reviewed on 2025-05-07 15:05_

---

_@AlexWaygood reviewed on 2025-05-07 15:26_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/mro.md`:316 on 2025-05-07 15:26_

added in https://github.com/astral-sh/ruff/pull/17914/commits/e99571bfb2d4faa55322670d21644ed1ccd83a58

---

_Merged by @AlexWaygood on 2025-05-07 15:27_

---

_Closed by @AlexWaygood on 2025-05-07 15:27_

---

_Branch deleted on 2025-05-07 15:27_

---
