```yaml
number: 18833
title: "[`pylint`] Fix false negative in `modified-iterating-set` lint when target variable is reassigned"
type: pull_request
state: closed
author: IDrokin117
labels: []
assignees: []
base: main
head: fix/ruff-18818
created_at: 2025-06-20T20:10:07Z
updated_at: 2025-06-24T06:22:23Z
url: https://github.com/astral-sh/ruff/pull/18833
synced_at: 2026-01-12T15:56:26Z
```

# [`pylint`] Fix false negative in `modified-iterating-set` lint when target variable is reassigned

---

_@IDrokin117_

## Summary

Resolves https://github.com/astral-sh/ruff/issues/18818

The change fixes the false negative described in the issue, but I want to ensure it doesn't break other scenarios.

I'm not sure why the original implementation only checked shadowed bindings with `only_binding()`, but I changed it to check any binding with `resolve_name()`. All existing tests pass, including my new test case, so the change appears to work correctly. 

However, I'd appreciate feedback on:
1. Why `only_binding()` was chosen initially
2. What edge cases my change might introduce
3. Whether this broader scope could cause false positives


## Test Plan

`cargo nextest run` and `cargo insta test`

---

_Comment by @dylwil3 on 2025-06-20 20:33_

My guess is that we are checking `only_binding` to be cautious about Ruff's ability to handle control flow.

I'll have to think of a less-contrived example, but I think your modification would trigger here, for example:

```python
if True:
    nums = []
else:
    nums = {1, 2, 3}
for num in nums:
    nums.add(num + 5)
```

But it's possible this is overly cautious. Let's see what the ecosystem report looks like.

---

_Comment by @ntBre on 2025-06-20 20:42_

I searched this for another PR earlier, and it seems like we have a pretty even split between `only_binding` and `resolve_name`, and I couldn't really tell which one should be used in the handful of cases I looked at.

I guess I kind of lean toward `resolve_name` in general, but `only_binding` is more conservative, as you said. Definitely curious to hear your thoughts, though! Maybe we can standardize or document when each one should be used, either way.

---

_Comment by @github-actions[bot] on 2025-06-20 20:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dylwil3 on 2025-06-20 20:53_

Ok, I think it's okay to use `resolve_name` here. But let's gate this behind preview since it might add diagnostics.

@LyricalToxic Can you add a method to `ruff_linter::preview` and gate your change behind that? Then you'll have to write add a test for it as well. Finally, add a little blurb to the documentation about the preview behavior (it doesn't have to be super technical).

---

_Comment by @IDrokin117 on 2025-06-20 21:08_

> @LyricalToxic Can you add a method to `ruff_linter::preview` and gate your change behind that? Then you'll have to write add a test for it as well. Finally, add a little blurb to the documentation about the preview behavior (it doesn't have to be super technical).

Sure, will do tomorrow

---

_Comment by @MichaReiser on 2025-06-23 11:50_

Can we add a test for:

```py
nums = []
for num in nums:
	nums = []
    nums.add(num + 5)
```

---

_Comment by @dylwil3 on 2025-06-23 13:03_

@MichaReiser did you want those to be empty sets instead of lists? (Lists don't have `.add`)

---

_Comment by @MichaReiser on 2025-06-23 13:05_

> @MichaReiser did you want those to be empty sets instead of lists? (Lists don't have `.add`)

Whoops, yes 😅

---

_Comment by @IDrokin117 on 2025-06-23 19:41_

@dylwil3 Sorry for the delay, it's done. I hope the changes align with your expectations. Please review it.

---

_Comment by @MichaReiser on 2025-06-23 20:34_

I want to double check this tomorrow because I found at least one new false positive in a similar pr and I don't think this is the right trade off

---

_Comment by @MichaReiser on 2025-06-24 06:22_

> My guess is that we are checking `only_binding` to be cautious about Ruff's ability to handle control flow.
> 
> I'll have to think of a less-contrived example, but I think your modification would trigger here, for example:
> 
> ```python
> if True:
>     nums = []
> else:
>     nums = {1, 2, 3}
> for num in nums:
>     nums.add(num + 5)
> ```
> 
> But it's possible this is overly cautious. Let's see what the ecosystem report looks like.

Indeed, this triggers now but didn't before. 

I don't think trading false-negatives with false-positives is a good trade here. Instead, we should look into if we can improve our inference by doing any of:

1. Resolve all visible bindings and check if they all resolve to a dict
2. Change `only_binding` to return the only binding that's unconditionally visible at this point (this might be hard?). 

Doing so would also fix https://github.com/astral-sh/ruff/pull/18861 and other places where we rely on type information without introducing new false positives. 

---

_Closed by @MichaReiser on 2025-06-24 06:22_

---
