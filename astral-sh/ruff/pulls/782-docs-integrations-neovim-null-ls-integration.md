```yaml
number: 782
title: "docs(integrations): neovim `null-ls` integration"
type: pull_request
state: merged
author: eddiebergman
labels: []
assignees: []
merged: true
base: main
head: main
created_at: 2022-11-17T08:26:25Z
updated_at: 2022-11-17T16:55:09Z
url: https://github.com/astral-sh/ruff/pull/782
synced_at: 2026-01-12T05:48:45Z
```

# docs(integrations): neovim `null-ls` integration

---

_Pull request opened by @eddiebergman on 2022-11-17 08:26_

This PR just adds some documentation on how to integrate `ruff` with `null-ls` which is a handy plugin in `neovim` for integrating diagnostic and formatting tools and expose them as an LSP to neovim.

I had some difficulty making the `--fix` directive work through this as `null-ls` cleanly separates diagnostic from formatting tools but I thought it would be worth documenting a work-around.

Please feel free to close this PR if you think this is out of scope!

Also, blazingly fast is an understatement, keep up the good work :)

---

_Comment by @charliermarsh on 2022-11-17 16:54_

Sweet! Makes sense to me.

---

_Merged by @charliermarsh on 2022-11-17 16:55_

---

_Closed by @charliermarsh on 2022-11-17 16:55_

---
