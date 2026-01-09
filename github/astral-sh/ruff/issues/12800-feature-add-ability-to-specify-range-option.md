---
number: 12800
title: "[feature] Add ability to specify `--range` option multiple times"
type: issue
state: open
author: afontenot
labels:
  - formatter
  - wish
  - needs-design
assignees: []
created_at: 2024-08-11T06:45:11Z
updated_at: 2024-09-19T12:18:12Z
url: https://github.com/astral-sh/ruff/issues/12800
synced_at: 2026-01-07T13:12:15-06:00
---

# [feature] Add ability to specify `--range` option multiple times

---

_Issue opened by @afontenot on 2024-08-11 06:45_

This feature would increase feature parity with `black`, which supports multiply specifying the equivalent `--line-ranges` option.

Accepting multiple ranges is extremely useful for programmatically checking / modifying the format of the *changed* portions of a file when changes occur. For example, on some code bases a style guide has not been correctly adhered to historically, but project maintainers want to ensure that all *new* work adheres to `ruff format` checks.

Some discussion of this happened in the original `--range` pull request https://github.com/astral-sh/ruff/pull/9733, which I'll copy here:

> The CLI only supports a single range. We can explore supporting multiple ranges in the future. The main challenge is that overlapping ranges invalidate the offset of whichever range gets formatted last (starting from the back helps but doesn't prevent it). This is especially a problem if the formatter has to extend the formatted range.

While I do agree that this adds some complexity, I think it's a valuable feature and I didn't see an open issue for it, so I decided to file one to hopefully push this forward.

---

_Label `cli` added by @AlexWaygood on 2024-08-11 12:36_

---

_Label `formatter` added by @AlexWaygood on 2024-08-11 12:36_

---

_Comment by @MichaReiser on 2024-08-11 15:12_

Thanks for opening this issue and doing the extra work of finding the PR where it was added. 

Could you tell us a bit more about your workflow? It seems you're already calling ruff in an automated way. 

---

_Comment by @afontenot on 2024-08-11 21:43_

> Could you tell us a bit more about your workflow?

Sure! I don't have this in use on any large projects (yet), but I work on some Python projects that don't have an established code style currently. I've written a small program that calls `git` to extract the precise line numbers that have been changed, and turns these into ranges that can be passed into either `black` or `ruff format`.

To make sure my contributions to these projects follow a consistent style (I use the black style on all my personal stuff), I set up a pre-commit hook that runs this tool in a `--check` form. I can also pass `--diff` to it to see code style changes suggested by `black` or `ruff` or just accept the suggested changes. This can't be done without iteratively changing one section of code at a time in `ruff`. I think project maintainers who want to make sure PRs pass a style check without reformatting their whole code base might be interested in adding a tool like this to their test suite.

Some editors allow you to pass selections to the formatter, so that's a partial workaround for this issue. However, this doesn't allow a pre-commit check, and the editor I'm currently using the most (Zed) does not support this.


---

_Comment by @dhruvmanila on 2024-08-12 05:08_

This will also be useful in implementing the upcoming (3.18) LSP feature `textDocument/rangesFormatting` which allow providing multiple ranges for a formatting request (https://microsoft.github.io/language-server-protocol/specifications/lsp/3.18/specification/#textDocument_rangeFormatting).

---

_Label `cli` removed by @MichaReiser on 2024-08-12 06:38_

---

_Label `wish` added by @MichaReiser on 2024-08-12 06:38_

---

_Referenced in [astral-sh/ruff-pre-commit#94](../../astral-sh/ruff-pre-commit/issues/94.md) on 2024-08-13 12:54_

---

_Label `needs-design` added by @MichaReiser on 2024-08-17 14:35_

---

_Comment by @okuuva on 2024-09-19 12:11_

Just to add a related note: the use case @afontenot described is the reason [`darker`](https://github.com/akaihola/darker) exists. Support for `ruff format` is [incoming](https://github.com/akaihola/darker/issues/521).

Full disclosure: I'm a `darker` contributor. Always feels like an icky humble brag but better to be open about it I guess.

---

_Referenced in [root-project/root#18171](../../root-project/root/pulls/18171.md) on 2025-03-27 15:51_

---

_Referenced in [astral-sh/ruff#17679](../../astral-sh/ruff/issues/17679.md) on 2025-04-29 10:16_

---

_Referenced in [astral-sh/ruff#20614](../../astral-sh/ruff/pulls/20614.md) on 2025-09-28 21:12_

---
