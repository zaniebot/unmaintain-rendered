```yaml
number: 7175
title: Respect exclusion when collecting workspace members
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/err
created_at: 2024-09-07T18:32:22Z
updated_at: 2024-09-09T16:08:08Z
url: https://github.com/astral-sh/uv/pull/7175
synced_at: 2026-01-12T16:07:43Z
```

# Respect exclusion when collecting workspace members

---

_@charliermarsh_

## Summary

We were only applying exclusions when discovering the root, apparently.

Our logic now matches the original intent, which is...

- `exclude` always post-filters `members`.
- We don't treat globs any differently than non-globs.

The one confusing setup that falls out of this is that given:

```toml
members = ["foo/bar/baz"]
exclude = ["foo/bar"]
```

`foo/bar/baz` **would** be included. To exclude it, you would need:

```toml
members = ["foo/bar/baz"]
exclude = ["foo/bar/*"]
```

Closes https://github.com/astral-sh/uv/issues/7071.


---

_Label `bug` added by @charliermarsh on 2024-09-07 18:32_

---

_Review requested from @konstin by @charliermarsh on 2024-09-07 18:50_

---

_@konstin approved on 2024-09-09 15:51_

---

_Merged by @charliermarsh on 2024-09-09 16:08_

---

_Closed by @charliermarsh on 2024-09-09 16:08_

---

_Branch deleted on 2024-09-09 16:08_

---
