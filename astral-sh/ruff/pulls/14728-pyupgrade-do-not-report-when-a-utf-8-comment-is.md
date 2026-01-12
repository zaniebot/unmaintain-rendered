```yaml
number: 14728
title: "[`pyupgrade`] Do not report when a UTF-8 comment is followed by a non-UTF-8 one (`UP009`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: UP009
created_at: 2024-12-02T13:06:45Z
updated_at: 2024-12-11T12:52:24Z
url: https://github.com/astral-sh/ruff/pull/14728
synced_at: 2026-01-12T15:55:48Z
```

# [`pyupgrade`] Do not report when a UTF-8 comment is followed by a non-UTF-8 one (`UP009`)

---

_@InSyncWithFoo_

## Summary

Resolves #14704.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-12-02 13:18_

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

_@dscorbett reviewed on 2024-12-02 14:15_

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_coding_comment.rs`:85 on 2024-12-02 14:15_

If this block removes both UTF-8 encoding declarations, what happens if there is a third, non-UTF-8 encoding declaration?
```python
# coding=utf-8
# coding=utf-8
# coding=ascii
```

I think it would be better to only offer the fix for `ranges_1`. If it is safe to delete `ranges_2` too, that can happen on the next iteration.

---

_@InSyncWithFoo reviewed on 2024-12-02 14:34_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_coding_comment.rs`:85 on 2024-12-02 14:34_

Just to be clear, should we:

* Report both but only suggest a fix for the first?
* Report only the first?
* Report both, marking the first fix safe but the second fix unsafe?
* Another way?

