```yaml
number: 471
title: Add support for path dependencies
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/env-var
created_at: 2023-11-20T23:46:19Z
updated_at: 2023-11-21T14:50:09Z
url: https://github.com/astral-sh/uv/pull/471
synced_at: 2026-01-12T16:03:58Z
```

# Add support for path dependencies

---

_@charliermarsh_

## Summary

This PR adds support for local path dependencies. The approach mostly just falls out of our existing approach and infrastructure for Git and URL dependencies.

Closes https://github.com/astral-sh/puffin/issues/436. (We'll open a separate issue for editable installs.)

## Test Plan

Added `pip-compile` tests that pre-download a wheel or source distribution, then install it via local path.


---

_Review requested from @konstin by @charliermarsh on 2023-11-20 23:46_

---

_Label `enhancement` added by @charliermarsh on 2023-11-20 23:46_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/direct_url.rs`:92 on 2023-11-20 23:48_

@konstin - One weird thing I noticed is that if you install a wheel via `file://`, pip uses the the `ArchiveUrl` format for `DirectUrl`, like:

```json
{"archive_info": {"hash": "sha256=53806937bd3dac4a964cc380d262e18e5687f50125839c18fda3a4b07fdea493", "hashes": {"sha256": "53806937bd3dac4a964cc380d262e18e5687f50125839c18fda3a4b07fdea493"}}, "url": "file:///Users/crmarsh/Library/Caches/puffin/archives-v0/e0248556cde58846/flake8-6.0.0-py2.py3-none-any.whl"}
```

This is surprising to me. I stuck with the `LocalDirectory` pattern:

```json
{"url":"file:///Users/crmarsh/Library/Caches/puffin/archives-v0/e0248556cde58846/flake8-6.0.0-py2.py3-none-any.whl","dir_info":{}}
```

---

_@charliermarsh reviewed on 2023-11-20 23:48_

---

_Comment by @konstin on 2023-11-21 07:06_

> I'm not sure how best to test this.

I'd add a minimal project to the repo, with a single pure dependency (we use `boltons` in maturin), a simple build system such as flit and a single function that returns/prints a string using boltons.

---

_Review comment by @konstin on `crates/distribution-types/src/direct_url.rs`:11 on 2023-11-21 07:08_

The docstring is unclear between path to a wheel file, path to a source dist file and path to a source directory (vs. those being e.g. in archive url or unsupported)

---

_Review comment by @konstin on `crates/puffin-cache/src/cache_key.rs`:201 on 2023-11-21 07:13_

Doesn't rust inline single action function calls anyway?

---

_@konstin approved on 2023-11-21 07:24_

---

_@charliermarsh reviewed on 2023-11-21 11:40_

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/cache_key.rs`:201 on 2023-11-21 11:40_

I assume so but this is copied directly from Ruff so I'll leave it as-is.

---

_Merged by @charliermarsh on 2023-11-21 11:49_

---

_Closed by @charliermarsh on 2023-11-21 11:49_

---

_Branch deleted on 2023-11-21 11:49_

---

_@zanieb reviewed on 2023-11-21 14:50_

LGTM :)

---
