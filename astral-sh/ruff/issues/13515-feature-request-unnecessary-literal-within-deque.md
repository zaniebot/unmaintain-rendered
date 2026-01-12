```yaml
number: 13515
title: "Feature request: unnecessary-literal-within-deque-call (C409/C418 but for deque)"
type: issue
state: closed
author: Avasam
labels:
  - rule
assignees: []
created_at: 2024-09-25T19:47:19Z
updated_at: 2025-04-29T01:22:16Z
url: https://github.com/astral-sh/ruff/issues/13515
synced_at: 2026-01-12T15:54:53Z
```

# Feature request: unnecessary-literal-within-deque-call (C409/C418 but for deque)

---

_@Avasam_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Today I found the following code:
`self.frame_buffer = deque([], self.frame_buffer_size)`

Which, to my understanding, should be the same as
`self.frame_buffer = deque(maxlen=self.frame_buffer_size)`
(but w/o an extra empty literal, be it a list, tuple, set, etc)

Same with `x = deque([])`/`x = deque(())` --> `x = deque()`

There may be more collection types for which this idea applies.

I think this could be enforced by a linting rule.


---

_Label `rule` added by @charliermarsh on 2024-09-25 22:54_

---

_Comment by @zanieb on 2024-09-26 01:55_

This seems reasonable to me. It'd be nice to have a single rule for it.

---

_Comment by @MichaReiser on 2024-09-26 06:17_

To sum up what I understand. The rule would test for `deque` calls where the first argument is an empty list or tuple literal because that's redundant

---

_Comment by @Avasam on 2024-09-26 13:15_

> To sum up what I understand. The rule would test for `deque` calls where the first argument is an empty list or tuple literal because that's redundant

Exactly. Actually any empty iterable if we really wanna be thorough.
But given there's already rules to cover replacing empty iterable calls by literals: At least any empty literal (`deque({})` is probably not common, but imo may as well cover it).

---

_Comment by @Avasam on 2025-04-29 01:20_

Added in `0.8.6` by #15104
[unnecessary-empty-iterable-within-deque-call (RUF037)](https://docs.astral.sh/ruff/rules/unnecessary-empty-iterable-within-deque-call/#unnecessary-empty-iterable-within-deque-call-ruf037)

---

_Closed by @Avasam on 2025-04-29 01:20_

---
