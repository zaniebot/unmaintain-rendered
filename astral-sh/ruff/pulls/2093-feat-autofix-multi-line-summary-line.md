```yaml
number: 2093
title: "feat: autofix `multi-line-summary-*-line`"
type: pull_request
state: merged
author: scop
labels: []
assignees: []
merged: true
base: main
head: feat/d21x-autofix
created_at: 2023-01-22T21:52:23Z
updated_at: 2023-01-25T19:59:30Z
url: https://github.com/astral-sh/ruff/pull/2093
synced_at: 2026-01-12T04:52:00Z
```

# feat: autofix `multi-line-summary-*-line`

---

_Pull request opened by @scop on 2023-01-22 21:52_

_No description provided._

---

_Review comment by @scop on `src/rules/pydocstyle/rules/multi_line_summary_start.rs`:67 on 2023-01-22 22:01_

This is to protect against breakage with https://github.com/charliermarsh/ruff/blob/ebfdefd110e83bcd1d101ee5a424fc6a82e020b5/resources/test/fixtures/pydocstyle/D.py#L438
The docstring's `indentation` ends up as `def docstring_start_in_same_line(): ` which we obviously cannot insert as indentation after the newline we'd insert. I guess it's fine to just skip these cases for now.

If that's acceptable, is this an ok way to conditionalize the fix, or is there some other preferred way? The thing is marked as an `AlwaysAutofixableViolation` but with this, it would not be _always_ autofixable. The CLI output says "potentially fixable" for these, and there doesn't seem to be a concept of codified conditional fixability, so I guess it's ok.

---

_@scop reviewed on 2023-01-22 22:01_

---

_@scop reviewed on 2023-01-22 22:24_

---

_Review comment by @scop on `src/rules/pydocstyle/rules/multi_line_summary_start.rs`:46 on 2023-01-22 22:24_

This could cause exceeding `line-length` on the start row. Should that be taken into account somehow, e.g. skipping the fix if it'd happen?

---

_Review comment by @charliermarsh on `src/rules/pydocstyle/rules/multi_line_summary_start.rs`:67 on 2023-01-23 02:50_

A few things:

1. We do have a pattern for a rule that's conditionally fixable. If you grep for `autofix_title_formatter`, you'll see some examples of that :)
2. We probably _could_ fix those cases, or at least try, although I'd still merge this even if we don't. `checker.stylist.indentation()` will contain the inferred indentation for the file... so you could look at the current indentation of the `def`, then add `checker.stylist.indentation()` to it. That could be wrong in some cases, like files with mixed indentation, but _should_ otherwise be correct. I'll leave it to you whether you want to enable that behavior :)

---

_@charliermarsh reviewed on 2023-01-23 02:50_

---

_@charliermarsh reviewed on 2023-01-23 02:52_

---

_Review comment by @charliermarsh on `src/rules/pydocstyle/rules/multi_line_summary_start.rs`:46 on 2023-01-23 02:52_

We do sometimes check that the generated line is `< checker.settings.line_length`. But mostly for opinionated checks, like proposed simplifications. In this case, I think it's okay to apply the fix anyway. It's consistent with how we handle other docstring fixes.

---

_Review comment by @scop on `src/rules/pydocstyle/rules/multi_line_summary_start.rs`:67 on 2023-01-23 20:12_

Thanks for the advice! I opted to go for 2), mostly for practice purposes, 6a7f2f15d2713106f4ee015f71e886977b35ee5b has it. There are still some cases in which we skip the fix (parent definition is not function/class, or their indentation contains something besides whitespace), but I'm not sure if they can actually happen, or are more on the theoretical side.

The implementation reads a bit clumsy to me now, but that's what my Rust-fu is up to at time of writing :|

---

_@scop reviewed on 2023-01-23 20:12_

---

_Review comment by @charliermarsh on `src/rules/pydocstyle/rules/multi_line_summary_start.rs`:67 on 2023-01-23 21:04_

Awesome, I'll review this today :)

---

_@charliermarsh reviewed on 2023-01-23 21:04_

---

_Merged by @charliermarsh on 2023-01-24 13:17_

---

_Closed by @charliermarsh on 2023-01-24 13:17_

---

_@charliermarsh reviewed on 2023-01-24 13:17_

---

_Review comment by @charliermarsh on `src/rules/pydocstyle/rules/multi_line_summary_start.rs`:99 on 2023-01-24 13:17_

One minor code change -- need to be sensitive to the current line-ending style (could be `\r\n`, etc.).

---

_@scop reviewed on 2023-01-25 19:55_

---

_Review comment by @scop on `src/rules/pydocstyle/rules/multi_line_summary_start.rs`:99 on 2023-01-25 19:55_

Ah, I thought about that and remember grepping for what should be done about it, but failed to follow through. Thanks for taking care of it!

There's a literal `\n` at https://github.com/charliermarsh/ruff/blob/662e29b1ce9dd99e1456aae6f5817335ca1a7562/src/autofix/helpers.rs#L182, too. Don't know if it's an issue, but thought I'd mention now that I noticed when re-grepping.

---

_Branch deleted on 2023-01-25 19:55_

---

_Review comment by @charliermarsh on `src/rules/pydocstyle/rules/multi_line_summary_start.rs`:99 on 2023-01-25 19:59_

Oh, thanks, I'll fix that real quick.

---

_@charliermarsh reviewed on 2023-01-25 19:59_

---
