```yaml
number: 4174
title: Ignore tags in universal resolution
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/tags
created_at: 2024-06-09T23:24:33Z
updated_at: 2024-06-10T13:42:40Z
url: https://github.com/astral-sh/uv/pull/4174
synced_at: 2026-01-12T16:06:04Z
```

# Ignore tags in universal resolution

---

_@charliermarsh_

## Summary

If a package lacks a source distribution, and we can't find a compatible wheel for the current platform, we need to just _assume_ that the package will have a valid wheel on all platforms on which it's requested; if not, we raise an error at install time.

It's possible that we can be smarter about this over time. For example, if the package was requested _only_ for macOS, we could verify that there's at least one macOS-compatible wheel. See the linked issue for more details.

Closes https://github.com/astral-sh/uv/issues/4139.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-06-09 23:24_

---

_Review requested from @konstin by @charliermarsh on 2024-06-09 23:24_

---

_Label `preview` added by @charliermarsh on 2024-06-09 23:28_

---

_@konstin approved on 2024-06-10 08:17_

---

_Merged by @charliermarsh on 2024-06-10 12:38_

---

_Closed by @charliermarsh on 2024-06-10 12:38_

---

_Branch deleted on 2024-06-10 12:38_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/flat_index.rs`:172 on 2024-06-10 13:32_

Nit: I think this could be `tags.map(|tags| match filename.compatibility(tags) { ... })`.

The tip-off was the `None => None` branch below.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/version_map.rs`:554 on 2024-06-10 13:33_

Nit: same as above, although I think this needs `self.tags.as_ref().map(...)`.

---

_@BurntSushi reviewed on 2024-06-10 13:33_

Nice!

---

_@charliermarsh reviewed on 2024-06-10 13:37_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/flat_index.rs`:172 on 2024-06-10 13:37_

I thought so too initially but we have an early return on line 174... Doesn't that cause problems?

---

_@BurntSushi reviewed on 2024-06-10 13:42_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/flat_index.rs`:172 on 2024-06-10 13:42_

Oh derp, I missed the `return`. Sorry!

---
