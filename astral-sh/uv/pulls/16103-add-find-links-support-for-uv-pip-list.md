```yaml
number: 16103
title: "Add `--find-links` support for `uv pip list`"
type: pull_request
state: open
author: terror
labels: []
assignees: []
base: main
head: pip-list-outdated
created_at: 2025-10-02T15:50:07Z
updated_at: 2025-10-27T06:25:20Z
url: https://github.com/astral-sh/uv/pull/16103
synced_at: 2026-01-12T16:12:06Z
```

# Add `--find-links` support for `uv pip list`

---

_@terror_

Resolves https://github.com/astral-sh/uv/issues/16095

This PR fixes `uv pip list --outdated` so `--find-links` locations impact the latest-version lookup. This wires flat indexes through the registry client even under `--no-index`, lets `LatestClient::find_latest` evaluate the same compatibility filters across both Simple API and flat metadata, and adds an integration test that installs from `scripts/links` to show the higher local release is reported.

---

_@charliermarsh reviewed on 2025-10-03 02:04_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_install.rs`:6193 on 2025-10-03 02:04_

Do you know why these changed?

---

_Review comment by @terror on `crates/uv/tests/it/pip_install.rs`:6193 on 2025-10-03 02:11_

This path used to short-circuit with `ErrorKind::NoIndex`, but now since we're considering flat indexes (via `--find-links`), we end up getting a `UnavailablePackage::NotFound` rendering as 'not found in the package registry' instead.

---

_@terror reviewed on 2025-10-03 02:11_

---

_@terror reviewed on 2025-10-03 02:17_

---

_Review comment by @terror on `crates/uv/tests/it/pip_install.rs`:6193 on 2025-10-03 02:17_

I think this wording is actually a bit confusing, this is implying we did a registry lookup (even though we supplied `--no-index`) as opposed to just looking at the directories/URLs provided by `--find-links`. Something to address for sure.

---

_@terror reviewed on 2025-10-03 15:49_

---

_Review comment by @terror on `crates/uv-distribution-types/src/index_url.rs`:510 on 2025-10-03 15:49_

Was there a reason why we did *not* want to include `flat_indexes` here?

---

_@terror reviewed on 2025-10-03 15:52_

---

_Review comment by @terror on `crates/uv-client/src/registry_client.rs`:416 on 2025-10-03 15:52_

This is a tad awkward, but this is here to keep the wording the same for the [`already_installed_local_path_dependent`](https://github.com/astral-sh/uv/blob/b06f02f13d79e644ffa7d35f38a817d1e32ec68a/crates/uv/tests/it/pip_install.rs#L6102) test.

---

_Marked ready for review by @terror on 2025-10-03 16:02_

---

_Review requested from @charliermarsh by @zanieb on 2025-10-08 13:46_

---

_Review comment by @konstin on `crates/uv/src/commands/pip/latest.rs`:40 on 2025-10-16 21:20_

Can we make this a separate function? You can pass in an `impl Iterator<...>` if that helps

---

_Review comment by @konstin on `crates/uv/tests/it/pip_list.rs`:215 on 2025-10-16 21:22_

We can run this test without the exclude newer env var, those warnings are misleading.

---

_Review comment by @konstin on `crates/uv-client/src/flat_index.rs`:325 on 2025-10-16 21:22_

That seems correct, but it's a behavior change, could you open a separate PR for it?

---

_Review comment by @konstin on `crates/uv-distribution-types/src/index_url.rs`:510 on 2025-10-16 21:29_

I'm not aware of any

---

_@konstin reviewed on 2025-10-16 21:30_

---

_@charliermarsh reviewed on 2025-10-16 21:39_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/index_url.rs`:510 on 2025-10-16 21:39_

Unfortunately I really can't remember. I'm sure there's a reason for it. Probably because historically we didn't query flat indexes in the registry client? Something like that?

---

_@charliermarsh approved on 2025-10-27 02:08_

---

_Comment by @charliermarsh on 2025-10-27 02:21_

It's actually a bit hard for me to reason about how `--find-links` works now, because we create the `FlatIndexClient` and pass that around, but we're now _also_ passing the `flat_indexes` to the registry client... Are they now being indexed _twice_?

---
