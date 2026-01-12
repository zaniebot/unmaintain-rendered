```yaml
number: 9050
title: "ruff_python_formatter: add test for extraneous info string text"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
  - docstring
  - formatter
assignees: []
merged: true
base: main
head: ag/fmt/markdown-additional-info-string
created_at: 2023-12-07T21:56:38Z
updated_at: 2023-12-08T00:52:16Z
url: https://github.com/astral-sh/ruff/pull/9050
synced_at: 2026-01-10T23:40:55Z
```

# ruff_python_formatter: add test for extraneous info string text

---

_Pull request opened by @BurntSushi on 2023-12-07 21:56_

@ofek asked [about this][ref]. I did specifically add support for it, but neglected to add a test. This PR adds a test.

[ref]: https://github.com/astral-sh/ruff/pull/9030#issuecomment-1846054764

---

_Label `internal` added by @BurntSushi on 2023-12-07 21:56_

---

_Label `docstring` added by @BurntSushi on 2023-12-07 21:56_

---

_Label `formatter` added by @BurntSushi on 2023-12-07 21:56_

---

_Added to milestone `Formatter: Stable` by @BurntSushi on 2023-12-07 21:56_

---

_Review requested from @MichaReiser by @BurntSushi on 2023-12-07 21:56_

---

_Review requested from @charliermarsh by @BurntSushi on 2023-12-07 21:56_

---

_@charliermarsh approved on 2023-12-07 21:58_

Nice

---

_Comment by @ofek on 2023-12-07 22:07_

Awesome, thank you!

---

_Comment by @github-actions[bot] on 2023-12-07 22:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@MichaReiser approved on 2023-12-08 00:20_

TIL that you can have additional text after the language specifier

---

_Comment by @ofek on 2023-12-08 00:23_

Not in pure Markdown but extensions do support that frequently to do various things!

---

_Comment by @BurntSushi on 2023-12-08 00:51_

> Not in pure Markdown but extensions do support that frequently to do various things!

Assuming pure Markdown is CommonMark, the extra text after the opening fences is called the "info string" and it does indeed permit for pretty much anything to go there. There are even examples in the spec. See: https://spec.commonmark.org/0.30/#fenced-code-blocks

The spec doesn't attach any semantic value to any part of the "info string," it just specifies what is allowed there. For example, when using backticks for a fenced code block, the info string itself cannot contain backticks. But it can if you use tildes!

---

_Merged by @BurntSushi on 2023-12-08 00:52_

---

_Closed by @BurntSushi on 2023-12-08 00:52_

---

_Branch deleted on 2023-12-08 00:52_

---
