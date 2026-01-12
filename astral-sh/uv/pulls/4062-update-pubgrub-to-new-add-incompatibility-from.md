```yaml
number: 4062
title: "Update pubgrub to new `add_incompatibility_from_dependencies`"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/update-pubgrub-dp-impl
created_at: 2024-06-05T18:35:13Z
updated_at: 2024-06-05T18:46:01Z
url: https://github.com/astral-sh/uv/pull/4062
synced_at: 2026-01-12T16:06:01Z
```

# Update pubgrub to new `add_incompatibility_from_dependencies`

---

_@konstin_

We had previously changed the signature of `DependencyProvider::get_dependencies` to return an iterator instead of a hashmap to avoid the conversion cost from our dependencies `Vec` to the pubgrub's hashmap. These changes are difficult to make in pubgrub since they complicate the public api. But we don't actually use `DependencyProvider::get_dependencies`, so we rolled those customizations back in https://github.com/pubgrub-rs/pubgrub/pull/226 and instead opted to change only the internal `add_incompatibility_from_dependencies` method that we exposed in our fork. This aligns us closer with upstream, removes the design questions about `DependencyProvider` from our concerns and reduces our diff (not counting the github action) to +36 -12.

---

_Merged by @konstin on 2024-06-05 18:46_

---

_Closed by @konstin on 2024-06-05 18:46_

---

_Branch deleted on 2024-06-05 18:46_

---
