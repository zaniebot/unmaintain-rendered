---
number: 2766
title: Link relevant settings in rule documentation
type: issue
state: closed
author: sbrugman
labels:
  - documentation
assignees: []
created_at: 2023-02-11T15:33:03Z
updated_at: 2023-02-12T18:19:13Z
url: https://github.com/astral-sh/ruff/issues/2766
synced_at: 2026-01-10T01:22:41Z
---

# Link relevant settings in rule documentation

---

_Issue opened by @sbrugman on 2023-02-11 15:33_

The individual rule documentation pages should reflect which settings are available that impact that rule. We could rely on the developers updating the documentation description with new rules and settings, but this is error prone. 

A dedicated section could help structure the related rules (did not find a good example in pylint etc.)

A more engineering heavy option is to keep track of a mapping between rules and options. (cc: @not-my-profile) This approach allows for automatic link generation, and linking from rule -> setting and vice versa.

Example:
- For [relative imports](https://beta.ruff.rs/docs/rules/relative-imports/) (link to strictness option seems broken, should be [this](https://github.com/charliermarsh/ruff#ban-relative-imports))
- For [unconventional-import-alias](https://beta.ruff.rs/docs/rules/unconventional-import-alias/), we have not linked the [relevant settings](https://beta.ruff.rs/docs/settings/#flake8-import-conventions) yet.

No strong preference, just putting the idea on the table.

---

_Comment by @charliermarsh on 2023-02-11 17:08_

My preference for now is to do it manually 😅 

---

_Label `documentation` added by @charliermarsh on 2023-02-11 17:08_

---

_Referenced in [astral-sh/ruff#2776](../../astral-sh/ruff/pulls/2776.md) on 2023-02-11 18:32_

---

_Comment by @not-my-profile on 2023-02-11 18:37_

I know a nice way to do this ... I'll implement it after #2775 has been merged, since the refactors in that PR make implementing what I have in mind easier.

---

_Comment by @charliermarsh on 2023-02-11 18:39_

Awesome :)

---

_Referenced in [astral-sh/ruff#2777](../../astral-sh/ruff/pulls/2777.md) on 2023-02-11 19:01_

---

_Closed by @charliermarsh on 2023-02-12 18:19_

---
