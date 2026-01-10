---
number: 7061
title: "Formatter: Add end of line configuration option"
type: issue
state: closed
author: MichaReiser
labels:
  - cli
  - formatter
assignees: []
created_at: 2023-09-02T07:53:49Z
updated_at: 2024-12-03T03:52:25Z
url: https://github.com/astral-sh/ruff/issues/7061
synced_at: 2026-01-10T01:22:46Z
---

# Formatter: Add end of line configuration option

---

_Issue opened by @MichaReiser on 2023-09-02 07:53_

Add a `end-of-line` configuration option similar to Prettier and editorconfig that allows configuring the preferred line ending. The option supports the following value:

* lf [Default]
* crlf
* cr
* Auto

Making `Lf` the default over `Auto` is to avoid accidentally mixing line endings in a project [see](https://prettier.io/docs/en/options.html#end-of-line).

---

_Label `formatter` added by @MichaReiser on 2023-09-02 07:53_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-02 07:53_

---

_Label `cli` added by @MichaReiser on 2023-09-02 07:53_

---

_Referenced in [astral-sh/ruff#7054](../../astral-sh/ruff/pulls/7054.md) on 2023-09-02 08:05_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-09-15 12:47_

---

_Referenced in [astral-sh/ruff#7566](../../astral-sh/ruff/pulls/7566.md) on 2023-09-21 11:33_

---

_Closed by @MichaReiser on 2023-09-22 15:47_

---

_Comment by @jet-c-21 on 2024-12-03 02:22_

does this feature online right now? how can I set LF as default in my `pyproject.toml`?

---

_Comment by @dhruvmanila on 2024-12-03 03:52_

@jet-c-21 https://docs.astral.sh/ruff/settings/#format_line-ending

---