Currently this rule is marked as always fixable.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_coding_comment.rs`:131 on 2024-12-03 08:47_

Oh wow, this still uses the old API. We can address this in a follow up PR but I think we could easily replace this with `locator.slice(TextRange::up_to(line_range.start())` and then count the number of  lines by calling `locator.full_line_end` until it reaches the end of the range. We could add this as a method to `LineRanges`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_coding_comment.rs`:85 on 2024-12-03 08:54_

In that case, we shouldn't raise a diagnostic for the second encoding comment. 


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_coding_comment.rs`:74 on 2024-12-03 08:56_

It would be great if we can avoid collecting the comments here to avoid the allocation. I think doing so should also reduce the amount of code needed below.

```rust
let mut comments = comment_ranges.iter().take(...)...

let Some(first) = comments.next() else {
	return None;
}

if let Some(CodingComment::Other) = comments.next() {
	return None;
}

```



---

_@MichaReiser requested changes on 2024-12-03 08:57_

We should address the case when there are multiple coding comments (that might also be on lines other than the first two)

---

_Review comment by @MichaReiser on `crates/ruff_source_file/src/line_ranges.rs`:331 on 2024-12-04 08:10_

Nit: i'd change the method to take a text range instead. It makes it more flexible and it also avoids that the function never terminates if `offset` == `len(text)`
```suggestion
    /// Returns the zero-based index of the line containing `offset`.
    ///
    /// ## Examples
    ///
    /// ```
    /// # use ruff_text_size::{Ranged, TextRange, TextSize};
    /// # use ruff_source_file::LineRanges;
    ///
    /// let text = "First line\nsecond line\r\nthird line";
    ///
    /// assert_eq!(text.count_lines_until(TextSize::from(5)), 0);
    /// assert_eq!(text.count_lines_until(TextSize::from(23)), 1);
    /// assert_eq!(text.count_lines_until(TextSize::from(24)), 2);
    /// ```
    ///
    /// ## Panics
    /// If `offset` is out of bounds.
    fn count_lines(&self, range: TextRange) -> u32 {
        let mut count = 0;
        let mut offset = range.start();

				loop {
					let line_end = self.full_line_end(offset);
					if line_end < range.end() {
						count += 1;
						offset = line_end;
				} else {
					break count;
				}
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP009_utf8_utf8_other.py`:1 on 2024-12-04 08:16_

Nice tests.

Could we add a test for

```
print("the following is not a coding comment")
# -*- coding: utf8 -*-
```

The PEP is surprisingly not specific about whether this coding statement is valid or not (I'd assume it isn't but the PEP doesn't mention anything about a statement before the coding block so maybe it is?)

---

_@MichaReiser reviewed on 2024-12-04 08:20_

Wow, thanks for implementing the `count_lines` method! 

There's one more edge case that needs handling

```
#! /usr/bin/env/python
# -*- coding: utf-8 -*-
# -*- coding: ascii -*-
```

The second comment gets flagged as unnecessary, which is incorrect because removing it would change the encoding to ascii

---

_@dscorbett reviewed on 2024-12-04 13:13_

---

_Review comment by @dscorbett on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP009_utf8_utf8_other.py`:1 on 2024-12-04 13:13_

[Python’s documentation](https://docs.python.org/3/reference/lexical_analysis.html#encoding-declarations) says “The encoding declaration must appear on a line of its own. If it is the second line, the first line must also be a comment-only line.” so that is just a normal comment, not an encoding declaration.

---

_Comment by @InSyncWithFoo on 2024-12-04 17:37_

Cases to handle, as a table:

|   1   |   2   |   3   | E |
|:-----:|:-----:|:-----:|:-:|
|   -   |   -   |   -   |   |
|   -   |   -   | UTF-8 |   |
|   -   |   -   | Other |   |
|   -   | UTF-8 |   -   | x |
|   -   | UTF-8 | UTF-8 | x |
|   -   | UTF-8 | Other |   |
|   -   | Other |   -   |   |
|   -   | Other | UTF-8 |   |
|   -   | Other | Other |   |
| UTF-8 |   -   |   -   | x |
| UTF-8 |   -   | UTF-8 | x |
| UTF-8 |   -   | Other | x |
| UTF-8 | UTF-8 |   -   | x |
| UTF-8 | UTF-8 | UTF-8 | x |
| UTF-8 | UTF-8 | Other | x |
| UTF-8 | Other |   -   |   |
| UTF-8 | Other | UTF-8 |   |
| UTF-8 | Other | Other |   |
| Other |   -   |   -   |   |
| Other |   -   | UTF-8 |   |
| Other |   -   | Other |   |
| Other | UTF-8 |   -   |   |
| Other | UTF-8 | UTF-8 |   |
| Other | UTF-8 | Other |   |
| Other | Other |   -   |   |
| Other | Other | UTF-8 |   |
| Other | Other | Other |   |


---

_@InSyncWithFoo reviewed on 2024-12-04 17:50_

---

_Review comment by @InSyncWithFoo on `crates/ruff_source_file/src/line_ranges.rs`:331 on 2024-12-04 17:50_

Fixed the infinite loop bug, but I think this does something entirely different: It counts the number of lines `range` spans rather than the index of the line `offset` is placed within.

---

_Comment by @MichaReiser on 2024-12-09 15:55_

@InSyncWithFoo what's the status of this PR? Is it ready for re-review?

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP009_utf8_utf8_other.py`:1 on 2024-12-09 15:56_

Thanks @dscorbett  for the link. That's what I expected. Let's add a test if there isn't one already:

```
# -*- coding: utf8 -*-
print("the following is not a coding comment")
# -*- coding: ascii -*-
```

---

_@MichaReiser reviewed on 2024-12-09 15:56_

---

_Comment by @InSyncWithFoo on 2024-12-09 16:39_

@MichaReiser It is. Please do.

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP009_utf8_utf8_other.py`:1 on 2024-12-09 17:53_

Added.

---

_@InSyncWithFoo reviewed on 2024-12-09 17:53_

---

_@MichaReiser reviewed on 2024-12-10 08:01_

---

_Review comment by @MichaReiser on `crates/ruff_source_file/src/line_ranges.rs`:331 on 2024-12-10 08:01_

Yes, it is different but `count_lines_until` can be expressed by `count_lines(TextRange::up_to(offset))` 

That's why I'd prefer to only expose the more generic solution because it allows solving more use cacses.

---

_@MichaReiser requested changes on 2024-12-10 08:06_

Thanks for addressing the feedback.

The rule currently incorrectly flags the following comments:

```
x = 10
# -*- coding: utf-8 -*-
# -*- coding: utf-8 -*-
```

```
x = 10
# -*- coding: utf-8 -*-
```

I think we need to make the `comment_ranges` iterator more stateful. It should never return `Some` after it has seen any non-comment. It might be worth implementing a custom iterator and tracking some internal state.



---

_Label `rule` added by @MichaReiser on 2024-12-11 09:28_

---

_Label `bug` added by @MichaReiser on 2024-12-11 09:28_

---

_Merged by @MichaReiser on 2024-12-11 10:30_

---

_Closed by @MichaReiser on 2024-12-11 10:30_

---

_Branch deleted on 2024-12-11 12:52_

---
