---
number: 9436
title: formatter still in beta?
type: issue
state: closed
author: DimitriPapadopoulos
labels:
  - question
assignees: []
created_at: 2024-01-08T09:51:52Z
updated_at: 2024-01-08T12:45:53Z
url: https://github.com/astral-sh/ruff/issues/9436
synced_at: 2026-01-07T13:12:15-06:00
---

# formatter still in beta?

---

_Issue opened by @DimitriPapadopoulos on 2024-01-08 09:51_

[The Ruff Formatter](https://docs.astral.sh/ruff/formatter/) documentation starts with:
> The Ruff formatter is available as a [production-ready Beta](https://astral.sh/blog/the-ruff-formatter) as of Ruff v0.1.2.

[The Ruff Linter](https://docs.astral.sh/ruff/linter/) is not referred to as beta despite the current version [v.0.1.11](https://github.com/astral-sh/ruff/releases/tag/v0.1.11) starting with 0.

Is the formatter still beta? What about the linter? Isn't it time to have consistent descriptions for the linter and the formatter?

---

_Label `question` added by @MichaReiser on 2024-01-08 10:35_

---

_Comment by @MichaReiser on 2024-01-08 10:36_

There are a few features that we want to implement before calling the formatter as stable, but you can consider it stable if you don't need those. You can track the remaining formatter work in the [Stable formatter milestone](https://github.com/astral-sh/ruff/issues?q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc+milestone%3A%22Formatter%3A+Stable%22)

---

_Closed by @DimitriPapadopoulos on 2024-01-08 12:45_

---
