```yaml
number: 811
title: Enable customization of autofixable error codes
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/fix-exclude
created_at: 2022-11-18T23:39:03Z
updated_at: 2022-11-19T04:19:04Z
url: https://github.com/astral-sh/ruff/pull/811
synced_at: 2026-01-12T15:55:05Z
```

# Enable customization of autofixable error codes

---

_@charliermarsh_

You can now specify errors that should be considered `fixable` and `unfixable`. The semantics are identical to `select` and `ignore.

For example, to disable fixing `F821`:

```toml
[tools.ruff]
unfixable = ["F821"]
```

Or, from the command line:
```
ruff --fix --unfixable F821 /path/to/file.py
```

By default, all codes are allowed, and none are omitted.

Resolves #786.


---

_Merged by @charliermarsh on 2022-11-18 23:49_

---

_Closed by @charliermarsh on 2022-11-18 23:49_

---

_Branch deleted on 2022-11-18 23:49_

---

_Comment by @JonathanPlasse on 2022-11-19 04:19_

What about unfixable per files?

---
