```yaml
number: 1680
title: "Replace `toml` with `toml_edit`"
type: pull_request
state: merged
author: messense
labels: []
assignees: []
merged: true
base: main
head: toml_edit
created_at: 2023-01-06T02:47:51Z
updated_at: 2023-01-06T03:09:16Z
url: https://github.com/astral-sh/ruff/pull/1680
synced_at: 2026-01-12T05:36:32Z
```

# Replace `toml` with `toml_edit`

---

_Pull request opened by @messense on 2023-01-06 02:47_

The `toml` crate doesn't support TOML 1.0, but `toml_edit` does. While there is a plan to [migrate `toml` to be on `toml_edit`](https://github.com/toml-rs/toml/issues/340), it's not ready yet and it's very easy to switch back to `toml` when it's ready.

---

_Merged by @charliermarsh on 2023-01-06 03:08_

---

_Closed by @charliermarsh on 2023-01-06 03:08_

---

_Branch deleted on 2023-01-06 03:09_

---
