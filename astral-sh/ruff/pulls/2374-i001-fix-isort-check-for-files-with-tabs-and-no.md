```yaml
number: 2374
title: "[`I001`] fix isort check for files with tabs and no indented blocks"
type: pull_request
state: merged
author: sciyoshi
labels:
  - isort
assignees: []
merged: true
base: main
head: isort-tabs
created_at: 2023-01-31T02:47:58Z
updated_at: 2023-01-31T20:37:31Z
url: https://github.com/astral-sh/ruff/pull/2374
synced_at: 2026-01-12T04:52:00Z
```

# [`I001`] fix isort check for files with tabs and no indented blocks

---

_Pull request opened by @sciyoshi on 2023-01-31 02:47_

This is a followup to #2361. The isort check still had an issue in a rather specific case: files with a multiline import, indented with tabs, and not containing any indented blocks.

The root cause is this: [`Stylist`'s indentation detection](https://github.com/charliermarsh/ruff/blob/ad8693e3deddbd8e178546dad7be5759ebf1a095/src/source_code/stylist.rs#L163-L172) works by finding `Indent` tokens to determine the type of indentation used by a file. This works for indented code blocks (loops/classes/functions/etc) but does not work for multiline values, so falls back to 4 spaces if the file doesn't contain code blocks.

I considered a few possible solutions:

1. Fix `detect_indentation` to avoid tokenizing and instead use some other heuristic to determine indentation. This would have the benefit of working in other places where this is potentially an issue, but would still fail if the file doesn't contain any indentation at all, and would need to fall back to option 2 anyways.
2. Add an option for specifying the default indentation in Ruff's config. I think this would confusing, since it wouldn't affect the detection behavior and only operate as a fallback, has no other current application and would probably end up being overloaded for other things.
3. Relax the isort check by comparing the expected and actual code's lexed tokens. This would require an additional lexing step.
4. Relax the isort check by comparing the expected and actual code modulo whitespace at the start of lines.

This PR does approach 4, which in addition to being the simplest option, has the (expected, although I didn't benchmark) added benefit of improved performance, since the check no longer needs to do two allocations for the two `dedent` calls. I also believe that the check is still correct enough for all practical purposes.

---

_Review comment by @sciyoshi on `src/rules/isort/rules/organize_imports.rs`:49 on 2023-01-31 02:49_

This helper might be useful elsewhere, although I wasn't sure what the best place for it would be.

---

_@sciyoshi reviewed on 2023-01-31 02:49_

---

_Comment by @sciyoshi on 2023-01-31 02:56_

A contrived example of code that will now pass the isort check but didn't before:

```
from module import (
 a,
  b,
   c,
    d,
)
```

I think this is acceptable and probably better handled by an autoformatter anyways.

---

_Review requested from @charliermarsh by @charliermarsh on 2023-01-31 03:02_

---

_Label `isort` added by @charliermarsh on 2023-01-31 03:02_

---

_@charliermarsh reviewed on 2023-01-31 03:20_

---

_Review comment by @charliermarsh on `src/rules/isort/rules/organize_imports.rs`:116 on 2023-01-31 03:20_

(IIRC, we used to compare `indent(&expected, indentation)` to `actual`, but `indent` and `dedent` don't respect `stylist.line_ending`, and so files with CLRF newlines would always fail the comparison.)

---

_Comment by @charliermarsh on 2023-01-31 03:22_

I agree with the reasoning you've prevented here. In theory, could we also check if `actual` has _consistent_ indentation? (Assuming that, if the indentation is inconsistent, it should always be replaced?)

---

_Comment by @sciyoshi on 2023-01-31 05:46_

> In theory, could we also check if `actual` has _consistent_ indentation?

If I'm understanding correctly, yes - definitely possible. [Here's a change that implements that](https://github.com/sciyoshi/ruff/compare/isort-tabs...sciyoshi:ruff:isort-consistent-indent) - although it does require the `dedent` call and performs allocations for getting prefixes (although there's probably a zero-allocation way to do that.) I'm not convinced it's necessary, but I can definitely include that in this PR if you'd like.

---

_Merged by @charliermarsh on 2023-01-31 12:18_

---

_Closed by @charliermarsh on 2023-01-31 12:18_

---

_Comment by @charliermarsh on 2023-01-31 12:19_

Let's run with this. Thank you for fixing.

As another optional tweak to the heuristic: could we _only_ ignore whitespace if we can't infer any indentation? That is, if we're falling back to the default?

---

_Comment by @sciyoshi on 2023-01-31 20:37_

@charliermarsh thanks! And yes, that's possible as well. You can see [the changes for that here](https://github.com/charliermarsh/ruff/compare/main...sciyoshi:isort-detection-failed-fallback) - I can open a PR if you think that's a worthwhile improvement.

---

_Branch deleted on 2023-01-31 20:37_

---
