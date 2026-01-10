```yaml
number: 14245
title: Normalize index URLs to remove trailing slash
type: pull_request
state: merged
author: jtfmumm
labels:
  - bug
assignees: []
merged: true
base: main
head: jtfm/normalize-index-url-trailing-slash
created_at: 2025-06-24T16:17:32Z
updated_at: 2025-06-27T15:11:23Z
url: https://github.com/astral-sh/uv/pull/14245
synced_at: 2026-01-10T07:01:23Z
```

# Normalize index URLs to remove trailing slash

---

_Pull request opened by @jtfmumm on 2025-06-24 16:17_

This PR updates `IndexUrl` parsing to normalize non-file URLs by removing trailing slashes. It also normalizes registry source URLs when using them to validate the lockfile.

Prior to this change, when writing an index URL to the lockfile, uv would use a trailing slash if present in the provided URL and no trailing slash otherwise. This can cause surprising behavior. For example, `uv lock --locked` will fail when a package is added with an `--index` value without a trailing slash and then `uv lock --locked` is run with a `pyproject.toml` version of the index URL that contains a trailing slash. This PR fixes this and adds a test for the scenario.

It might be safe to normalize file URLs in the same way, but since slashes have a well-defined meaning in the context of files and directories, I chose not to normalize them here.

Closes #13707.


---

_Label `bug` added by @jtfmumm on 2025-06-24 16:17_

---

_@jtfmumm reviewed on 2025-06-24 16:20_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/lock.rs`:28264 on 2025-06-24 16:20_

Here's the new test

---

_Review requested from @charliermarsh by @konstin on 2025-06-24 18:59_

---

_@konstin approved on 2025-06-24 19:01_

---

_Comment by @charliermarsh on 2025-06-24 19:33_

In the "Is this lockfile up-to-date check?", should we also strip trailing slashes?

---

_Comment by @jtfmumm on 2025-06-25 08:50_

> In the "Is this lockfile up-to-date check?", should we also strip trailing slashes?

Updated and added a test for a pre-existing lockfile with trailing slashes in registry sources.

---

_@charliermarsh reviewed on 2025-06-27 14:28_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/index_url.rs`:46 on 2025-06-27 14:28_

Could this be `Some(Scheme::file)`, instead of the nested `if`?

---

_@charliermarsh approved on 2025-06-27 14:30_

---

_@charliermarsh reviewed on 2025-06-27 14:31_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/file.rs`:182 on 2025-06-27 14:31_

I think this and `without_fragment` could both return `Cow`? But that can be a separate PR if you want to try it.

---

_@jtfmumm reviewed on 2025-06-27 14:38_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/file.rs`:182 on 2025-06-27 14:38_

Makes sense to me. I'll open as a separate PR

---

_@jtfmumm reviewed on 2025-06-27 15:08_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index_url.rs`:46 on 2025-06-27 15:08_

That was what I initially wanted to do, but there numerous other `Scheme` variants for things like `bzr+file`

---

_Merged by @jtfmumm on 2025-06-27 15:11_

---

_Closed by @jtfmumm on 2025-06-27 15:11_

---

_Branch deleted on 2025-06-27 15:11_

---
