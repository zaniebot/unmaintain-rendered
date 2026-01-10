```yaml
number: 337
title: "Write `direct_url.json` in wheel installer"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/direct-url
created_at: 2023-11-06T16:27:27Z
updated_at: 2023-11-06T17:09:30Z
url: https://github.com/astral-sh/uv/pull/337
synced_at: 2026-01-10T15:50:28Z
```

# Write `direct_url.json` in wheel installer

---

_Pull request opened by @charliermarsh on 2023-11-06 16:27_

## Summary

This PR just adds the logic in `install-wheel-rs` to write `direct_url.json`. We're not actually taking advantage of it yet (or wiring it through) in Puffin.

Part of https://github.com/astral-sh/puffin/issues/332.

---

_Review requested from @konstin by @charliermarsh on 2023-11-06 16:27_

---

_Review comment by @konstin on `crates/gourgeist/src/packages.rs`:54 on 2023-11-06 16:37_

I think we should drop this venv setup method, we have a proper way of installing packages into a venv now; It's technically nice that gourgeist could be independent but the synergies with puffin as a whole outweigh this

---

_Review comment by @konstin on `crates/install-wheel-rs/src/direct_url.rs`:21 on 2023-11-06 16:41_

```suggestion
    /// {"url": "https://github.com/pallets/flask.git", "vcs_info": {"commit_id": "8d9519df093864ff90ca446d4af2dc8facd3c542", "vcs": "git"}}
```

---

_Review comment by @konstin on `crates/install-wheel-rs/src/lib.rs`:35 on 2023-11-06 16:42_

```suggestion
    #[error("Invalid direct_url.json")]
```
Otherwise it's really hard to figure out what failed when seeing the serde error

---

_@konstin approved on 2023-11-06 16:43_

---

_@charliermarsh reviewed on 2023-11-06 17:04_

---

_Review comment by @charliermarsh on `crates/gourgeist/src/packages.rs`:54 on 2023-11-06 17:04_

Sounds good, I'll do it in a separate PR.

---

_Merged by @charliermarsh on 2023-11-06 17:09_

---

_Closed by @charliermarsh on 2023-11-06 17:09_

---

_Branch deleted on 2023-11-06 17:09_

---
