```yaml
number: 2117
title: Allow flagging of multiline implicit string concatenations
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/implicit
created_at: 2023-01-24T04:51:08Z
updated_at: 2023-01-24T05:01:02Z
url: https://github.com/astral-sh/ruff/pull/2117
synced_at: 2026-01-12T15:55:07Z
```

# Allow flagging of multiline implicit string concatenations

---

_@charliermarsh_

At present, `ISC001` and `ISC002` flag concatenations like the following:

```py
"a" "b"  # ISC001
"a" \
  "b"  # ISC002
```

However, multiline concatenations are allowed.

This PR adds a setting:

```toml
[tool.ruff.flake8-implicit-str-concat]
allow-multiline = false
```

Which extends `ISC002` to _also_ flag multiline concatenations, like:

```py
(
  "a"  # ISC002
  "b"
)
```

Note that this is backwards compatible, as `allow-multiline` defaults to `true`.


---

_Merged by @charliermarsh on 2023-01-24 05:01_

---

_Closed by @charliermarsh on 2023-01-24 05:01_

---

_Branch deleted on 2023-01-24 05:01_

---
