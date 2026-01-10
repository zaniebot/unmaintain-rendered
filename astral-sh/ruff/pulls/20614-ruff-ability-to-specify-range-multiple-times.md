```yaml
number: 20614
title: "[`ruff`] Ability to specify `--range` multiple times."
type: pull_request
state: open
author: cristian64
labels:
  - formatter
assignees: []
base: main
head: format_multiple_ranges
created_at: 2025-09-28T21:12:00Z
updated_at: 2025-10-30T18:59:38Z
url: https://github.com/astral-sh/ruff/pull/20614
synced_at: 2026-01-10T16:59:49Z
```

# [`ruff`] Ability to specify `--range` multiple times.

---

_Pull request opened by @cristian64 on 2025-09-28 21:12_

[`ruff`] Ability to specify `--range` multiple times.

## Summary

Text ranges that overlap (or are adjacent) will be collapsed. This is done after text ranges have been adjusted to their enclosing nodes in the tree. Formatting of each range is then performed starting from the bottom of the file, ensuring that the following range still applies to the lines that the user intended to format.

Implements #12800.

## Test Plan

- Pre-existing unit tests ensure no regression or unexpected behavior change in regular executions with a single `--range` argument.
- New unit tests have been added to cover the feature enhancement.

---

_@cristian64 reviewed on 2025-09-28 21:14_

---

_Review comment by @cristian64 on `crates/ruff/src/commands/format.rs`:298 on 2025-09-28 21:14_

Note for the reviewer: the only change in this block is `range` -> `ranges`; hiding whitespace changes in the diff viewer may improve readability of the diff.

---

_Comment by @github-actions[bot] on 2025-09-28 21:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:372 on 2025-09-29 06:57_

It should be fine to use the same code path for one and multiple ranges. The overhead should be minimal compared to the formatting itself.

---

_@MichaReiser requested changes on 2025-09-29 06:59_

Thank you for looking into this. 

Unfortunately, I don't think the way the ranges are collapsed is correct and this, instead, requires deeper integration into `format_range`. 

The issue is that `format_range` can expand the formatting range. E.g. for:

```py
a = (1 + 
	3 + 2)
```

A range `2:3` will be expanded to `1:3` because Ruff can only format entire statements (or clause headers), not sub-expressions. 



---

_Label `formatter` added by @MichaReiser on 2025-09-29 06:59_

---

_Comment by @cristian64 on 2025-09-29 07:12_

> Thank you for looking into this.
> 
> Unfortunately, I don't think the way the ranges are collapsed is correct and this, instead, requires deeper integration into `format_range`.
> 
> The issue is that `format_range` can expand the formatting range. E.g. for:
> 
> ```python
> a = (1 + 
> 	3 + 2)
> ```
> 
> A range `2:3` will be expanded to `1:3` because Ruff can only format entire statements (or clause headers), not sub-expressions.

I'm trying to digest why this would be a problem.

If the user provides ranges that overlap with these expanded ranges (which I think can be considered an edge case), the only drawback would be that Ruff would perform a redundant format operation, but the end result would be correct.

Would you have a more concrete example where things can go wrong? (And would we be happy to document the case and/or file a separate issue for it, while providing a solution for the general case?)

---

_Comment by @MichaReiser on 2025-09-29 07:17_

> Formatting of each range is performed starting from the bottom of the file, ensuring that the following range still applies to the lines that the user intended to format.

I think the assumption that ranges will not be invalidated because they're format from back to front is no longer correct if a range can be extended forward.

I think the right approach is:

* Expand every range and collapse overlapping ranges
* Find the minimal range that covers all ranges
* Expand that minimal range too
* Format the minimal range (avoids re-parsing the same file multiple times)
* Now replace the text of the expanded (and collapsed) ranges)

---

_@cristian64 reviewed on 2025-09-29 07:17_

---

_Review comment by @cristian64 on `crates/ruff/src/commands/format.rs`:372 on 2025-09-29 07:17_

I'm not that familiar with Rust, and the use of `mut` wasn't too comforting, so I went with the option that produced a smaller diff for the sake of the reviewer.

Happy to collapse the two cases if we are okay with the solution, pending discussion of the other point you raised.

---

_Comment by @cristian64 on 2025-09-29 07:23_

> > Formatting of each range is performed starting from the bottom of the file, ensuring that the following range still applies to the lines that the user intended to format.
> 
> I think the assumption that ranges will not be invalidated because they're format from back to front is no longer correct if a range can be extended forward.

If I understand correctly, the range would be invalidated, but it would sit on top of code that has just been formatted, so the format operation would be no-op, with no other side effect. (At least that was how I reasoned about this.)

---

_Comment by @MichaReiser on 2025-09-29 07:28_

I don't think this assumption is correct because you replace `unformatted` with the newly formatted content, invalidating any prevoius source range. 

What's the reaosnf or updating the unformatted code. Can't we keep using the same unformatted code and simply format different slices (in which case we should avoid reparsing the AST over and over again)

---

_Comment by @cristian64 on 2025-09-29 07:44_

