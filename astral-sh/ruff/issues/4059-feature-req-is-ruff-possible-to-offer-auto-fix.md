```yaml
number: 4059
title: "[feature req] is ruff possible to offer auto-fix hint for auto-import?"
type: issue
state: closed
author: Shane-XB-Qian
labels:
  - question
assignees: []
created_at: 2023-04-21T14:30:35Z
updated_at: 2023-04-23T19:47:29Z
url: https://github.com/astral-sh/ruff/issues/4059
synced_at: 2026-01-12T15:54:44Z
```

# [feature req] is ruff possible to offer auto-fix hint for auto-import?

---

_@Shane-XB-Qian_

i am not very sure how ruff worked, but since pylsp-ruff based on this/ruff,
per the discussion https://github.com/python-lsp/python-lsp-ruff/issues/29 here,
would you mind consider adding a feature to make it work for auto-fix for auto-import?

---

_Comment by @dhruvmanila on 2023-04-21 15:42_

Looking at the linked issue, do you mean to suggest that `ruff` offers a way to auto-import symbols which are missing in the current namespace / module?

Language servers do provide this ability on _selecting_ a completion item i.e., to auto-import the symbol if not present.

---

_Comment by @Shane-XB-Qian on 2023-04-21 16:28_

yes.
make it work via code action, or TextadditionalEdit, or compl item, etc depended on lsp server preferred.
// normally i think better be auto import.
// then isort can tidy it.

but now wish ruff gave a way to Pylsp-ruff firstly to make it be possible to implement it.

--
shane.xb.qian


---

_Comment by @charliermarsh on 2023-04-23 19:47_

Hmm, we might support this eventually, but it's a little out-of-scope right now. I recommend using Ruff alongside another LSP to provide some of those more general language server features, since Ruff's LSP right now is mostly focused on enforcing and facilitating Ruff-related rules.

---

_Closed by @charliermarsh on 2023-04-23 19:47_

---

_Label `question` added by @charliermarsh on 2023-04-23 19:47_

---
