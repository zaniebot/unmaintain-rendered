```yaml
number: 3929
title: "Avoid filtering preferences by `--reinstall`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/filter
created_at: 2024-05-30T20:08:09Z
updated_at: 2024-05-30T20:19:10Z
url: https://github.com/astral-sh/uv/pull/3929
synced_at: 2026-01-10T13:59:34Z
```

# Avoid filtering preferences by `--reinstall`

---

_Pull request opened by @charliermarsh on 2024-05-30 20:08_

## Summary

In general, it's not quite right to filter preferences by `--reinstall` -- we still want to respect existing versions, we just don't want to respect _installed_ versions. But now that the installed versions and preferences are decoupled, we can remove this (`--reinstall` is enforced on the installed versions via the `Exclusions` struct that we pass to the resolver).

While I was here, I also cleaned up the lockfile preference code to better match the structure for `requirements.txt`.


---

_Label `internal` added by @charliermarsh on 2024-05-30 20:09_

---

_Marked ready for review by @charliermarsh on 2024-05-30 20:09_

---

_Merged by @charliermarsh on 2024-05-30 20:19_

---

_Closed by @charliermarsh on 2024-05-30 20:19_

---

_Branch deleted on 2024-05-30 20:19_

---
