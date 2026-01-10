```yaml
number: 18994
title: "[ty] While loop modeling cleanup"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/while-loop-cleanup
created_at: 2025-06-27T19:24:52Z
updated_at: 2025-06-30T09:38:27Z
url: https://github.com/astral-sh/ruff/pull/18994
synced_at: 2026-01-10T18:39:09Z
```

# [ty] While loop modeling cleanup

---

_Pull request opened by @sharkdp on 2025-06-27 19:24_

## Summary

I found the previous code here very confusing, and it also did some unnecessary work. Hopefully this is a bit easier to understand.



---

_Review requested from @carljm by @sharkdp on 2025-06-27 19:24_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-06-27 19:24_

---

_Review requested from @dcreager by @sharkdp on 2025-06-27 19:24_

---

_Label `ty` added by @sharkdp on 2025-06-27 19:24_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1543 on 2025-06-27 19:27_

I think we were missing a negated reachability constraint here before.

---

_@sharkdp reviewed on 2025-06-27 19:27_

---

_Comment by @github-actions[bot] on 2025-06-27 19:28_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bandersnatch (https://github.com/pypa/bandersnatch)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~80MB

vision (https://github.com/pytorch/vision)
-     memo fields = ~304MB
+     memo fields = ~276MB

discord.py (https://github.com/Rapptz/discord.py)
-     memo fields = ~189MB
+     memo fields = ~207MB

```
</details>


---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1543 on 2025-06-27 19:30_

Is it difficult to capture this in a regression test?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1543 on 2025-06-27 19:32_

If that's the case, I should probably write a regression test for this.

---

_@sharkdp reviewed on 2025-06-27 19:33_

---

_Converted to draft by @sharkdp on 2025-06-27 19:33_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1517 on 2025-06-27 19:37_

It looks like we can replace all four of these lines, and the below call to `self.record_reachability_constraint_id(first_predicate_id)`, with the single line:
```suggestion
                let first_reachability_constraint = self.record_reachability_constraint(predicate);
```

and avoid some repeat work, too.

Also, the comment line below "So the body's full reachability constraint is `first`." might need a bit of attention, since we no longer have anything here named `first`.

---

_@carljm approved on 2025-06-27 19:42_

Nice, thank you.

---

_@sharkdp reviewed on 2025-06-27 20:52_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1543 on 2025-06-27 20:52_

> Is it difficult to capture this in a regression test?

Oh, haha. Didn't even see your comment when I wrote mine.

---

_@carljm reviewed on 2025-06-27 21:08_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1543 on 2025-06-27 21:08_

> Didn't even see your comment when I wrote mine.

That's because it wasn't there yet! GH does this funny thing where it dates a review comment to the moment it was drafted, even if it was published as part of a review batch later on.

I'm the one who didn't see your comment before submitting my review.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1543 on 2025-06-30 09:28_

That previous behavior was actually correct. The key is that the constraint was only applied after the branches had been merged. This is needs to record fewer constraints overall, so I changed it back. This also allows us to take one fewer snapshot which should help with performance.

I tried to capture this in the updates comments.

---

_@sharkdp reviewed on 2025-06-30 09:28_

---

_Marked ready for review by @sharkdp on 2025-06-30 09:29_

---

_Merged by @sharkdp on 2025-06-30 09:38_

---

_Closed by @sharkdp on 2025-06-30 09:38_

---

_Branch deleted on 2025-06-30 09:38_

---
