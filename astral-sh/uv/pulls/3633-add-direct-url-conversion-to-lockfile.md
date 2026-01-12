```yaml
number: 3633
title: Add direct URL conversion to lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/url-lock-source
created_at: 2024-05-17T03:13:12Z
updated_at: 2024-05-17T21:32:43Z
url: https://github.com/astral-sh/uv/pull/3633
synced_at: 2026-01-12T16:05:45Z
```

# Add direct URL conversion to lockfile

---

_@charliermarsh_

## Summary

Similar to #3630, but for direct URL distributions (for both wheels and source distributions).


---

_Label `preview` added by @charliermarsh on 2024-05-17 03:13_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-05-17 03:13_

---

_Comment by @charliermarsh on 2024-05-17 03:18_

We might want some dedicated `LockUrl` types here to facilitate all these conversions. It's a lot of conversion code and the expectations around the URLs aren't always clear (i.e., is it the plan "location" URL? or does it include adornments, like the subdirectory?).

---

_@konstin reviewed on 2024-05-17 11:41_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock.rs`:266 on 2024-05-17 11:41_

Shouldn't this be fragment not query?

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:266 on 2024-05-17 13:15_

Not necessarily. This format is internal to the lockfile. We’re using the fragment for the Git SHA.

---

_@charliermarsh reviewed on 2024-05-17 13:15_

---

_@BurntSushi approved on 2024-05-17 14:10_

LGTM! Thank you!

---

_@konstin reviewed on 2024-05-17 14:27_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock.rs`:266 on 2024-05-17 14:27_

Isn't this method the reverse conversion, from lockfile to installable `Dist`?

---

_@charliermarsh reviewed on 2024-05-17 14:28_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:266 on 2024-05-17 14:28_

Yes you're right.

---

_Merged by @charliermarsh on 2024-05-17 21:32_

---

_Closed by @charliermarsh on 2024-05-17 21:32_

---

_Branch deleted on 2024-05-17 21:32_

---
