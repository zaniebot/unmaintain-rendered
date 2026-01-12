```yaml
number: 2978
title: "Add support for file-scoped `noqa` directives"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/noqa
created_at: 2023-02-17T01:09:22Z
updated_at: 2023-02-17T01:59:02Z
url: https://github.com/astral-sh/ruff/pull/2978
synced_at: 2026-01-12T15:55:12Z
```

# Add support for file-scoped `noqa` directives

---

_@charliermarsh_

# Summary

This allows users to do things like:

```py
# ruff: noqa: F401
```

...to ignore all `F401` directives in a file. It's equivalent to `per-file-ignores`, but allows users to specify the behavior inline.

Note that Flake8 does _not_ support this, so we _don't_ respect `# flake8: noqa: F401`. (Flake8 treats that as equivalent to `# flake8: noqa`, so ignores _all_ errors in the file. I think all of [these usages](https://cs.github.com/?scopeName=All+repos&scope=&q=%22%23+flake8%3A+noqa%3A+%22) are probably mistakes!)

A couple notes on the details:

- If a user has `# ruff: noqa: F401` in the file, but also `# noqa: F401` on a line that would legitimately trigger an `F401` violation, we _do_ mark that as "unused" for `RUF100` purposes. This may be the wrong choice. The `noqa` is legitimately unused, but it's also not "wrong". It's just redundant.
- If a user has `# ruff: noqa: F401`, and runs `--add-noqa`, we _won't_ add `# noqa: F401` to any lines (which seems like the obvious right choice to me).

Closes #1054 (which has some extra pieces that I'll carve out into a separate issue).

Closes #2446.


---

_Merged by @charliermarsh on 2023-02-17 01:59_

---

_Closed by @charliermarsh on 2023-02-17 01:59_

---

_Branch deleted on 2023-02-17 01:59_

---
