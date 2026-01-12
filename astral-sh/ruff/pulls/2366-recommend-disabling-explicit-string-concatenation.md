```yaml
number: 2366
title: "Recommend disabling `explicit-string-concatenation`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/imp
created_at: 2023-01-30T21:41:46Z
updated_at: 2023-01-30T21:42:31Z
url: https://github.com/astral-sh/ruff/pull/2366
synced_at: 2026-01-12T15:55:08Z
```

# Recommend disabling `explicit-string-concatenation`

---

_@charliermarsh_

If `allow-multiline = false` is set, then if the user enables `explicit-string-concatenation` (`ISC003`), there's no way for them to create valid multiline strings. This PR notes that they should turn off `ISC003`.

Closes #2362.

---

_Label `documentation` added by @charliermarsh on 2023-01-30 21:41_

---

_Merged by @charliermarsh on 2023-01-30 21:42_

---

_Closed by @charliermarsh on 2023-01-30 21:42_

---

_Branch deleted on 2023-01-30 21:42_

---
