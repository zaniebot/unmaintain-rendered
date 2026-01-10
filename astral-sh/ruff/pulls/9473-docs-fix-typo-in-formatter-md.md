```yaml
number: 9473
title: "docs: fix typo in formatter.md"
type: pull_request
state: merged
author: rsyring
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-01-11T17:29:56Z
updated_at: 2024-01-11T17:59:01Z
url: https://github.com/astral-sh/ruff/pull/9473
synced_at: 2026-01-10T22:57:09Z
```

# docs: fix typo in formatter.md

---

_Pull request opened by @rsyring on 2024-01-11 17:29_

I believe there was a typo in the formatter docs and I've attempted to fix it according to what I think was originally intended.

---

_Comment by @charliermarsh on 2024-01-11 17:34_

Thank you! So this isn't exactly a _typo_ since it was intentional, but it's clearly confusing enough that we should change it. We use "trivia" to refer to whitespace, comments, parentheses, newlines -- the stuff _around_ the code.

Maybe like: "outside of these whitespace and quote-related settings."? Wdyt?

---

_Label `documentation` added by @charliermarsh on 2024-01-11 17:34_

---

_Comment by @rsyring on 2024-01-11 17:39_

I wondered.  It seemed like an interesting typo to make.  :)

Since this sentence is already in a section that is explicitly discussing the deviations, i.e. "The Ruff Formatter exposes a small set of configuration options...", you could just make that final sentence a bit more concise.

> Given the focus on Black compatibility (and unlike formatters like [YAPF](https://github.com/google/yapf)), Ruff does not currently expose any other configuration options.

Another option would be to use a `<abbr>` around "trivia" and just define it.  Although I'm not sure if the doc generator supports that.

Or, if the docs support footnotes, you could footnote "trivia" with an explanation.

---

_Comment by @charliermarsh on 2024-01-11 17:44_

> Given the focus on Black compatibility (and unlike formatters like [YAPF](https://github.com/google/yapf)), Ruff does not currently expose any other configuration options.

This sounds good to me!

---

_Comment by @rsyring on 2024-01-11 17:55_

PR updated.

---

_@charliermarsh approved on 2024-01-11 17:58_

Thanks!

---

_Merged by @charliermarsh on 2024-01-11 17:59_

---

_Closed by @charliermarsh on 2024-01-11 17:59_

---
