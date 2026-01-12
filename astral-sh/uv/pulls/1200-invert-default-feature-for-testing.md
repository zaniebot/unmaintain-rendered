```yaml
number: 1200
title: Invert default feature for testing
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/invert
created_at: 2024-01-31T00:42:17Z
updated_at: 2024-01-31T14:44:28Z
url: https://github.com/astral-sh/uv/pull/1200
synced_at: 2026-01-12T16:04:30Z
```

# Invert default feature for testing

---

_@charliermarsh_

## Summary

We have some flags in Puffin that enable us to opt-in to certain tests. To date, they've been opt-in, so we've run our tests with `--all-features`. This PR makes them opt-out, and we now run tests with default features.

The main motivation here is that I want to get tests working for macOS on CI, but for unknown reasons, macOS can't compile the PyO3 features at the same time as everything else due to strange linker issues. By avoiding `--all-features` for tests, we thus avoid unnecessarily including features that we don't actually use in Puffin.

I verified that the exact same number of tests (439) are run before and after this change. For users, the primary difference is that you now need to specify `--no-default-features --features pypi --features python` to avoid (e.g.) including the Git tests.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-01-31 01:19_

---

_Review requested from @zanieb by @charliermarsh on 2024-01-31 01:19_

---

_Review requested from @konstin by @charliermarsh on 2024-01-31 01:19_

---

_Marked ready for review by @charliermarsh on 2024-01-31 01:26_

---

_@BurntSushi approved on 2024-01-31 02:06_

I think I'm sold. I like that `cargo test` will just do the thing you'll usually want it to do.

---

_@konstin approved on 2024-01-31 12:27_

---

_@zanieb approved on 2024-01-31 14:42_

I was wondering if there was a way we could make `--all-features` less bad...

---

_Merged by @charliermarsh on 2024-01-31 14:44_

---

_Closed by @charliermarsh on 2024-01-31 14:44_

---

_Branch deleted on 2024-01-31 14:44_

---
