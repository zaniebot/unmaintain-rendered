```yaml
number: 4489
title: Implement settings conversion traits
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/convert-settings
created_at: 2024-06-24T19:56:45Z
updated_at: 2024-06-24T20:08:13Z
url: https://github.com/astral-sh/uv/pull/4489
synced_at: 2026-01-10T13:48:28Z
```

# Implement settings conversion traits

---

_Pull request opened by @charliermarsh on 2024-06-24 19:56_

## Summary

This does require cloning the settings, but I think it's fine. A better solution would be to have owned and unowned settings structs, so that we could convert `ResolverInstallerSettingsRef` to `InstallerSettingsRef` without cloning, but that requires maintaining owned and unowned variants.

Closes https://github.com/astral-sh/uv/issues/4455.


---

_Comment by @charliermarsh on 2024-06-24 20:06_

https://github.com/astral-sh/uv/pull/4490 adds the thing I mention at the end, removing any clones.

---

_Label `internal` added by @charliermarsh on 2024-06-24 20:06_

---

_Merged by @charliermarsh on 2024-06-24 20:08_

---

_Closed by @charliermarsh on 2024-06-24 20:08_

---

_Branch deleted on 2024-06-24 20:08_

---
