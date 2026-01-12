```yaml
number: 719
title: Add support for HTML indexes
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/html
created_at: 2023-12-24T15:24:35Z
updated_at: 2023-12-26T14:05:07Z
url: https://github.com/astral-sh/uv/pull/719
synced_at: 2026-01-12T16:04:08Z
```

# Add support for HTML indexes

---

_@charliermarsh_

## Summary

This PR adds support for HTML index responses (as with `--index-url=https://download.pytorch.org/whl`).

Closes https://github.com/astral-sh/puffin/issues/412.


---

_Label `enhancement` added by @charliermarsh on 2023-12-24 15:24_

---

_Merged by @charliermarsh on 2023-12-24 16:04_

---

_Closed by @charliermarsh on 2023-12-24 16:04_

---

_Branch deleted on 2023-12-24 16:04_

---

_Comment by @zanieb on 2023-12-24 16:43_

Why the rush? :p

---

_Comment by @charliermarsh on 2023-12-24 17:03_

I didn’t assume anyone would be reviewing anything for the next week and I don’t want PRs to sit, so erring on the side of merging. If anyone wants to review they are welcome and I will fix in follow-up commits.

---

_Review comment by @konstin on `crates/puffin-client/src/error.rs`:96 on 2023-12-26 13:01_

This should tell the user which content types we support

---

_Review comment by @konstin on `crates/puffin-client/src/html.rs`:15 on 2023-12-26 13:02_

This should also show the input string, `url::ParseError` is a value-less enum

---

_Review comment by @konstin on `crates/puffin-client/src/html.rs`:38 on 2023-12-26 13:06_

This needs an unwrap-safety comment

---

_Review comment by @konstin on `crates/puffin-client/src/html.rs`:126 on 2023-12-26 13:20_

I don't think we can make hashes mandatory, in PEP 503 hashes are a _SHOULD_.

---

_Review comment by @konstin on `crates/puffin-client/src/registry_client.rs`:506 on 2023-12-26 13:31_

i'd `q=0.2` the `text/html` too

---

_@konstin reviewed on 2023-12-26 13:31_

nice work!

---

_@charliermarsh reviewed on 2023-12-26 13:57_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/registry_client.rs`:506 on 2023-12-26 13:57_

Added in the relative URLs PR but I’ll carve it out into a separate change if that doesn’t merge soon.

---

_@charliermarsh reviewed on 2023-12-26 13:58_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/html.rs`:126 on 2023-12-26 13:58_

Agreed, but they’re currently required everywhere else.

---

_Comment by @charliermarsh on 2023-12-26 14:05_

Thanks for the nice review!

---
