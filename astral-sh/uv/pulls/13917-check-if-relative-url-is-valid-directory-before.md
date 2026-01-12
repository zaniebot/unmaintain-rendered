```yaml
number: 13917
title: Check if relative URL is valid directory before treating as index
type: pull_request
state: merged
author: jtfmumm
labels:
  - bug
assignees: []
merged: true
base: main
head: jtfm/index-local-directory
created_at: 2025-06-09T14:26:42Z
updated_at: 2025-06-09T17:28:40Z
url: https://github.com/astral-sh/uv/pull/13917
synced_at: 2026-01-12T16:10:55Z
```

# Check if relative URL is valid directory before treating as index

---

_@jtfmumm_

As per #13874, passing a relative URL like `test` to `--index` for `uv add` causes unexpected behavior if the directory does not exist. The non-existent index is effectively ignored and uv falls back to PyPI. If a package is found there, the spurious index is then written to `pyproject.toml`. This doesn't happen for `--default-index` since resolution will fail without fallback to PyPI.

This PR adds a validation step for indexes provided on the command line. If a directory does not exist, uv will fail with an error.

Closes #13874 

---

_Label `bug` added by @jtfmumm on 2025-06-09 14:26_

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:9347 on 2025-06-09 14:31_

Can you add a test case where an index named `test` exists in the project? We'll probably want to follow this up with a hint dedicated to that case.

---

_@zanieb reviewed on 2025-06-09 14:31_

---

_@jtfmumm reviewed on 2025-06-09 14:37_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/edit.rs`:9347 on 2025-06-09 14:37_

I've added a case with an existing relative path index and will add your suggested test next

---

_@jtfmumm reviewed on 2025-06-09 14:40_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/edit.rs`:9347 on 2025-06-09 14:40_

Added

---

_Marked ready for review by @jtfmumm on 2025-06-09 14:55_

---

_@zanieb reviewed on 2025-06-09 15:54_

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:9395 on 2025-06-09 15:54_

Let's open an issue to improve this message

---

_@zanieb reviewed on 2025-06-09 15:54_

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:9330 on 2025-06-09 15:54_

Let's open an issue to warn if `test-index` exists but is an empty directory?

---

_@zanieb approved on 2025-06-09 15:54_

---

_@zanieb reviewed on 2025-06-09 15:56_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:482 on 2025-06-09 15:56_

Is there no test coverage for this?

---

_@zanieb reviewed on 2025-06-09 15:57_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:482 on 2025-06-09 15:57_

When would this fail? Shouldn't we display the URL?

---

_@jtfmumm reviewed on 2025-06-09 16:18_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/add.rs`:482 on 2025-06-09 16:18_

This would fail for reasons like invalid encoding or an unexpected hostname. I don't think it will fail in practice because we have already parsed the input to create the `VerbatimUrl` and `IndexUrl::Path`. But I've added displaying the URL. 

---

_@jtfmumm reviewed on 2025-06-09 16:23_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/edit.rs`:9330 on 2025-06-09 16:23_

#13922

---

_@jtfmumm reviewed on 2025-06-09 16:24_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/edit.rs`:9395 on 2025-06-09 16:24_

#13921

---

_Merged by @jtfmumm on 2025-06-09 17:28_

---

_Closed by @jtfmumm on 2025-06-09 17:28_

---

_Branch deleted on 2025-06-09 17:28_

---
