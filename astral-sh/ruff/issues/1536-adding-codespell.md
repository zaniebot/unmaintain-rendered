```yaml
number: 1536
title: Adding Codespell
type: issue
state: closed
author: colin99d
labels: []
assignees: []
created_at: 2023-01-01T19:49:05Z
updated_at: 2023-01-02T20:58:00Z
url: https://github.com/astral-sh/ruff/issues/1536
synced_at: 2026-01-12T15:54:41Z
```

# Adding Codespell

---

_@colin99d_

It would be cool to add codespell to our CI. Let me know if you want this done. (Also its a fairly simple python linter, so we could add it to ruff itself eventually, but that is probably out of scope for now).

---

_Comment by @not-my-profile on 2023-01-02 13:24_

What is codespell?

---

_Comment by @colin99d on 2023-01-02 13:26_

It is a linter written in python that has a mapping of common misspellings, and maps it to correct spellings of words. So it pretty much checks your code for spelling errors. 
https://github.com/codespell-project/codespell

---

_Comment by @colin99d on 2023-01-02 13:27_

Most of the work is just the text files they maintain, the code itself is 1,200 lines of python total:
https://github.com/codespell-project/codespell/blob/master/codespell_lib/_codespell.py

---

_Comment by @not-my-profile on 2023-01-02 14:34_

Codespell apparently doesn't support CamelCase (https://github.com/codespell-project/codespell/issues/196), so I don't think using it on a Rust codebase makes much sense given how prevalent CamelCase is in Rust.

---

_Comment by @colin99d on 2023-01-02 14:39_

The real benefits here are for messages to users, so that we avoid embarrassing misspellings. This means that this is a pretty small negative. However, if you have a better library to use I am all for it.

---

_Closed by @charliermarsh on 2023-01-02 20:58_

---
