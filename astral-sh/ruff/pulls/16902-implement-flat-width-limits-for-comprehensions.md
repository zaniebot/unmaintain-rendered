```yaml
number: 16902
title: Implement flat width limits for comprehensions
type: pull_request
state: closed
author: aronhoff
labels:
  - formatter
  - needs-decision
assignees: []
base: main
head: main
created_at: 2025-03-21T17:41:22Z
updated_at: 2025-03-31T09:24:07Z
url: https://github.com/astral-sh/ruff/pull/16902
synced_at: 2026-01-10T19:40:36Z
```

# Implement flat width limits for comprehensions

---

_Pull request opened by @aronhoff on 2025-03-21 17:41_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Currently ruff formats comprehension expressions on a single line if they fit. This can squish a lot of complexity on the same line, making it harder to comprehend. There is also no way to force the comprehension to be expanded, like magic trailing commas for function calls. There has been a discussion of this in #11753.

One of the most viable suggestions in that issue (in my eyes at least) was to configure a specific width limit for the flat form of these expressions.
- If the expression fits in this width limit, it can be printed on one line (subject to all other usual limitations) .
- If the expression exceeds this limit, it is printed expanded. The specific limit no longer applies.

This pull request is a proof of concept implementation. I wanted to gather some feedback before finishing it up. Currently, flat width limits are implemented for:
- list comprehensions
- dict comprehensions
- set comprehensions
- generator expressions
- ternary expressions (inline ifs)

Please let me know if you have any feedback, for the approach or for the code! I am new to this codebase and to rust in general.

### Implementation details

The basis of the change is introducing a tag in `ruff_formatter`, `StartWidthLimitedBlock`/`EndWidthLimitedBlock` that can put an alternative line width limit on the formatter stack. This alternative line width limit is effectively `min(current_width_limit, current_cursor_position + expression_flat_width_limit)`. The alternative limit is removed from the stack at the end of the tag.

Beyond that, in `ruff_python_formatter`, this tag is only applied if the expression in question will end up being printed flat. If the expression ends up being printed in the expanded form, the tags are not emitted.

Out of scope for this PR, but I expect the formatting tag introduced here could be useful for simplifying the formatting of docstring code blocks.

## Test Plan

I added a few snapshot tests to verify that the approach works. This would likely need to be expanded.

---

_Review requested from @MichaReiser by @aronhoff on 2025-03-21 17:41_

---

_Label `formatter` added by @MichaReiser on 2025-03-21 17:46_

---

_Label `needs-decision` added by @MichaReiser on 2025-03-21 17:46_

---

_Comment by @MichaReiser on 2025-03-21 17:50_

Thanks for this PR. This looks very impressive. 

However, adding all those options isn't well in line with the formatter's philosophy. I'd prefer a configuration-less approach but uses some other heuristics to decide when and how to split generators. 

---

_Comment by @github-actions[bot] on 2025-03-21 17:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Closed by @MichaReiser on 2025-03-31 09:24_

---