> I don't think this assumption is correct because you replace `unformatted` with the newly formatted content, invalidating any prevoius source range.

They would be invalidated if a range was expanded, but even then that shouldn't be a problem as the invalidated range would now sit on code that has been formatted, and a second format operation is no-op.

> What's the reaosnf or updating the unformatted code. Can't we keep using the same unformatted code and simply format different slices (in which case we should avoid reparsing the AST over and over again)

The unformatted code needs to be updated due to the potential existence of expanded ranges. I see how performance-wise it'd be better to implement this at a lower level, but it's also less trivial.

---

_Comment by @MichaReiser on 2025-09-29 07:52_

> They would be invalidated if a range was expanded, but even then that shouldn't be a problem as the invalidated range would now sit on code that has been formatted, and a second format operation is no-op.

The range could now point between Unicode characters which results in a panic. Or the range now points passed the end of the source file which is an error.

> The unformatted code needs to be updated due to the potential existence of expanded ranges. I see how performance-wise it'd be better to implement this at a lower level, but it's also less trivial.

That's fair. However, performance is a crucial aspect of our tooling. 


---

_Comment by @cristian64 on 2025-09-29 08:01_

> The range could now point between Unicode characters which results in a panic. 

I thought the numbers in the range were to be treated as unicode units, meaning that this shouldn't be possible.

> Or the range now points passed the end of the source file which is an error.

This case should not be possible. The range would have been joined with the previous range, or the formatting of the previous range should have hit the error earlier. (Doing it from back to front guarantees correctness.)



---

_Comment by @MichaReiser on 2025-09-29 08:29_

Oh right, you convert from `TextRange` to `FormatRange` in each iteration. This might also protect against going past the file end. 

> This case should not be possible. The range would have been joined with the previous range, or the formatting of the previous range should have hit the error earlier. (Doing it from back to front guarantees correctness.)

It's possible on the byte range level but I suspect that `to_range` protects against going past the file range?

E.g. 

```py
a = (1 + 
    3 + 2)
b = (x)
```

with 2:5 to 2:6 (3) and 2:11 to 2:8

The ranges don't overlap, so they aren't merged. Formatting 2:7 to 2:8 results in 

```
a = 1 + 3 + 2
b = (x)
```

The range 2:5 to 2:6 now incorrectly selects the `(` of `b = (x)`


---

_Comment by @cristian64 on 2025-09-29 08:44_

I think there is a typo in `2:11` (should be `2:7`), but I see how it can lead to the formatting of unsolicited code blocks.

Joining of overlapping ranges must indeed be done after ranges have been potentially expanded.

---

_Comment by @cristian64 on 2025-09-29 18:00_

I've added a unit test to cover the discussed case (which should be failing now). I'll see if I can look into a more robust implementation in the upcoming days.

---

_Comment by @cristian64 on 2025-10-04 10:27_

I've made some progress on this. The new implementation collapses ranges _after_ they have been expanded. The unit test that was added earlier in the week is now passing, and another unit test with ranges that touch on various statements has been added.

Performance wise there should be room for improvement (avoid copies of the text; avoid parsing more than once), but the deeper in the code the harder it gets.

@MichaReiser How do you feel about this? Would this be good enough for a first iteration? Would you like to take it from here?

---

_Comment by @MichaReiser on 2025-10-04 11:34_

> @MichaReiser How do you feel about this? Would this be good enough for a first iteration? Would you like to take it from here?

Nice. I'll try to take a look at this sometime soon but it probably won't be in the next two weeks. I'm stretched extremely thin as we're working towards the ty beta. I'm sorry. Please ping me if I forget to get back to this.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/range.rs`:146 on 2025-10-30 14:06_

We should avoid parsing and extracting the comments here. Both are very expensive operations and we only want to perform them exactly one per file

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/range.rs`:191 on 2025-10-30 14:10_

I'm not sure if simply joining here is correct. `covering_node` handles a few special cases, e.g. only formatting the clause header and `slice_range` assumes on some of those assumptions too (I think). 

So this might be a bit trickier than just joining ranges together but I'd have to take a closer look myself.

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:372 on 2025-10-30 14:11_

I'd very much prefer to share as much of the code because we already have very low test coverage for range formatting.



---

_@MichaReiser reviewed on 2025-10-30 14:12_

Thanks. This looks better but I still don't feel very confident about the change. Have you looked at how prettier implements multi range formatting? Maybe there's something we could learn from them as our approach is very similar.

---

_Review comment by @cristian64 on `crates/ruff/src/commands/format.rs`:372 on 2025-10-30 18:49_

I was leaving the removal to the end, to keep the diff a bit easier to read until we would be happy with the rest of the changes. Would you rather like this removed _now_?

---

_Review comment by @cristian64 on `crates/ruff_python_formatter/src/range.rs`:146 on 2025-10-30 18:52_

Perhaps I'm unable to see how this can be skipped. It seems the other functions require this context to be provided.

---

_@cristian64 reviewed on 2025-10-30 18:59_

---
