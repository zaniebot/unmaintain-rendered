```yaml
number: 589
title: "Enable selective cache purging in `puffin clean`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/purge
created_at: 2023-12-08T01:55:03Z
updated_at: 2023-12-08T19:51:33Z
url: https://github.com/astral-sh/uv/pull/589
synced_at: 2026-01-10T15:44:44Z
```

# Enable selective cache purging in `puffin clean`

---

_Pull request opened by @charliermarsh on 2023-12-08 01:55_

## Summary

This PR enables `puffin clean` to accept package names as command line arguments, and selectively purge entries from the cache tied to the given package.

Relate to #572.

## Test Plan

Modified all the caching tests to run an additional step to (1) purge the cache, and (2) re-install the package.

---

_Label `enhancement` added by @charliermarsh on 2023-12-08 01:55_

---

_Marked ready for review by @charliermarsh on 2023-12-08 02:10_

---

_Review requested from @konstin by @charliermarsh on 2023-12-08 02:14_

---

_Review requested from @zanieb by @charliermarsh on 2023-12-08 02:14_

---

_Review comment by @konstin on `crates/puffin-cache/src/lib.rs`:442 on 2023-12-08 08:26_

This captures non-index wheels, but not alternative indices. This is currently not a problem will the lack of html index support, but we need an issue to add that with html indices or we'll forget

---

_Review comment by @konstin on `crates/puffin-cache/src/lib.rs`:511 on 2023-12-08 08:27_

I we also need to delete git builds with that name so the wheel is rebuilt, otherwise this confusing for the user (While the source doesn't change, the host environment may have, and the user may still want a rebuild)

---

_Review comment by @konstin on `crates/puffin-cli/src/commands/clean.rs`:52 on 2023-12-08 08:31_

nit: Why don't we `remove_dir_all` the whole cache dir?

---

_Review comment by @konstin on `crates/puffin-cli/src/commands/clean.rs`:54 on 2023-12-08 08:35_

nit: I'd just go with `"Cleared {count} cache entries for package: {}"` or `"Cleared {count} cache(s) for package {}"`

---

_Review comment by @konstin on `crates/puffin-distribution/src/distribution_database.rs`:166 on 2023-12-08 08:36_

Commented code

---

_Review comment by @konstin on `crates/puffin-fs/src/lib.rs`:65 on 2023-12-08 08:40_

nit: We can skip the stat call by trying `remove_dir_all` and switching to `remove_file` if the error is a `ErrorKind::NotADirectory`

---

_Review comment by @konstin on `crates/puffin-cache/src/lib.rs`:428 on 2023-12-08 08:44_

Do we want to add debug logging here which dirs were removed? I expect we'll need that when debugging strange behaviors concerning builds and caching in the future

---

_@konstin approved on 2023-12-08 08:47_

Tried to split my review into required and optional changes

---

_@charliermarsh reviewed on 2023-12-08 16:44_

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/lib.rs`:442 on 2023-12-08 16:44_

I think we store alternative indices in this manner right now. It seems odd to me, maybe it was a mistake in a refactor somewhere.

---

_@konstin reviewed on 2023-12-08 17:09_

---

_Review comment by @konstin on `crates/puffin-cache/src/lib.rs`:442 on 2023-12-08 17:09_

They use `index` and not `url` as root though

![image](https://github.com/astral-sh/puffin/assets/6826232/a29dd335-c13f-4604-8741-2d067739ab1c)

https://github.com/astral-sh/puffin/blob/9806901a1603d062c70c14e523ae9058bdb32675/crates/puffin-cache/src/wheel.rs#L31

---

_@charliermarsh reviewed on 2023-12-08 17:42_

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/lib.rs`:442 on 2023-12-08 17:42_

I thought I tested this, thanks.

---

_@charliermarsh reviewed on 2023-12-08 18:44_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/clean.rs`:52 on 2023-12-08 18:44_

I honestly don't know. I'll change it.

---

_@charliermarsh reviewed on 2023-12-08 18:44_

---

_Review comment by @charliermarsh on `crates/puffin-fs/src/lib.rs`:65 on 2023-12-08 18:44_

I think `NotADirectory` is unstable :/

---

_Merged by @charliermarsh on 2023-12-08 19:51_

---

_Closed by @charliermarsh on 2023-12-08 19:51_

---

_Branch deleted on 2023-12-08 19:51_

---
